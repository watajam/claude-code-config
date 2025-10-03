# Claude Code コマンド使用例

## Web/ブラウザテストコマンド

### 1. パフォーマンス分析 `/web/perf-analyze`

```bash
# 本番サイトのパフォーマンスチェック
/web/perf-analyze https://example.com

# ローカル開発環境をチェック
/web/perf-analyze http://localhost:3000

# 特定のページをチェック
/web/perf-analyze http://localhost:3000/products/123
```

**主な分析項目**:

- Core Web Vitals（LCP、FID、CLS）
- レンダリングパフォーマンス
- リソース読み込み時間
- 最適化提案

---

### 2. アクセシビリティチェック `/web/a11y-check`

```bash
# トップページをチェック
/web/a11y-check https://example.com

# ログインページをチェック
/web/a11y-check http://localhost:3000/login

# ダッシュボードをチェック
/web/a11y-check http://localhost:3000/dashboard
```

**検証項目**:

- WCAG 2.1 Level AA 準拠
- ARIA 属性の妥当性
- キーボードナビゲーション
- 色コントラスト比
- 代替テキスト

---

### 3. コンソールエラー調査 `/web/console-debug`

```bash
# ページを開いてエラーチェック
/web/console-debug http://localhost:3000

# SPAの特定ページ
/web/console-debug http://localhost:3000/dashboard

# エラーが出ているページを調査
/web/console-debug https://example.com/error-page
```

**検出内容**:

- JavaScript エラー
- ネットワーク警告
- セキュリティ警告
- 非推奨 API の使用

---

### 4. レスポンシブデザインチェック `/web/responsive-check`

```bash
# トップページの表示確認
/web/responsive-check https://example.com

# ランディングページ確認
/web/responsive-check http://localhost:3000/lp/campaign

# ダッシュボードのレイアウト確認
/web/responsive-check http://localhost:3000/dashboard
```

**テスト対象**:

- モバイル（375x812、812x375）
- タブレット（768x1024、1024x768）
- デスクトップ（1920x1080、2560x1440）
- タッチターゲットサイズ
- コンテンツオーバーフロー

---

### 5. ネットワーク分析 `/web/network-analyze`

```bash
# サイト全体のファイル読み込みを分析
/web/network-analyze https://example.com

# 画像が多いページを分析
/web/network-analyze http://localhost:3000/gallery

# API呼び出しが多いページを分析
/web/network-analyze http://localhost:3000/dashboard
```

**分析内容**:

- リクエスト数と総容量
- ロード時間の詳細
- リソース効率
- 最適化の余地

---

### 6. フォームテスト `/web/form-test`

```bash
# お問い合わせフォームをテスト
/web/form-test http://localhost:3000/contact

# 会員登録フォームをテスト
/web/form-test http://localhost:3000/signup

# ログインフォームをテスト
/web/form-test http://localhost:3000/login
```

**検証項目**:

- バリデーション動作
- 送信処理
- エラーメッセージ表示
- 必須項目チェック

---

### 7. モバイルエミュレーション `/web/mobile-emulate`

```bash
# サイト全体をモバイルで体験
/web/mobile-emulate https://example.com

# ローカル開発中のページをテスト
/web/mobile-emulate http://localhost:3000

# ECサイトの商品ページをテスト
/web/mobile-emulate http://localhost:3000/products/123
```

**エミュレート機能**:

- タッチ操作
- センサー（位置情報、加速度等）
- ビューポート設定
- ネットワーク速度制限

---

### 8. フルページ監査 `/web/page-audit`

```bash
# サイト全体を総合診断
/web/page-audit https://example.com

# リリース前の最終チェック
/web/page-audit http://localhost:3000

# 特定のページを詳細診断
/web/page-audit http://localhost:3000/important-page
```

**監査項目**:

- SEO 最適化
- パフォーマンス
- アクセシビリティ
- ベストプラクティス
- 総合品質評価

---

## CI/CD 統合コマンド

### 9. PR CI 修正 `/pr_ci_fix`

```bash
# 現在のブランチのPRを自動検出して修正
/pr_ci_fix

# PR番号を指定して修正
/pr_ci_fix 3220

# PR URLを指定して修正
/pr_ci_fix https://github.com/owner/repo/pull/3220
```

**実行内容**:

1. GitHub Actions の CI 失敗を検出
2. エラーログを分析
3. 自動修正を適用（dart analyze、テスト、ビルド、golden test）
4. 修正結果を表示（コミット・プッシュは手動）

**対応エラー**:

- dart analyze エラー
- テスト失敗
- ビルドエラー
- ゴールデンテストの差分

---

## SuperClaude 統合コマンド

### 10. 機能実装 `/sc:implement`

```bash
# 新機能の実装
/sc:implement ユーザー認証機能を追加

# MCP統合を活用した実装
/sc:implement RESTful APIエンドポイントを設計・実装
```

**活用 MCP**: Context7、Sequential、Magic、Playwright

---

### 11. コード分析 `/sc:analyze`

```bash
# 包括的なコード分析
/sc:analyze

# 特定のモジュールを分析
/sc:analyze auth モジュールのセキュリティ分析
```

**活用 MCP**: Sequential

---

### 12. 要件発見 `/sc:brainstorm`

```bash
# 対話型要件発見
/sc:brainstorm 新しいダッシュボード機能

# プロジェクト全体の要件整理
/sc:brainstorm
```

---

### 13. システム設計 `/sc:design`

```bash
# API設計
/sc:design ユーザー管理APIの設計

# システムアーキテクチャ設計
/sc:design マイクロサービスアーキテクチャの設計
```

**活用 MCP**: Context7

---

### 14. テスト実行 `/sc:test`

```bash
# テスト実行とカバレッジ分析
/sc:test

# E2Eテスト
/sc:test E2Eテストシナリオを実行
```

**活用 MCP**: Playwright

---

### 15. セッション管理 `/sc:load` `/sc:save`

```bash
# プロジェクトコンテキスト読込
/sc:load

# セッション永続化
/sc:save
```

**活用 MCP**: Serena

---

## Git 統合コマンド

### 16. PR 作成 `/git/create-pr`

```bash
# 現在のブランチからPR作成
/git/create-pr

# タイトルと説明を指定
/git/create-pr "feat: 新機能追加" "詳細な説明..."
```

---

### 17. ローカル変更レビュー `/git/review-local-changes`

```bash
# 変更内容をレビュー
/git/review-local-changes

# レビュー後に改善提案
/git/review-local-changes --suggest
```

---

### 18. Issue 修正 `/git/fix-github-issue`

```bash
# Issue番号を指定して修正
/git/fix-github-issue 123

# Issue URLから修正
/git/fix-github-issue https://github.com/owner/repo/issues/123
```

---

## 思考深度コマンド

### 19. 深層分析 `/think/think-hard`

```bash
# アーキテクチャ分析
/think/think-hard システムアーキテクチャの課題を分析

# 依存関係調査
/think/think-hard モジュール間の依存関係を調査
```

**思考量**: 中深度（~10K tokens）

---

### 20. 最大深度分析 `/think/ultrathink`

```bash
# システム再設計
/think/ultrathink モノリスからマイクロサービスへの移行戦略

# レガシー移行
/think/ultrathink レガシーコードの全面リファクタリング計画
```

**思考量**: 最大深度（~32K tokens）

---

## TDD ワークフローコマンド

### 21. 要件整理 `/tsumiki/tdd-requirements`

```bash
# 機能要件を整理
/tsumiki/tdd-requirements ユーザー認証機能
```

---

### 22. テストケース洗い出し `/tsumiki/tdd-testcases`

```bash
# テストケースを特定
/tsumiki/tdd-testcases
```

---

### 23. Red → Green → Refactor

```bash
# Red: 失敗するテスト作成
/tsumiki/tdd-red

# Green: テストを通す実装
/tsumiki/tdd-green

# Refactor: コード品質改善
/tsumiki/tdd-refactor
```

---

### 24. 完了検証 `/tsumiki/tdd-verify-complete`

```bash
# 全テスト通過を検証
/tsumiki/tdd-verify-complete
```

---

## Zenn 記事管理コマンド

### 25. 新規記事作成 `/zenn/zenn-new`

```bash
# 記事テーマを指定
/zenn/zenn-new Claude Codeの活用術

# 章立てとテンプレート生成
/zenn/zenn-new "MCPサーバー統合ガイド"
```

---

### 26. 記事レビュー `/zenn/zenn-review`

```bash
# 記事ファイルを指定
/zenn/zenn-review articles/my-article.md

# 品質チェックと改善提案
/zenn/zenn-review articles/tech-guide.md
```

---

### 27. タイトル最適化 `/zenn/zenn-title`

```bash
# 現在のタイトルを最適化
/zenn/zenn-title Claude Codeを使ってみた

# 3パターンの提案
/zenn/zenn-title "MCPサーバーの使い方"
```

---

## 🎯 シーン別使い分け

### 開発中

```bash
/web/console-debug http://localhost:3000
/web/responsive-check http://localhost:3000
/sc:test
```

### リリース前

```bash
/web/page-audit https://staging.example.com
/web/perf-analyze https://staging.example.com
/web/mobile-emulate https://staging.example.com
/web/a11y-check https://staging.example.com
```

### 問題調査

```bash
# 遅い
/web/perf-analyze [URL]

# スマホで崩れる
/web/responsive-check [URL]

# エラーが出る
/web/console-debug [URL]

# フォームが動かない
/web/form-test [URL]

# CIが失敗
/pr_ci_fix
```

### 新機能リリース前チェックリスト

```bash
# 1. 要件確認
/sc:brainstorm
/sc:design

# 2. TDD実装
/tsumiki/tdd-requirements
/tsumiki/tdd-testcases
/tsumiki/tdd-red
/tsumiki/tdd-green
/tsumiki/tdd-refactor

# 3. 品質チェック
/web/console-debug http://localhost:3000/new-feature
/web/responsive-check http://localhost:3000/new-feature
/web/perf-analyze http://localhost:3000/new-feature
/web/a11y-check http://localhost:3000/new-feature
/web/page-audit http://localhost:3000/new-feature

# 4. CI/CD
/pr_ci_fix
/git/review-local-changes
/git/create-pr
```

### 緊急バグ修正フロー

```bash
# 1. 問題特定
/web/console-debug [URL]
/think/think-hard バグの根本原因を分析

# 2. 修正実装
/sc:implement バグ修正

# 3. テスト
/sc:test

# 4. CI確認
/pr_ci_fix

# 5. レビュー・マージ
/git/review-local-changes
```

### アーキテクチャ見直しフロー

```bash
# 1. 現状分析
/think/ultrathink システム全体の課題を洗い出し

# 2. 設計
/sc:design 新アーキテクチャの設計

# 3. 段階的実装
/sc:implement フェーズ1の実装
/sc:test

# 4. 品質確認
/web/page-audit [URL]
/web/perf-analyze [URL]
```

### 記事執筆フロー

```bash
# 1. 企画
/zenn/zenn-new "記事のテーマ"

# 2. 執筆
# ... 記事を書く ...

# 3. レビュー
/zenn/zenn-review articles/draft.md

# 4. 最適化
/zenn/zenn-title "現在のタイトル"

# 5. 公開
# ... Zennに公開 ...
```
