# Claude Code Configuration

SuperClaude フレームワーク、カスタムエージェント、スラッシュコマンド、ワークフロー自動化を含む個人用 Claude Code 設定。

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

## 導入している MCP サーバー

- **serena**: セマンティックコード理解とプロジェクトメモリ管理
- **context7**: 公式ライブラリドキュメント検索とパターンガイダンス
- **playwright**: ブラウザ自動化と E2E テスト
- **sequential-thinking**: 複雑な問題の多段階推論エンジン
- **codex**: 高度なコード生成・大規模リファクタリング・アーキテクチャ設計
- **figma-dev-mode-mcp-server**: Figma デザインファイルの開発モード連携
- **cipher**: 暗号化・復号化処理
- **chrome-devtools**: Chrome DevTools プロトコル統合

## セキュリティ設定（settings.json）

### 環境変数

| 変数                         | 値  | 説明             |
| ---------------------------- | --- | ---------------- |
| CLAUDE_CODE_ENABLE_TELEMETRY | 0   | テレメトリ無効化 |

### 許可コマンド（permissions.allow）

| カテゴリ            | コマンド                                                                     | 用途                         |
| ------------------- | ---------------------------------------------------------------------------- | ---------------------------- |
| **ファイル操作**    | ls, find, rg, cat, mkdir, mv, cp, rm, echo, pwd, cd                          | 基本的なファイルシステム操作 |
| **Git 読取専用**    | git status, git diff, git log, git branch, git checkout, git pull, git add   | リポジトリ情報取得・準備     |
| **GitHub CLI 読取** | gh repo view, gh pr list/view, gh issue list/view                            | GitHub 情報参照              |
| **WebFetch**        | github.com, docs.anthropic.com, developer.mozilla.org, nodejs.org, npmjs.com | 信頼ドメインからの情報取得   |

### 拒否コマンド（permissions.deny）

| カテゴリ              | 拒否コマンド                                                                                              | 理由                     |
| --------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------ |
| **破壊的操作**        | rm -rf /\*, sudo, git reset/rebase/push/commit                                                            | システム・リポジトリ保護 |
| **ネットワーク**      | curl, wget, nc                                                                                            | 不正アクセス防止         |
| **パッケージ削除**    | npm uninstall/remove                                                                                      | 依存関係保護             |
| **DB 直接操作**       | psql, mysql, mongod, supabase execute_sql                                                                 | データ保護               |
| **GitHub 破壊的操作** | repo delete, auth token, secret/variable set, workflow disable, release delete, API DELETE/PUT/PATCH/POST | GitHub リソース保護      |
| **秘密情報**          | id_rsa, id_ed25519, .env*, *token*, *key\*, mcp.json                                                      | 認証情報保護             |

### フック設定（hooks）

| フック種別              | トリガー                    | 動作                                             | 目的                 |
| ----------------------- | --------------------------- | ------------------------------------------------ | -------------------- |
| **Notification**        | 許可ダイアログ表示時        | macOS 通知（Glass 音）                           | ユーザーへのアラート |
| **Stop**                | タスク完了時                | macOS 通知（Hero 音）                            | 作業完了通知         |
| **PreToolUse(Edit 系)** | Edit/MultiEdit/Write 実行前 | .env, package-lock.json, .git ファイルをブロック | 重要ファイル保護     |
| **PreToolUse(Bash)**    | Bash コマンド実行前         | 危険な rm/delete 操作をブロック                  | 破壊的操作防止       |
| **SubagentStop**        | Subagent 終了時             | validation-gates 完了チェック                    | 品質ゲート確認       |
| **UserPromptSubmit**    | プロンプト送信時            | テスト関連キーワード検出でリマインダー           | テスト実施促進       |

### 常時有効機能

| 機能                  | 設定値 | 説明                   |
| --------------------- | ------ | ---------------------- |
| alwaysThinkingEnabled | true   | 常に思考プロセスを表示 |

## 主な機能

### SuperClaude Framework

- **行動モード**: Brainstorming、Introspection、Orchestration、Task Management、Token Efficiency
- **MCP サーバー統合**: Context7、Playwright、Sequential、Serena
- **フラグシステム**: タスク分析、実行制御、出力最適化

### カスタムエージェント

[wshobson/agents](https://github.com/wshobson/agents)から必要な機能を選定・統合

**命名規則**:

- `-sc` サフィックス: SuperClaude Framework 専用エージェント
- `-wshobson` サフィックス: wshobson/agents から統合したエージェント
- `-official` サフィックス: 公式推奨エージェント

#### 主要エージェント一覧

| カテゴリ                  | エージェント名                        | 説明                                         | 主な用途                                  |
| ------------------------- | ------------------------------------- | -------------------------------------------- | ----------------------------------------- |
| **開発系**                | python-expert-sc                      | SOLID 原則に従った本番対応 Python コード生成 | Python 開発、最適化、テスト戦略           |
|                           | backend-architect-sc                  | RESTful API、マイクロサービス設計            | バックエンド設計、DB 設計                 |
|                           | backend-architect-wshobson            | バックエンド開発専門                         | API 設計、データベース最適化              |
|                           | frontend-architect-sc                 | アクセシブルで高性能な UI 設計               | UX、モダンフレームワーク                  |
|                           | frontend-developer-wshobson           | React/Vue/Angular コンポーネント実装         | フロントエンド開発                        |
|                           | csharp-pro-wshobson                   | 最新 C#機能を活用した.NET 開発               | records、pattern matching、async/await    |
|                           | javascript-pro-wshobson               | モダン JavaScript/ES6+開発                   | 非同期パターン、Node.js API               |
|                           | typescript-pro-wshobson               | 高度な型システム活用 TypeScript 開発         | ジェネリクス、型推論最適化                |
|                           | mobile-developer-wshobson             | React Native/Flutter アプリ開発              | クロスプラットフォーム、ネイティブ統合    |
|                           | ios-developer-wshobson                | Swift/SwiftUI ネイティブ iOS 開発            | UIKit/SwiftUI、Core Data                  |
|                           | serena-expert                         | /serena コマンド活用による効率的開発         | アプリ開発、API 実装、システム構築        |
| **品質系**                | code-reviewer-official                | コード品質・セキュリティ・保守性レビュー     | コードレビュー、品質保証                  |
|                           | code-reviewer-wshobson                | エキスパートコードレビュー専門               | 品質、セキュリティ、保守性                |
|                           | quality-engineer-sc                   | 包括的テスト戦略とエッジケース検出           | 品質保証、テスト設計                      |
|                           | debugger-official                     | エラー・テスト失敗のデバッグ専門             | 問題調査、根本原因分析                    |
|                           | debugger-wshobson                     | デバッグスペシャリスト                       | エラー解析、バグ修正                      |
|                           | test-automator-wshobson               | unit/integration/E2E テスト自動化            | テスト戦略、CI/CD 統合                    |
| **セキュリティ**          | security-engineer-sc                  | セキュリティ標準準拠と脆弱性検出             | セキュリティ監査、ベストプラクティス      |
|                           | security-auditor-wshobson             | OWASP 準拠、認証実装、脆弱性検出             | セキュリティレビュー、認証フロー          |
| **パフォーマンス**        | performance-engineer-sc               | 測定駆動分析とボトルネック解消               | パフォーマンス最適化                      |
|                           | performance-engineer-wshobson         | ボトルネック分析、最適化、キャッシング戦略   | 性能改善、負荷テスト                      |
| **アーキテクチャ**        | system-architect-sc                   | スケーラブルなシステム設計                   | 長期的な技術判断、保守性                  |
|                           | architect-review-wshobson             | アーキテクチャ一貫性レビュー                 | 設計パターン、SOLID 原則検証              |
|                           | refactoring-expert-sc                 | 体系的リファクタリングと技術的負債削減       | コード品質改善、クリーンコード            |
|                           | legacy-modernizer-wshobson            | レガシーコード刷新とフレームワーク移行       | 技術的負債解消、段階的移行                |
|                           | root-cause-analyst-sc                 | エビデンスベース問題分析と仮説検証           | 根本原因調査、システム分析                |
| **DevOps/インフラ**       | devops-architect-sc                   | インフラ自動化と信頼性向上                   | CI/CD、監視、デプロイ                     |
|                           | devops-troubleshooter-wshobson        | 本番問題デバッグとログ分析                   | インシデント対応、根本原因分析            |
|                           | deployment-engineer-wshobson          | CI/CD パイプライン構築と自動化               | GitHub Actions、Docker、Kubernetes        |
|                           | incident-responder-wshobson           | 本番インシデント緊急対応                     | 障害対応、即時修正、事後分析              |
|                           | network-engineer-wshobson             | ネットワーク接続とプロトコル最適化           | DNS、SSL/TLS、負荷分散                    |
| **データベース**          | database-admin-wshobson               | DB 運用・バックアップ・レプリケーション      | データベース管理、災害復旧                |
|                           | database-optimizer-wshobson           | SQL クエリ最適化とインデックス設計           | N+1 問題解決、クエリチューニング          |
|                           | sql-pro-wshobson                      | 複雑 SQL 作成と実行計画最適化                | CTE、ウィンドウ関数、ストアドプロシージャ |
|                           | data-scientist-official               | SQL/BigQuery によるデータ分析                | データ分析、クエリ最適化                  |
|                           | data-scientist-wshobson               | データ分析エキスパート                       | BigQuery、データインサイト                |
| **AI/ML**                 | ai-engineer-wshobson                  | LLM アプリケーション・RAG システム構築       | ベクトル検索、エージェント統合            |
|                           | ml-engineer-wshobson                  | ML パイプライン・モデル本番化                | TensorFlow/PyTorch、A/B テスト            |
|                           | mlops-engineer-wshobson               | ML 実験管理とモデルレジストリ                | MLflow、Kubeflow、再学習自動化            |
|                           | quant-analyst-wshobson                | 金融モデル・トレーディング戦略構築           | ポートフォリオ最適化、リスク分析          |
|                           | risk-manager-wshobson                 | ポートフォリオリスク監視と戦略               | ヘッジ戦略、ポジション管理                |
| **ドキュメント**          | technical-writer-sc                   | 技術文書作成、対象読者に合わせた説明         | API 文書、ユーザーガイド                  |
|                           | api-documenter-wshobson               | OpenAPI/Swagger 仕様・SDK 生成               | API 文書、クライアントライブラリ          |
|                           | docs-architect-wshobson               | コードベースから包括的技術文書生成           | システム文書、アーキテクチャガイド        |
|                           | reference-builder-wshobson            | 網羅的技術リファレンス作成                   | API 仕様、設定ガイド                      |
| **教育/学習**             | learning-guide-sc                     | プログラミング概念教育と段階的学習           | 概念説明、実践例                          |
|                           | socratic-mentor-sc                    | ソクラテス式問答による学習支援               | 発見学習、戦略的質問                      |
| **SEO/コンテンツ**        | seo-content-writer-wshobson           | SEO 最適化コンテンツ作成                     | ブログ記事、マーケティング                |
|                           | seo-content-auditor-wshobson          | コンテンツ品質・E-E-A-T 分析                 | 品質評価、改善提案                        |
|                           | seo-content-planner-wshobson          | コンテンツ戦略・トピッククラスター設計       | コンテンツカレンダー、ギャップ分析        |
|                           | seo-content-refresher-wshobson        | 古いコンテンツの更新提案                     | 鮮度維持、統計更新                        |
|                           | seo-keyword-strategist-wshobson       | キーワード使用分析・LSI 提案                 | 密度最適化、セマンティック展開            |
|                           | seo-meta-optimizer-wshobson           | メタタイトル・ディスクリプション最適化       | クリック率向上、文字数最適化              |
|                           | seo-snippet-hunter-wshobson           | 強調スニペット獲得フォーマット作成           | SERP 機能対応                             |
|                           | seo-structure-architect-wshobson      | コンテンツ構造・スキーママークアップ最適化   | 見出し階層、内部リンク                    |
|                           | seo-authority-builder-wshobson        | E-E-A-T 信号分析と信頼性強化                 | 権威性構築、YMYL 対応                     |
|                           | seo-cannibalization-detector-wshobson | キーワード重複・カニバリゼーション検出       | 差別化戦略                                |
|                           | content-marketer-wshobson             | マーケティングコンテンツ作成                 | ブログ、SNS、メール                       |
| **法務/コンプライアンス** | legal-advisor-wshobson                | プライバシーポリシー、利用規約作成           | 法的文書、GDPR 対応                       |
| **決済/金融**             | payment-integration-wshobson          | Stripe/PayPal 決済統合                       | 決済フロー、サブスクリプション            |
| **デザイン/UI**           | ui-ux-designer-wshobson               | インターフェース設計・デザインシステム       | ワイヤーフレーム、アクセシビリティ        |
|                           | mermaid-expert-wshobson               | Mermaid 図（フローチャート、ER 図）作成      | ビジュアル文書化、設計図                  |
| **ビジネス分析**          | business-analyst-wshobson             | メトリクス分析・レポート作成                 | ダッシュボード、成長予測                  |
|                           | requirements-analyst-sc               | 要件発見と仕様明確化                         | 要件定義、体系的分析                      |
| **開発体験**              | dx-optimizer-wshobson                 | 開発者体験向上とツール改善                   | セットアップ最適化、ワークフロー改善      |
|                           | prompt-engineer-wshobson              | LLM プロンプト最適化                         | AI 機能構築、エージェント性能向上         |
| **調査/検索**             | search-specialist-wshobson            | 高度な検索技術と情報収集                     | 競合分析、事実確認                        |
|                           | error-detective-wshobson              | ログ・エラーパターン検索                     | 根本原因特定、異常検出                    |
| **統合/管理**             | context-manager-wshobson              | マルチエージェント・長期タスク管理           | コンテキスト保持、複雑ワークフロー        |
| **ルーティング**          | intelligent-router-codex              | タスク複雑度を評価し最適な実行方法を自動判断 | Codex MCP 統合、タスク分析                |

### スラッシュコマンド

**命名規則**:

- `/sc:*` プレフィックス: SuperClaude Framework 統合コマンド
- `/think/*` プレフィックス: Sequential MCP 活用思考深度コマンド
- `/tsumiki/*` プレフィックス: Tsumiki ワークフロー（TDD、Kairo、リバースエンジニアリング）
- `/git/*` プレフィックス: Git/GitHub 統合コマンド
- `/web/*` プレフィックス: Web テスト・ブラウザ自動化コマンド
- `/zenn/*` プレフィックス: Zenn 記事管理コマンド

#### 汎用ワークフロー

| コマンド            | 説明                    | 主な機能                                                   |
| ------------------- | ----------------------- | ---------------------------------------------------------- |
| `/bugfix`           | バグ修正ワークフロー    | バグ理解 →Context7/Web Search 活用 → 修正 → 検証           |
| `/update-claude-md` | CLAUDE.md 自動更新      | プロジェクト構造分析 → 技術スタック識別 → ドキュメント更新 |
| `/visual`           | ビジュアル駆動開発      | Playwright 活用、スクリーンショット比較、UI 反復改善       |
| `/serena`           | Serena MCP 統合問題解決 | トークン効率的な構造化開発（-q:簡易/-d:深層/-s:実装）      |

#### SuperClaude コマンド（/sc:）

| コマンド           | 説明                                    | 活用 MCP                                |
| ------------------ | --------------------------------------- | --------------------------------------- |
| `/sc:implement`    | 機能実装（ペルソナ・MCP 統合）          | Context7, Sequential, Magic, Playwright |
| `/sc:analyze`      | 包括的コード分析                        | Sequential                              |
| `/sc:brainstorm`   | 対話型要件発見                          | -                                       |
| `/sc:design`       | システム設計・API 設計                  | Context7                                |
| `/sc:test`         | テスト実行・カバレッジ分析              | Playwright                              |
| `/sc:load`         | プロジェクトコンテキスト読込            | Serena                                  |
| `/sc:save`         | セッション永続化                        | Serena                                  |
| `/sc:document`     | コンポーネント/関数/API 文書生成        | -                                       |
| `/sc:explain`      | コード・概念・振る舞いの明確な説明      | -                                       |
| `/sc:improve`      | コード品質・性能・保守性の体系的改善    | -                                       |
| `/sc:troubleshoot` | コード・ビルド・デプロイの問題診断解決  | -                                       |
| `/sc:estimate`     | タスク・機能・プロジェクトの開発見積    | -                                       |
| `/sc:spawn`        | メタシステムタスク編成と委譲            | -                                       |
| `/sc:build`        | ビルド・コンパイル・パッケージ化        | -                                       |
| `/sc:cleanup`      | コード整理・デッドコード削除・最適化    | -                                       |
| `/sc:git`          | Git 操作と最適化ワークフロー            | -                                       |
| `/sc:index`        | プロジェクト文書・ナレッジベース生成    | -                                       |
| `/sc:reflect`      | タスク振り返りと Serena MCP 分析        | Serena                                  |
| `/sc:select-tool`  | 複雑度評価による最適 MCP 選択           | -                                       |
| `/sc:workflow`     | PRD・要件から構造化実装ワークフロー生成 | -                                       |

#### Git 統合

| コマンド                     | 説明                       |
| ---------------------------- | -------------------------- |
| `/git/create-pr`             | プルリクエスト作成         |
| `/git/review-local-changes`  | ローカル変更レビュー       |
| `/git/fix-github-issue`      | GitHub Issue 修正          |
| `/git/plan-github-issue`     | GitHub Issue 実装計画策定  |
| `/git/update-planned-issue`  | 計画済み Issue の更新      |
| `/git/apply-review-feedback` | レビューフィードバック適用 |

#### 思考深度コマンド（/think:）

| コマンド              | 思考量                  | 用途                                       |
| --------------------- | ----------------------- | ------------------------------------------ |
| `/think/think-hard`   | 中深度（~10K tokens）   | アーキテクチャ分析、依存関係調査           |
| `/think/think-harder` | 深層分析（~20K tokens） | 複雑な問題の多角的分析                     |
| `/think/ultrathink`   | 最大深度（~32K tokens） | システム再設計、レガシー移行、複雑デバッグ |

#### TDD ワークフロー（/tsumiki:）

| コマンド                       | フェーズ | 説明                               |
| ------------------------------ | -------- | ---------------------------------- |
| `/tsumiki/init-tech-stack`     | 初期化   | 技術スタック選定（CLAUDE.md 作成） |
| `/tsumiki/tdd-requirements`    | 準備     | 要件整理                           |
| `/tsumiki/tdd-testcases`       | 準備     | テストケース洗い出し               |
| `/tsumiki/tdd-todo`            | 準備     | 実装可能 TODO リスト作成           |
| `/tsumiki/tdd-red`             | Red      | 失敗するテスト作成                 |
| `/tsumiki/tdd-green`           | Green    | テストを通す実装                   |
| `/tsumiki/tdd-refactor`        | Refactor | コード品質改善                     |
| `/tsumiki/tdd-checkpoint`      | 検証     | 各フェーズ完了検証                 |
| `/tsumiki/tdd-verify-complete` | 完了     | 全テスト通過検証                   |
| `/tsumiki/auto-debug`          | デバッグ | テストエラー自動解消               |

#### Kairo ワークフロー（/tsumiki:）

| コマンド                      | 説明                                      |
| ----------------------------- | ----------------------------------------- |
| `/tsumiki/kairo-requirements` | EARS 記法で要件定義書作成                 |
| `/tsumiki/kairo-design`       | 技術設計書生成（データフロー、API、DB）   |
| `/tsumiki/kairo-tasks`        | 設計から 1 日単位タスク分割・フェーズ整理 |
| `/tsumiki/kairo-task-verify`  | タスクファイル内容検証・補完              |
| `/tsumiki/kairo-implement`    | 分割タスクの順次実装                      |

#### リバースエンジニアリング（/tsumiki:）

| コマンド                    | 説明                                       |
| --------------------------- | ------------------------------------------ |
| `/tsumiki/rev-requirements` | コードベースから要件定義書逆生成           |
| `/tsumiki/rev-design`       | コードベースから技術設計書逆生成           |
| `/tsumiki/rev-specs`        | コードベースからテストケース・仕様書逆生成 |
| `/tsumiki/rev-tasks`        | コードベースから実装タスク一覧逆生成       |

#### その他 Tsumiki コマンド

| コマンド                    | 説明                           |
| --------------------------- | ------------------------------ |
| `/tsumiki/direct-setup`     | DIRECT 設定作業実行            |
| `/tsumiki/direct-verify`    | DIRECT 設定動作確認・テスト    |
| `/tsumiki/start-server`     | 開発サーバー起動・管理         |
| `/tsumiki/tech-stack`       | 技術スタック情報表示           |
| `/tsumiki/tdd-load-context` | TDD 関連ファイル読込（非推奨） |

#### Zenn 記事管理

| コマンド            | 説明                                     |
| ------------------- | ---------------------------------------- |
| `/zenn/zenn-new`    | 新規記事作成（章立て・テンプレート生成） |
| `/zenn/zenn-review` | 記事品質チェック・改善提案               |
| `/zenn/zenn-title`  | タイトル最適化（3 パターン提案）         |

#### CI/CD 統合

| コマンド     | 説明                                                          |
| ------------ | ------------------------------------------------------------- |
| `/pr_ci_fix` | 現在のブランチから PR を特定、GitHub Actions 失敗を調査・修正 |

#### Web/ブラウザテスト（/web:）

| コマンド                | 説明                           | 主な検証項目                                     |
| ----------------------- | ------------------------------ | ------------------------------------------------ |
| `/web/a11y-check`       | アクセシビリティ監査           | WCAG 2.1 Level AA 準拠、ARIA 検証                |
| `/web/console-debug`    | コンソールエラー/警告分析      | JavaScript エラー、ネットワーク警告検出          |
| `/web/form-test`        | フォーム機能テスト             | バリデーション、送信処理、エラー表示             |
| `/web/mobile-emulate`   | モバイルデバイスエミュレート   | タッチ操作、センサー、ビューポート               |
| `/web/network-analyze`  | ネットワークパフォーマンス分析 | リクエスト最適化、ロード時間、リソース効率       |
| `/web/page-audit`       | ページ総合監査                 | SEO、パフォーマンス、品質評価                    |
| `/web/perf-analyze`     | パフォーマンス分析             | Core Web Vitals、レンダリング、最適化提案        |
| `/web/responsive-check` | レスポンシブデザイン検証       | 複数デバイスサイズ、レイアウト、タッチターゲット |

## 参考資料

### フレームワーク・エージェント

- [SuperClaude Framework](https://github.com/SuperClaude-Org/SuperClaude_Framework)
- [wshobson/agents](https://github.com/wshobson/agents)
- [サブエージェントの例 - Anthropic 公式](https://docs.anthropic.com/ja/docs/claude-code/sub-agents)
- [serena-expert の活用](https://zenn.dev/sc30gsw/articles/ff81891959aaef#2.-sub-agent)
- [Claude Code 活用術](https://note.com/taku_sid/n/n9daa1a6ead98)

### MCP 導入ガイド

- [Claude Code で MCP ツール（Context7、Serena、Cipher）を活用して AI コーディングを次のレベルへ](https://qiita.com/sukimaengineer/items/845ad14a3ec2d3c39930)
- [Claude Code で Context7、Serena、Cipher を同時使用する際の注意事項](https://qiita.com/sukimaengineer/items/9a0a10d23c86f2a2e850)
- [Claude Code MCP Serena & Cipher 導入ガイド](https://zenn.dev/aki1990/articles/a7db63dbc99848)
- [Claude Code を更に賢く - serena と cipher を mcp で使う](https://zenn.dev/sho7650/articles/5d9b46a119a08f)

## 使い方

1. このリポジトリを`~/.claude/`に配置
2. Claude Code を起動
3. SuperClaude フレームワークが自動的に読み込まれる
4. カスタムコマンドは`/`で呼び出し可能

### コマンド使用例

各コマンドの具体的な使い方とシーン別の活用方法については、以下のドキュメントを参照してください：

📖 [コマンド使用例集](claudedocs/command-usage-examples.md)

- 全 27 コマンドの使用例
- シーン別使い分けガイド
- 実践的なワークフロー例（開発、リリース、問題調査、バグ修正等）

## カスタマイズ

`CLAUDE.md`でフレームワーク設定を調整可能。個人の開発スタイルに合わせて、エージェントやコマンドを追加・削除できます。
