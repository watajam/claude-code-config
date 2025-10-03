---
description: TDDのRedフェーズを実行します。失敗するテストケースを作成し、実装すべき機能を明確に定義します。
---

# TDD Redフェーズ（失敗するテストを書く）

TDDのRedフェーズを実行します。

## 事前準備

開発コンテキストの準備を行います：

1. **追加ルールの読み込み**
   - `AGENTS.md` ファイルが存在する場合は読み込み
   - `docs/rule` ディレクトリが存在する場合は読み込み
   - `docs/rule/tdd` ディレクトリが存在する場合は読み込み  
   - `docs/rule/tdd/red` ディレクトリが存在する場合は読み込み
   - 各ディレクトリ内のすべてのファイルを読み込み、追加ルールとして適用

2. **技術スタック定義の読み込み**
   - `docs/tech-stack.md` が存在する場合は読み込み
   - 存在しない場合は `CLAUDE.md` から技術スタックセクションを読み込み  
   - どちらも存在しない場合は `.claude/commands/tech-stack.md` のデフォルト定義を使用

3. **@agent-symbol-searcher でテスト実装関連情報を検索し、見つかったファイルを読み込み**
   - 読み込んだ技術スタック定義に基づいてテストフレームワークを特定
   - **UIタスクの場合**: E2Eテストフレームワーク（Playwright等）の設定とサンプルを優先的に確認
   - 既存のテストファイルやテスト関数を検索し、該当ファイルをReadツールで読み込み
   - テストセットアップやモックの使用パターンを特定し、関連ファイルをReadツールで読み込み
   - **E2Eテスト設定確認**: playwright.config.js、cypress.config.js等の設定ファイルをReadツールで読み込み

4. **関連ファイルを直接読み込み**
   - `docs/implements/{要件名}/{{task_id}}/{feature_name}-memo.md` - 既存の開発履歴を確認
   - `docs/implements/{要件名}/{{task_id}}/{feature_name}-requirements.md` - 要件定義を確認
   - `docs/implements/{要件名}/{{task_id}}/{feature_name}-testcases.md` - テストケース定義を確認
   - 関連する設計文書やタスクファイルも必要に応じて読み込み

読み込み完了後、準備されたコンテキスト情報を基にRedフェーズ（失敗テスト作成）の作業を開始します。

## 対象テストケース

**【対象テストケース】**: {{test_case_name}}

## テストケース追加目標数

**テストケース追加目標数**: 10以上（利用可能なテストケースが10未満の場合は全て追加）

未実装のテストケースから10個以上のテストケースを選択して実装してください。利用可能なテストケースが10個未満の場合は、利用可能な全てのテストケースを実装対象とします。
既にテストケースが実装済みの場合はテストケース定義に書かれているテストケースからテストを追加します。
要件網羅率と機能網羅率を高めるテストケースを追加してください
NEVER テストケースの最大行数を500行を目標に分割してください

## 信頼性レベル指示

テストコード作成時には、各テストケースの内容について元の資料との照合状況を以下の信号でコメントしてください：

- 🔵 **青信号**: 元の資料を参考にしてほぼ推測していない場合
- 🟡 **黄信号**: 元の資料から妥当な推測の場合
- 🔴 **赤信号**: 元の資料にない推測の場合

## 要件

- **使用言語/フレームワーク**: 読み込んだ技術スタック定義に基づく
- テストは必ず失敗する状態で作成
- テスト名は分かりやすく日本語で記述
- アサーション（期待値の検証）を明確に記述
- まだ実装されていない関数・メソッドを呼び出す形で作成

## 🎯 UI開発タスクでの**E2Eテスト優先**方針

**UIコンポーネントや画面機能の開発タスクでは、E2Eテスト（特にPlaywright）を最優先で活用してください：**

### 🚀 E2Eテストを**必須**とすべきケース

- **ユーザーインターフェースの動作確認** - UIコンポーネントの表示・非表示、状態変更
- **画面遷移やナビゲーションの検証** - ページ間の移動、ルーティングの動作
- **フォーム入力と送信機能のテスト** - 入力検証、エラー処理、送信後の動作
- **ユーザーインタラクション** - クリック、入力、スクロール、ドラッグ＆ドロップ等
- **ブラウザ固有の動作確認** - 異なるブラウザでの互換性テスト
- **レスポンシブデザインの検証** - 画面サイズによるレイアウト変更
- **アクセシビリティの確認** - キーボード操作、スクリーンリーダー対応
- **パフォーマンスの確認** - 読み込み時間、レンダリング速度

#### E2Eテストの作成方針
- **実際のユーザー操作を模倣**: クリック、入力、ナビゲーション等の実際の操作をテストに含める
- **エンドツーエンドのシナリオ**: ログインから目的の操作完了まで、完全なユーザージャーニーをテスト
- **視覚的な検証**: 画面表示、要素の配置、スタイリングの確認を含める
- **複数ブラウザでの検証**: 主要ブラウザでの動作確認を実施
- **レスポンシブ対応**: 様々な画面サイズでの動作確認

### 🥇 **Playwright強力推奨**フレームワーク

**UIタスクでは原則としてPlaywrightを使用してください：**

#### 1. **Playwright** (🎯最優先推奨)
   - ✅ 複数ブラウザサポート（Chrome, Firefox, Safari）
   - ✅ 高速で安定した実行
   - ✅ 豊富なアサーション機能とセレクター
   - ✅ 自動待機機能（要素の表示/非表示を自動待機）
   - ✅ ネットワーク監視・モック機能
   - ✅ スクリーンショット・動画録画
   - ✅ パフォーマンス測定機能
   - ✅ アクセシビリティテスト統合

#### 2. **Cypress** (補完的選択肢)
   - 開発者フレンドリーなAPI
   - リアルタイムデバッグ機能
   - ※ シングルタブ制限あり

#### 3. **WebDriver/Selenium** (レガシー対応時のみ)
   - 幅広いブラウザサポート
   - 成熟したエコシステム
   - ※ 設定が複雑、実行速度が遅い

## 🧭 テスト戦略選択ガイド

### UI関連タスクの場合 → **E2Eテスト最優先**
- コンポーネント作成・修正
- ページ作成・レイアウト変更
- ユーザーインタラクション実装
- フォーム機能実装
- ナビゲーション・ルーティング

### ビジネスロジック・API関連タスク → **単体・統合テスト**
- データ処理アルゴリズム
- バリデーション関数
- API エンドポイント
- データベース操作
- ユーティリティ関数

## テストコード作成指針

### E2Eテスト重視の構造
- **Scenario-Given-When-Then パターン**を採用
- **シナリオ定義**: どのようなユーザージャーニーをテストするか
- **初期状態設定（Given）**: ページアクセス、ログイン状態等
- **ユーザー操作実行（When）**: クリック、入力、ナビゲーション等
- **結果確認（Then）**: 画面表示、状態変化、ページ遷移等

### 単体・統合テスト用の構造
- **Given-When-Then パターン**を使用
- テストデータの準備（Given）
- 実際の処理の実行（When）
- 結果の検証（Then）

## 日本語コメント必須要件

テストコードには以下の日本語コメントを必ず含めてください：

### テストケース開始時のコメント

```javascript
describe('{{feature_name}}', () => {
  test('{{test_case_name}}', () => {
    // 【テスト目的】: [このテストで何を確認するかを日本語で明記]
    // 【テスト内容】: [具体的にどのような処理をテストするかを説明]
    // 【期待される動作】: [正常に動作した場合の結果を説明]
    // 🔵🟡🔴 信頼性レベル: [このテストの内容が元資料のどの程度に基づいているか]

    // 【テストデータ準備】: [なぜこのデータを用意するかの理由]
    // 【初期条件設定】: [テスト実行前の状態を説明]
    const input = {{test_input}};

    // 【実際の処理実行】: [どの機能/メソッドを呼び出すかを説明]
    // 【処理内容】: [実行される処理の内容を日本語で説明]
    const result = {{function_name}}(input);

    // 【結果検証】: [何を検証するかを具体的に説明]
    // 【期待値確認】: [期待される結果とその理由を説明]
    expect(result).toBe({{expected_output}}); // 【確認内容】: [この検証で確認している具体的な項目] 🔵🟡🔴
  });
});
```

### セットアップ・クリーンアップのコメント（必要に応じて）

```javascript
beforeEach(() => {
  // 【テスト前準備】: [各テスト実行前に行う準備作業の説明]
  // 【環境初期化】: [テスト環境をクリーンな状態にする理由と方法]
});

afterEach(() => {
  // 【テスト後処理】: [各テスト実行後に行うクリーンアップ作業の説明]
  // 【状態復元】: [次のテストに影響しないよう状態を復元する理由]
});
```

### 各expectステートメントのコメント

各expectステートメントには必ず日本語コメントを付けてください：

```javascript
expect(result.property).toBe(expectedValue); // 【確認内容】: [この検証で確認している具体的な項目と理由]
expect(result.array).toHaveLength(3); // 【確認内容】: [配列の長さが期待値と一致することを確認する理由]
expect(result.errors).toContain('error message'); // 【確認内容】: [特定のエラーメッセージが含まれることを確認する理由]
```

## 作成するテストコードの例

### 単体テスト・統合テストの例

```javascript
// テストファイル: {{test_file_name}}
describe('{{feature_name}}', () => {
  beforeEach(() => {
    // 【テスト前準備】: 各テスト実行前にテスト環境を初期化し、一貫したテスト条件を保証
    // 【環境初期化】: 前のテストの影響を受けないよう、ファイルシステムの状態をクリーンにリセット
  });

  afterEach(() => {
    // 【テスト後処理】: テスト実行後に作成された一時ファイルやディレクトリを削除
    // 【状態復元】: 次のテストに影響しないよう、システムを元の状態に戻す
  });

  test('{{test_case_name}}', () => {
    // 【テスト目的】: {{test_purpose}}
    // 【テスト内容】: {{test_description}}
    // 【期待される動作】: {{expected_behavior}}
    // 🔵🟡🔴 信頼性レベル: [このテストの内容が元資料のどの程度に基づいているか]

    // 【テストデータ準備】: {{test_data_reason}}
    // 【初期条件設定】: {{initial_condition}}
    const input = {{test_input}};

    // 【実際の処理実行】: {{function_description}}
    // 【処理内容】: {{process_description}}
    const result = {{function_name}}(input);

    // 【結果検証】: {{verification_description}}
    // 【期待値確認】: {{expected_result_reason}}
    expect(result).toBe({{expected_output}}); // 【確認内容】: {{specific_verification_point}}
  });
});
```

### UIタスク向けE2Eテストの例（Playwright）

```javascript
// E2Eテストファイル: tests/e2e/{{feature_name}}.spec.js
import { test, expect } from '@playwright/test';

describe('{{feature_name}} E2Eテスト', () => {
  test.beforeEach(async ({ page }) => {
    // 【E2Eテスト前準備】: ブラウザを起動し、テスト対象のページに移動
    // 【環境初期化】: 各テストを独立して実行するため、ページ状態をリセット
    await page.goto('/{{target_page}}');
  });

  test('{{ui_test_case_name}}', async ({ page }) => {
    // 【テスト目的】: {{ui_test_purpose}}
    // 【テスト内容】: {{ui_test_description}}
    // 【期待される動作】: {{expected_ui_behavior}}
    // 🔵🟡🔴 信頼性レベル: [このテストの内容が元資料のどの程度に基づいているか]

    // 【初期状態確認】: {{initial_ui_state_reason}}
    // 【画面表示確認】: {{screen_display_verification}}
    await expect(page.locator('{{initial_element_selector}}')).toBeVisible();
    // 【確認内容】: 初期状態で必要な要素が表示されていることを確認

    // 【ユーザー操作実行】: {{user_action_description}}
    // 【操作内容】: {{specific_action_description}}
    await page.click('{{target_button_selector}}');
    await page.fill('{{input_selector}}', '{{test_input_value}}');
    await page.click('{{submit_button_selector}}');

    // 【結果確認】: {{ui_result_verification}}
    // 【期待される表示変化】: {{expected_ui_changes}}
    await expect(page.locator('{{result_element_selector}}')).toContainText('{{expected_text}}');
    // 【確認内容】: {{specific_ui_verification_point}}

    // 【追加検証】: {{additional_verification_description}}
    await expect(page).toHaveURL('{{expected_url}}');
    // 【確認内容】: 正しいページに遷移したことを確認
  });

  test('{{responsive_test_case_name}}', async ({ page }) => {
    // 【テスト目的】: レスポンシブデザインの動作確認
    // 【テスト内容】: 異なる画面サイズでのUI表示とユーザビリティの検証
    // 【期待される動作】: モバイル・タブレット・デスクトップサイズで適切に表示される
    // 🔵🟡🔴 信頼性レベル: [このテストの内容が元資料のどの程度に基づいているか]

    // 【画面サイズ設定】: モバイルサイズでの表示確認
    // 【レスポンシブ確認】: 小さい画面での要素配置とユーザビリティをテスト
    await page.setViewportSize({ width: 375, height: 667 });
    
    // 【モバイル表示確認】: モバイル向けレイアウトが適用されることを確認
    await expect(page.locator('{{mobile_navigation_selector}}')).toBeVisible();
    // 【確認内容】: モバイル用ナビゲーションが表示されている

    // 【タブレットサイズ設定】: 中間サイズでの表示確認
    await page.setViewportSize({ width: 768, height: 1024 });
    
    // 【タブレット表示確認】: タブレット向けレイアウトの動作を確認
    await expect(page.locator('{{tablet_layout_selector}}')).toBeVisible();
    // 【確認内容】: タブレット用レイアウトが適切に表示されている
  });
});
```

### アクセシビリティテストの例

```javascript
// アクセシビリティテストファイル: tests/e2e/accessibility/{{feature_name}}.spec.js
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

describe('{{feature_name}} アクセシビリティテスト', () => {
  test('{{accessibility_test_case_name}}', async ({ page }) => {
    // 【テスト目的】: Webアクセシビリティガイドライン（WCAG）準拠の確認
    // 【テスト内容】: 自動アクセシビリティ検査とキーボード操作の検証
    // 【期待される動作】: アクセシビリティ違反がなく、キーボードで操作可能
    // 🔵🟡🔴 信頼性レベル: [このテストの内容が元資料のどの程度に基づいているか]

    // 【ページ読み込み】: テスト対象ページへの移動
    await page.goto('/{{target_page}}');

    // 【自動アクセシビリティ検査】: axe-coreを使用した自動検査
    // 【WCAG準拠確認】: 色のコントラスト、ALTテキスト、ラベル等の確認
    const accessibilityScanResults = await new AxeBuilder({ page }).analyze();
    expect(accessibilityScanResults.violations).toEqual([]);
    // 【確認内容】: アクセシビリティ違反が検出されないことを確認

    // 【キーボード操作確認】: Tabキーによるフォーカス移動の検証
    // 【操作性確認】: マウスを使わずにすべての機能が利用可能であることを確認
    await page.keyboard.press('Tab');
    await expect(page.locator('{{first_focusable_element}}')).toBeFocused();
    // 【確認内容】: 最初のフォーカス可能要素にフォーカスが移動している
  });
});
```

## 提供してください

1. **テストコード**: 実行可能な形式で、必須の日本語コメント付き
2. **テスト実行コマンド**: どのように実行するか
3. **期待される失敗メッセージ**: どのようなエラーが出るか
4. **コメントの説明**: 各日本語コメントの意図と目的

### E2Eテスト実行コマンドの例

#### Playwrightの場合
```bash
# 全E2Eテスト実行
npx playwright test

# 特定のテストファイル実行
npx playwright test tests/e2e/{{feature_name}}.spec.js

# ヘッドレスモードでない実行（ブラウザ表示）
npx playwright test --headed

# 特定のブラウザでのテスト実行
npx playwright test --project=chromium
npx playwright test --project=firefox
npx playwright test --project=webkit

# デバッグモードでの実行
npx playwright test --debug

# レポート生成
npx playwright show-report
```

#### Cypressの場合
```bash
# Cypress Test Runnerを開く
npx cypress open

# ヘッドレスモードでテスト実行
npx cypress run

# 特定のテストファイル実行
npx cypress run --spec "cypress/e2e/{{feature_name}}.cy.js"

# 特定のブラウザでの実行
npx cypress run --browser chrome
```

テストコード作成後、以下を実行してください：

1. **メモファイル作成・更新**: `docs/implements/{要件名}/{{task_id}}/{feature_name}-memo.md` ファイルにRedフェーズの内容を作成または追記
   - 既存のメモファイルがある場合は、Redフェーズセクションを更新
   - メモファイルが存在しない場合は新規作成
2. テストコードの設計内容を `docs/implements/{要件名}/{{task_id}}/{feature_name}-red-phase.md` に保存（既存ファイルがある場合は追記）
3. TODOステータスを更新（Redフェーズ完了をマーク）
4. **品質判定**: テストコードの品質を以下の基準で判定
   - テスト実行: 実行可能で失敗することを確認済み
   - 期待値: 明確で具体的
   - アサーション: 適切
   - 実装方針: 明確
5. **次のステップ表示**: 判定結果に関わらず、次のお勧めコマンドを表示
   - 「次のお勧めステップ: `/tdd-green` でGreenフェーズ（最小実装）を開始します。」

## TDDメモファイル形式

`docs/implements/{要件名}/{{task_id}}/{feature_name}-memo.md` ファイルの形式：

```markdown
# TDD開発メモ: {feature_name}

## 概要

- 機能名: [機能名]
- 開発開始: [日時]
- 現在のフェーズ: [Red/Green/Refactor]

## 関連ファイル

- 元タスクファイル: `docs/tasks/{taskファイルのパス}.md`
- 要件定義: `docs/implements/{要件名}/{{task_id}}/{feature_name}-requirements.md`
- テストケース定義: `docs/implements/{要件名}/{{task_id}}/{feature_name}-testcases.md`
- 実装ファイル: `[実装ファイルのパス]`
- テストファイル: `[テストファイルのパス]`

## Redフェーズ（失敗するテスト作成）

### 作成日時

[日時]

### テストケース

[作成したテストケースの概要]

### テストコード

[実際のテストコード]

### 期待される失敗

[どのような失敗が期待されるか]

### 次のフェーズへの要求事項

[Greenフェーズで実装すべき内容]

## Greenフェーズ（最小実装）

### 実装日時

[日時]

### 実装方針

[最小実装の方針]

### 実装コード

[実際の実装コード]

### テスト結果

[テストが通った結果]

### 課題・改善点

[Refactorフェーズで改善すべき点]

## Refactorフェーズ（品質改善）

### リファクタ日時

[日時]

### 改善内容

[具体的な改善内容]

### セキュリティレビュー

[セキュリティ面での確認結果]

### パフォーマンスレビュー

[パフォーマンス面での確認結果]

### 最終コード

[リファクタ後のコード]

### 品質評価

[最終的な品質評価]
```

## 品質判定基準

```
✅ 高品質:
- テスト実行: 成功（失敗することを確認）
- 期待値: 明確で具体的
- アサーション: 適切
- 実装方針: 明確

⚠️ 要改善:
- テストが実行できない
- 期待値が曖昧
- 実装アプローチが不明
- 複雑なテストケース
```

## TODO更新パターン

```
- 現在のTODO「Redフェーズ（失敗テスト作成）」を「completed」にマーク
- 失敗テスト作成フェーズの完了をTODO内容に反映
- 品質判定結果をTODO内容に記録
- 次のフェーズ「Greenフェーズ（最小実装）」をTODOに追加
```

次のステップ: `/tdd-green` でテストを通すための最小限の実装を行います。
