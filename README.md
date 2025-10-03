# Claude Code Configuration

SuperClaudeフレームワーク、カスタムエージェント、スラッシュコマンド、ワークフロー自動化を含む個人用Claude Code設定。

## ディレクトリ構成

```text
.claude/
├── CLAUDE.md              # SuperClaudeフレームワークのエントリーポイント
├── settings.json          # 権限設定とワークフローフック
├── agents/                # カスタムサブエージェント定義
└── commands/              # カスタムスラッシュコマンド
    ├── git/              # Git関連コマンド
    ├── sc/               # SuperClaude統合コマンド
    ├── think/            # 思考深度コマンド
    ├── tsumiki/          # Tsumikiワークフローコマンド
    └── zenn/             # Zenn記事管理コマンド
```

## 導入しているMCPサーバー

- **serena**: セマンティックコード理解とプロジェクトメモリ管理
- **context7**: 公式ライブラリドキュメント検索とパターンガイダンス
- **playwright**: ブラウザ自動化とE2Eテスト
- **sequential-thinking**: 複雑な問題の多段階推論エンジン
- **figma-dev-mode-mcp-server**: Figmaデザインファイルの開発モード連携
- **cipher**: 暗号化・復号化処理
- **chrome-devtools**: Chrome DevToolsプロトコル統合

## セキュリティ設定（settings.json）

### 環境変数

| 変数 | 値 | 説明 |
|------|-----|------|
| CLAUDE_CODE_ENABLE_TELEMETRY | 0 | テレメトリ無効化 |

### 許可コマンド（permissions.allow）

| カテゴリ | コマンド | 用途 |
|---------|---------|------|
| **ファイル操作** | ls, find, rg, cat, mkdir, mv, cp, rm, echo, pwd, cd | 基本的なファイルシステム操作 |
| **Git読取専用** | git status, git diff, git log, git branch, git checkout, git pull, git add | リポジトリ情報取得・準備 |
| **GitHub CLI読取** | gh repo view, gh pr list/view, gh issue list/view | GitHub情報参照 |
| **WebFetch** | github.com, docs.anthropic.com, developer.mozilla.org, nodejs.org, npmjs.com | 信頼ドメインからの情報取得 |

### 拒否コマンド（permissions.deny）

| カテゴリ | 拒否コマンド | 理由 |
|---------|------------|------|
| **破壊的操作** | rm -rf /*, sudo, git reset/rebase/push/commit | システム・リポジトリ保護 |
| **ネットワーク** | curl, wget, nc | 不正アクセス防止 |
| **パッケージ削除** | npm uninstall/remove | 依存関係保護 |
| **DB直接操作** | psql, mysql, mongod, supabase execute_sql | データ保護 |
| **GitHub破壊的操作** | repo delete, auth token, secret/variable set, workflow disable, release delete, API DELETE/PUT/PATCH/POST | GitHubリソース保護 |
| **秘密情報** | id_rsa, id_ed25519, .env*, *token*, *key*, mcp.json | 認証情報保護 |

### フック設定（hooks）

| フック種別 | トリガー | 動作 | 目的 |
|-----------|---------|------|------|
| **Notification** | 許可ダイアログ表示時 | macOS通知（Glass音） | ユーザーへのアラート |
| **Stop** | タスク完了時 | macOS通知（Hero音） | 作業完了通知 |
| **PreToolUse(Edit系)** | Edit/MultiEdit/Write実行前 | .env, package-lock.json, .gitファイルをブロック | 重要ファイル保護 |
| **PreToolUse(Bash)** | Bashコマンド実行前 | 危険なrm/delete操作をブロック | 破壊的操作防止 |
| **SubagentStop** | Subagent終了時 | validation-gates完了チェック | 品質ゲート確認 |
| **UserPromptSubmit** | プロンプト送信時 | テスト関連キーワード検出でリマインダー | テスト実施促進 |

### 常時有効機能

| 機能 | 設定値 | 説明 |
|------|-------|------|
| alwaysThinkingEnabled | true | 常に思考プロセスを表示 |

## 主な機能

### SuperClaude Framework

- **行動モード**: Brainstorming、Introspection、Orchestration、Task Management、Token Efficiency
- **MCPサーバー統合**: Context7、Playwright、Sequential、Serena
- **フラグシステム**: タスク分析、実行制御、出力最適化

### カスタムエージェント

[wshobson/agents](https://github.com/wshobson/agents)から必要な機能を選定・統合

#### 主要エージェント一覧

| カテゴリ | エージェント名 | 説明 | 主な用途 |
|---------|--------------|------|---------|
| **開発系** | python-expert-sc | SOLID原則に従った本番対応Pythonコード生成 | Python開発、最適化、テスト戦略 |
| | backend-architect-wshobson | RESTful API、マイクロサービス設計 | バックエンド設計、DB設計 |
| | frontend-developer-wshobson | React/Vue/Angularコンポーネント実装 | フロントエンド開発 |
| | serena-expert | /serenaコマンド活用による効率的開発 | アプリ開発、API実装、システム構築 |
| **品質系** | code-reviewer-official | コード品質・セキュリティ・保守性レビュー | コードレビュー、品質保証 |
| | debugger-wshobson | エラー・テスト失敗のデバッグ専門 | 根本原因分析、バグ修正 |
| | test-automator-wshobson | unit/integration/E2Eテスト自動化 | テスト戦略、CI/CD統合 |
| **セキュリティ** | security-auditor-wshobson | OWASP準拠、認証実装、脆弱性検出 | セキュリティレビュー、認証フロー |
| **パフォーマンス** | performance-engineer-wshobson | ボトルネック分析、最適化、キャッシング戦略 | 性能改善、負荷テスト |
| **アーキテクチャ** | system-architect-sc | スケーラブルなシステム設計 | 長期的な技術判断、保守性 |
| **ドキュメント** | technical-writer-sc | 技術文書作成、対象読者に合わせた説明 | API文書、ユーザーガイド |
| **特化系** | seo-content-writer-wshobson | SEO最適化コンテンツ作成 | ブログ記事、マーケティング |
| | legal-advisor-wshobson | プライバシーポリシー、利用規約作成 | 法的文書、GDPR対応 |
| | mermaid-expert-wshobson | Mermaid図（フローチャート、ER図）作成 | ビジュアル文書化、設計図 |
| **ルーティング** | intelligent-router-codex | タスク複雑度を評価し最適な実行方法を自動判断 | Codex MCP統合、タスク分析 |

### スラッシュコマンド

#### 汎用ワークフロー

| コマンド | 説明 | 主な機能 |
|---------|------|---------|
| `/bugfix` | バグ修正ワークフロー | バグ理解→Context7/Web Search活用→修正→検証 |
| `/update-claude-md` | CLAUDE.md自動更新 | プロジェクト構造分析→技術スタック識別→ドキュメント更新 |
| `/visual` | ビジュアル駆動開発 | Playwright活用、スクリーンショット比較、UI反復改善 |
| `/serena` | Serena MCP統合問題解決 | トークン効率的な構造化開発（-q:簡易/-d:深層/-s:実装） |

#### SuperClaudeコマンド（/sc:）

| コマンド | 説明 | 活用MCP |
|---------|------|--------|
| `/sc:implement` | 機能実装（ペルソナ・MCP統合） | Context7, Sequential, Magic, Playwright |
| `/sc:analyze` | 包括的コード分析 | Sequential |
| `/sc:brainstorm` | 対話型要件発見 | - |
| `/sc:design` | システム設計・API設計 | Context7 |
| `/sc:test` | テスト実行・カバレッジ分析 | Playwright |
| `/sc:load` | プロジェクトコンテキスト読込 | Serena |
| `/sc:save` | セッション永続化 | Serena |

#### Git統合

| コマンド | 説明 |
|---------|------|
| `/git/create-pr` | プルリクエスト作成 |
| `/git/review-local-changes` | ローカル変更レビュー |
| `/git/fix-github-issue` | GitHub Issue修正 |

#### 思考深度コマンド（/think:）

| コマンド | 思考量 | 用途 |
|---------|--------|-----|
| `/think/think-hard` | 中深度（~10K tokens） | アーキテクチャ分析、依存関係調査 |
| `/think/ultrathink` | 最大深度（~32K tokens） | システム再設計、レガシー移行、複雑デバッグ |

#### TDDワークフロー（/tsumiki:）

| コマンド | フェーズ | 説明 |
|---------|---------|------|
| `/tsumiki/tdd-requirements` | 準備 | 要件整理 |
| `/tsumiki/tdd-testcases` | 準備 | テストケース洗い出し |
| `/tsumiki/tdd-red` | Red | 失敗するテスト作成 |
| `/tsumiki/tdd-green` | Green | テストを通す実装 |
| `/tsumiki/tdd-refactor` | Refactor | コード品質改善 |
| `/tsumiki/tdd-verify-complete` | 完了 | 全テスト通過検証 |

#### Zenn記事管理

| コマンド | 説明 |
|---------|------|
| `/zenn/zenn-new` | 新規記事作成（章立て・テンプレート生成） |
| `/zenn/zenn-review` | 記事品質チェック・改善提案 |
| `/zenn/zenn-title` | タイトル最適化（3パターン提案） |

#### CI/CD統合

| コマンド | 説明 |
|---------|------|
| `/pr_ci_fix` | 現在のブランチからPRを特定、GitHub Actions失敗を調査・修正 |

#### Web/ブラウザテスト（/web:）

| コマンド | 説明 | 主な検証項目 |
|---------|------|-------------|
| `/web/a11y-check` | アクセシビリティ監査 | WCAG 2.1 Level AA準拠、ARIA検証 |
| `/web/console-debug` | コンソールエラー/警告分析 | JavaScriptエラー、ネットワーク警告検出 |
| `/web/form-test` | フォーム機能テスト | バリデーション、送信処理、エラー表示 |
| `/web/mobile-emulate` | モバイルデバイスエミュレート | タッチ操作、センサー、ビューポート |
| `/web/network-analyze` | ネットワークパフォーマンス分析 | リクエスト最適化、ロード時間、リソース効率 |
| `/web/page-audit` | ページ総合監査 | SEO、パフォーマンス、品質評価 |
| `/web/perf-analyze` | パフォーマンス分析 | Core Web Vitals、レンダリング、最適化提案 |
| `/web/responsive-check` | レスポンシブデザイン検証 | 複数デバイスサイズ、レイアウト、タッチターゲット |

## 参考資料

### フレームワーク・エージェント

- [SuperClaude Framework](https://github.com/SuperClaude-Org/SuperClaude_Framework)
- [wshobson/agents](https://github.com/wshobson/agents)
- [サブエージェントの例 - Anthropic公式](https://docs.anthropic.com/ja/docs/claude-code/sub-agents)
- [serena-expertの活用](https://zenn.dev/sc30gsw/articles/ff81891959aaef#2.-sub-agent)
- [Claude Code活用術](https://note.com/taku_sid/n/n9daa1a6ead98)

### MCP導入ガイド

- [Claude CodeでMCPツール（Context7、Serena、Cipher）を活用してAIコーディングを次のレベルへ](https://qiita.com/sukimaengineer/items/845ad14a3ec2d3c39930)
- [Claude Code で Context7、Serena、Cipher を同時使用する際の注意事項](https://qiita.com/sukimaengineer/items/9a0a10d23c86f2a2e850)
- [Claude Code MCP Serena & Cipher 導入ガイド](https://zenn.dev/aki1990/articles/a7db63dbc99848)
- [Claude Code を更に賢く - serena と cipher を mcp で使う](https://zenn.dev/sho7650/articles/5d9b46a119a08f)

## 使い方

1. このリポジトリを`~/.claude/`に配置
2. Claude Codeを起動
3. SuperClaudeフレームワークが自動的に読み込まれる
4. カスタムコマンドは`/`で呼び出し可能

### コマンド使用例

各コマンドの具体的な使い方とシーン別の活用方法については、以下のドキュメントを参照してください：

📖 [コマンド使用例集](claudedocs/command-usage-examples.md)

- 全27コマンドの使用例
- シーン別使い分けガイド
- 実践的なワークフロー例（開発、リリース、問題調査、バグ修正等）

## カスタマイズ

`CLAUDE.md`でフレームワーク設定を調整可能。個人の開発スタイルに合わせて、エージェントやコマンドを追加・削除できます。
