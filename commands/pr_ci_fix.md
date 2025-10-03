---
description: "現在のブランチからPRを特定しGitHub Actionsの失敗を調査・修正"
argument-hint: "[オプション: PR番号 (省略時は現在のブランチから検索)]"
allowed-tools:
  - "Bash(gh pr list:*)"
  - "Bash(gh pr view:*)"
  - "Bash(gh pr checks:*)"
  - "Bash(gh pr diff:*)"
  - "Bash(gh run list:*)"
  - "Bash(gh run view:*)"
  - "Bash(gh workflow run:*)"
  - "Bash(git branch:*)"
  - "Bash(git status:*)"
  - "Bash(dart analyze:*)"
  - "Bash(melos run:*)"
---

現在のブランチまたは指定された PR の GitHub Actions 失敗を調査し、原因を特定して修正を実施するコマンドです。

## 利用方法

```bash
# 現在のブランチからPRを自動検出
/pr_ci_fix

# PR番号を指定
/pr_ci_fix 3220

# PR URLを指定
/pr_ci_fix https://github.com/st-tech/zozomatch-app/pull/3220
```

## 処理の流れ

1. 現在のブランチ名または PR 番号から Pull Request を特定
2. GitHub CLI を使用して PR のチェック状況を取得
3. 失敗しているワークフローとジョブを特定
4. エラーログを詳細に分析
5. 失敗原因を特定して修正案を提示
6. 可能な場合は自動修正を実行（ファイルの変更のみ）
7. 修正結果を表示（コミット・プッシュは手動で実行）

## 実行手順

以下のコマンドを順番に実行してください：

### 1. PR の特定と状況確認

```bash
# 現在のブランチ名を取得
CURRENT_BRANCH=$(git branch --show-current)
echo "現在のブランチ: $CURRENT_BRANCH"

# PRの検索（引数が与えられない場合）
if [ -z "$1" ]; then
  echo "現在のブランチからPRを検索中..."
  PR_INFO=$(gh pr list --head "$CURRENT_BRANCH" --json number,title,url,state --limit 1)

  if [ -z "$PR_INFO" ] || [ "$PR_INFO" = "[]" ]; then
    echo "エラー: 現在のブランチに関連するPRが見つかりません"
    exit 1
  fi

  PR_NUMBER=$(echo "$PR_INFO" | jq -r '.[0].number')
  PR_URL=$(echo "$PR_INFO" | jq -r '.[0].url')
else
  # 引数からPR番号を抽出
  if [[ "$1" =~ ^[0-9]+$ ]]; then
    PR_NUMBER="$1"
  elif [[ "$1" =~ pull/([0-9]+) ]]; then
    PR_NUMBER=$(echo "$1" | grep -oE '[0-9]+' | tail -1)
  else
    echo "エラー: 無効なPR番号またはURL: $1"
    exit 1
  fi
  PR_URL="https://github.com/st-tech/zozomatch-app/pull/$PR_NUMBER"
fi

echo "PR #$PR_NUMBER を調査中..."
echo "URL: $PR_URL"
```

### 2. GitHub Actions のステータス確認

```bash
# PRのチェック状況を取得
echo ""
echo "GitHub Actionsのステータスを確認中..."
gh pr checks "$PR_NUMBER" --watch=false

# 失敗しているチェックの詳細を取得
FAILED_CHECKS=$(gh pr checks "$PR_NUMBER" --json name,state,conclusion,detailsUrl | jq -r '.[] | select(.conclusion == "failure" or .state == "failure") | @json')

if [ -z "$FAILED_CHECKS" ]; then
  echo "✅ すべてのチェックが成功しています"
  exit 0
fi

echo ""
echo "⚠️ 失敗しているチェックが見つかりました:"
echo "$FAILED_CHECKS" | jq -r '. | "- \(.name): \(.conclusion // .state)"'
```

### 3. エラーログの詳細分析

```bash
# 各失敗チェックのログを取得して分析
echo ""
echo "エラーログの詳細を分析中..."

# ワークフローの実行IDを取得
RUN_ID=$(gh run list --branch "$CURRENT_BRANCH" --json databaseId,status,conclusion,workflowName --limit 5 | jq -r '.[] | select(.conclusion == "failure") | .databaseId' | head -1)

if [ -n "$RUN_ID" ]; then
  echo "ワークフロー実行ID: $RUN_ID"

  # 失敗ジョブのログを取得
  echo ""
  echo "失敗したジョブのログを取得中..."
  gh run view "$RUN_ID" --log-failed
fi
```

### 4. 一般的なエラーパターンの検出と修正

```bash
# dart analyzeエラーの検出
if echo "$FAILED_CHECKS" | grep -q "analyze"; then
  echo ""
  echo "📍 Dart Analyzeエラーを検出"
  echo "修正を実行中..."

  # dart analyzeを実行してエラーを確認
  dart analyze

  # 自動修正を適用
  melos run fix
  melos run format

  # 再度analyzeを実行
  dart analyze
fi

# テストエラーの検出
if echo "$FAILED_CHECKS" | grep -q "test"; then
  echo ""
  echo "📍 テストエラーを検出"
  echo "テストを実行中..."

  # テストを実行してエラーを確認
  melos run test:diff:head
fi

# ビルドエラーの検出
if echo "$FAILED_CHECKS" | grep -q "build"; then
  echo ""
  echo "📍 ビルドエラーを検出"
  echo "コード生成を実行中..."

  # コード生成
  melos run gen:diff:head
fi

# ゴールデンテストエラーの検出
if echo "$FAILED_CHECKS" | grep -q "golden"; then
  echo ""
  echo "📍 ゴールデンテストエラーを検出"
  echo "ゴールデンファイルを更新中..."

  # ゴールデンテストの更新
  melos run golden_test
fi
```

### 5. 修正結果の確認

```bash
echo ""
echo "修正を適用しました。変更内容を確認中..."

# 変更されたファイルを確認
git status --short

# 変更がある場合は差分を表示
if [ -n "$(git status --porcelain)" ]; then
  echo ""
  echo "📝 以下の変更が適用されました:"
  git diff --stat

  echo ""
  echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
  echo "✅ 修正が完了しました"
  echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
  echo ""
  echo "修正内容を確認後、必要に応じて手動でコミット・プッシュしてください:"
  echo ""
  echo "  git add -A"
  echo "  git commit -m \"fix: GitHub Actions失敗の修正\""
  echo "  git push"
else
  echo ""
  echo "✅ 修正は必要ありませんでした"
fi
```

### 6. 修正後の再実行提案

```bash
echo ""
echo "🔄 修正をプッシュした後、以下のコマンドでワークフローを再実行できます:"
echo "gh workflow run [workflow-name] --ref $CURRENT_BRANCH"
echo ""
echo "または、PRページから手動で再実行:"
echo "$PR_URL"
```

## 前提条件

- GitHub CLI（gh）がインストールされ、認証済みであること
- git リポジトリ内で実行すること
- 適切な権限（リポジトリへの読み取り権限）があること
- Dart/Flutter 環境がセットアップされていること
- melos がインストールされていること

## エラーハンドリング

以下の場合はエラーメッセージが表示されます：

- 現在のブランチに関連する PR が見つからない場合
- 指定された PR 番号が無効な場合
- GitHub CLI の認証に失敗した場合
- ネットワーク接続に問題がある場合
- 修正に必要な権限がない場合

## 注意事項

- このコマンドは自動修正を試みますが、すべてのエラーを自動修正できるわけではありません
- **修正の適用のみを行い、コミット・プッシュは自動実行しません**
- 修正内容を確認後、必要に応じて手動でコミット・プッシュしてください
- 複雑なエラーの場合は、手動での修正が必要になることがあります
- 修正後は必ずコードレビューを受けてください
- プライベートリポジトリでの使用には適切な権限が必要です
- 大規模な変更が必要な場合は、チームメンバーと相談することをお勧めします
