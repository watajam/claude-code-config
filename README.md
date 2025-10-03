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

## 主な機能

### SuperClaude Framework

- **行動モード**: Brainstorming、Introspection、Orchestration、Task Management、Token Efficiency
- **MCPサーバー統合**: Context7、Playwright、Sequential、Serena
- **フラグシステム**: タスク分析、実行制御、出力最適化

### カスタムエージェント

[wshobson/agents](https://github.com/wshobson/agents)から必要な機能を選定・統合

- **開発系**: frontend-developer、backend-architect、python-expert
- **品質系**: code-reviewer、debugger、test-automator
- **特化系**: seo-content-writer、performance-engineer、security-auditor

### スラッシュコマンド

- **Git統合**: `/git/create-pr`、`/git/review-local-changes`
- **思考深度**: `/think/think-hard`、`/think/ultrathink`
- **TDD**: `/tsumiki/tdd-red`、`/tsumiki/tdd-green`、`/tsumiki/tdd-refactor`
- **Zenn**: `/zenn/zenn-new`、`/zenn/zenn-review`

## 参考資料

- [SuperClaude Framework](https://github.com/SuperClaude-Org/SuperClaude_Framework)
- [wshobson/agents](https://github.com/wshobson/agents)
- [サブエージェントの例 - Anthropic公式](https://docs.anthropic.com/ja/docs/claude-code/sub-agents)
- [serena-expertの活用](https://zenn.dev/sc30gsw/articles/ff81891959aaef#2.-sub-agent)
- [Claude Code活用術](https://note.com/taku_sid/n/n9daa1a6ead98)

## 使い方

1. このリポジトリを`~/.claude/`に配置
2. Claude Codeを起動
3. SuperClaudeフレームワークが自動的に読み込まれる
4. カスタムコマンドは`/`で呼び出し可能

## カスタマイズ

`CLAUDE.md`でフレームワーク設定を調整可能。個人の開発スタイルに合わせて、エージェントやコマンドを追加・削除できます。
