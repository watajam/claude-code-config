---
description: 承認された要件定義書に基づいて、技術設計文書を生成する。データフロー図、TypeScriptインターフェース、データベーススキーマ、APIエンドポイントを含む包括的な設計を行います。
---

# kairo-design

## 目的

承認された要件定義書に基づいて、技術設計文書を生成する。データフロー図、TypeScriptインターフェース、データベーススキーマ、APIエンドポイントを含む包括的な設計を行う。

## 前提条件

- `docs/spec/` に要件定義書が存在する
- 要件がユーザによって承認されている

## 事前準備

1. **追加ルールの読み込み**
   - `docs/rule` ディレクトリが存在する場合は読み込み
   - `docs/rule/kairo` ディレクトリが存在する場合は読み込み  
   - `docs/rule/kairo/design` ディレクトリが存在する場合は読み込み
   - 各ディレクトリ内のすべてのファイルを読み込み、追加ルールとして適用

## 実行内容

**【信頼性レベル指示】**:
各項目について、元の資料（EARS要件定義書・設計文書含む）との照合状況を以下の信号でコメントしてください：

- 🔵 **青信号**: EARS要件定義書・設計文書を参考にしてほぼ推測していない場合
- 🟡 **黄信号**: EARS要件定義書・設計文書から妥当な推測の場合
- 🔴 **赤信号**: EARS要件定義書・設計文書にない推測の場合

2. **技術スタック定義の読み込み**
   - `docs/tech-stack.md` が存在する場合は読み込み
   - 存在しない場合は `CLAUDE.md` から技術スタックセクションを読み込み  
   - どちらも存在しない場合は `.claude/commands/tech-stack.md` のデフォルト定義を使用

3. **要件の分析**
   - @agent-symbol-searcher で要件定義書を検索し、見つかったファイルをReadツールで読み込み
   - @agent-symbol-searcher で関連する既存設計文書を確認し、見つかったファイルをReadツールで読み込み
   - 読み込んだ技術スタック定義に基づいて技術選択を決定
   - 機能要件と非機能要件を整理する
   - システムの境界を明確にする

4. **アーキテクチャ設計**
   - システム全体のアーキテクチャを決定
   - フロントエンド/バックエンドの分離
   - マイクロサービスの必要性を検討

5. **データフロー図の作成**
   - Mermaid記法でデータフローを可視化
   - ユーザーインタラクションの流れ
   - システム間のデータの流れ

6. **TypeScriptインターフェースの定義**
   - NEVER 対象言語がTypeScriptで無い場合はそれに合わせたファイル形式に変更する。インタフェース定義が不要なら生成しない
   - エンティティの型定義
   - APIリクエスト/レスポンスの型定義
   - 共通型の定義

8. **データベーススキーマの設計**
   - NEVER 対象がデータベーススキーマが不要の場合は生成しない
   - テーブル定義
   - リレーションシップ
   - インデックス戦略
   - 正規化レベルの決定

9. **APIエンドポイントの設計**
   - NEVER 対象にAPIではない場合、または既存のAPIを利用する場合は生成しない
   - RESTful API設計
   - エンドポイントの命名規則
   - HTTPメソッドの適切な使用
   - リクエスト/レスポンスの構造

10. **ファイルの作成**
   - `docs/design/{要件名}/` ディレクトリに以下を作成：
     - `architecture.md` - アーキテクチャ概要
     - `dataflow.md` - データフロー図
     - `interfaces.ts` - TypeScript型定義
     - `database-schema.sql` - DBスキーマ
     - `api-endpoints.md` - API仕様

## 出力フォーマット例

### architecture.md

```markdown
# {要件名} アーキテクチャ設計

## システム概要

{システムの概要説明}

## アーキテクチャパターン

- パターン: {選択したパターン}
- 理由: {選択理由}

## コンポーネント構成

### フロントエンド

- フレームワーク: {使用フレームワーク}
- 状態管理: {状態管理方法}

### バックエンド

- フレームワーク: {使用フレームワーク}
- 認証方式: {認証方法}

### データベース

- DBMS: {使用するDBMS}
- キャッシュ: {キャッシュ戦略}
```

### dataflow.md

```markdown
# データフロー図

## ユーザーインタラクションフロー

\`\`\`mermaid
flowchart TD
A[ユーザー] --> B[フロントエンド]
B --> C[API Gateway]
C --> D[バックエンド]
D --> E[データベース]
\`\`\`

## データ処理フロー

\`\`\`mermaid
sequenceDiagram
participant U as ユーザー
participant F as フロントエンド
participant B as バックエンド
participant D as データベース

    U->>F: アクション
    F->>B: APIリクエスト
    B->>D: クエリ実行
    D-->>B: 結果返却
    B-->>F: レスポンス
    F-->>U: 画面更新

\`\`\`
```

### interfaces.ts

```typescript
// エンティティ定義
export interface User {
  id: string;
  email: string;
  name: string;
  createdAt: Date;
  updatedAt: Date;
}

// APIリクエスト/レスポンス
export interface CreateUserRequest {
  email: string;
  name: string;
  password: string;
}

export interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: {
    code: string;
    message: string;
  };
}
```

### database-schema.sql

```sql
-- ユーザーテーブル
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- インデックス
CREATE INDEX idx_users_email ON users(email);
```

### api-endpoints.md

```markdown
# API エンドポイント仕様

## 認証

### POST /auth/login

リクエスト:
\`\`\`json
{
"email": "user@example.com",
"password": "password"
}
\`\`\`

レスポンス:
\`\`\`json
{
"success": true,
"data": {
"token": "jwt-token",
"user": { ... }
}
}
\`\`\`

## ユーザー管理

### GET /users/:id

### POST /users

### PUT /users/:id

### DELETE /users/:id
```

## 実行後の確認

- @agent-symbol-searcher で作成した設計と既存システムとの整合性を確認
- 作成したファイルの一覧を表示
- 設計の主要なポイントをサマリーで表示
- ユーザに確認を促すメッセージを表示
