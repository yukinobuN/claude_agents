# 開発用フォルダ - CLAUDE.md

このファイルは、開発用フォルダ配下のすべてのプロジェクトに適用される共通ガイドラインです。

## 新規プロジェクト開発時の必須手順

### 重要: プロジェクト初期化

ユーザーが新しいアプリケーションやプロジェクトの開発を依頼した場合、**必ず最初に以下を実行すること**:

```
/doc-generator
```

このエージェントは、プロジェクトに必要な以下のドキュメントとシステムを自動生成します：

1. **要件定義書**: プロジェクトの目的と機能要件
2. **詳細仕様書**: 技術的な実装詳細
3. **作業指示書**: 段階的な実装タスク
4. **イシュー管理システム**: エラーやバグの管理
5. **開発ガイド**: 開発ワークフロー

### ワークフロー

#### 新規プロジェクト開始時
1. ユーザーから新規アプリ開発の依頼を受ける
2. **即座に `/doc-generator` の実行を提案**
3. ユーザーと対話しながら要件をヒアリング
4. ドキュメント完全セットを自動生成
5. 生成されたドキュメントをもとに実装開始

#### 開発中
- エラーやバグが発生したら `docs/issues/TEMPLATE.md` をコピーしてイシューとして記録
- 作業指示書に従って段階的に実装
- イシューのステータスを更新しながら進める

#### 完了時
- 作業指示書の完了チェックリストで確認
- すべての機能が動作することを検証

## doc-generatorエージェントについて

### 自動提案のタイミング

以下のようなユーザーの発言があった場合、**自動的に** `/doc-generator` の使用を提案すること:

- 「新しいアプリを作りたい」
- 「〇〇システムを開発したい」
- 「プロジェクトを始めたい」
- 「要件定義を作成したい」
- 「アプリ開発を再開したい」

### 提案方法の例

```
新しいアプリの開発ですね。まず `/doc-generator` を実行して、
要件定義書・仕様書・作業指示書・イシュー管理システムを作成しましょうか？
```

### エージェントが生成するもの

- `docs/{プロジェクト名}-requirements.md`
- `docs/{プロジェクト名}-specification.md`
- `docs/{プロジェクト名}-work-instructions.md`
- `docs/issues/README.md`
- `docs/issues/TEMPLATE.md`
- `docs/DEVELOPMENT_GUIDE.md`

## エラー・イシュー管理

**重要・絶対遵守**: 開発中にエラーやバグが発生した場合、**必ず**イシューとして記録すること。イシュー記録なしでエラー修正を進めてはならない。

### イシュー作成の必須手順

開発中にエラーやバグが発生した場合、以下を**即座に実行**:

1. **イシューファイル作成**
   - `docs/issues/TEMPLATE.md` をコピー
   - `docs/issues/issue-XXX.md` として保存（連番）

2. **エラー情報の記録**（以下すべて必須）:
   - ステータス（Open/In Progress/Resolved/Closed）
   - 優先度（Critical/High/Medium/Low）
   - カテゴリ（Bug/Error/Enhancement/Documentation/Question）
   - **エラーメッセージ全文**
   - **スタックトレース**
   - **再現手順**
   - 試した解決方法（失敗も含む）
   - 最終的な解決方法

3. **イシュー一覧の更新**
   - `docs/issues/README.md` に新しいイシューを追加

### イシュー管理のワークフロー

```
エラー発生
    ↓
【必須】イシューファイル作成 ← ここを飛ばしてはいけない
    ↓
エラー調査・原因特定
    ↓
イシューに調査結果を記録
    ↓
修正実装
    ↓
イシューに解決方法を記録
    ↓
ステータスをResolvedに更新
```

### イシュー記録が必要な場面

以下のような場合は**必ず**イシューを作成:

1. **ビルドエラー** - ESLint、TypeScript、Webpack等
2. **実行時エラー** - アプリケーションクラッシュ、API エラー
3. **デプロイエラー** - Docker、Cloud Run、Vercel等
4. **テストエラー** - E2Eテスト、ユニットテスト失敗
5. **データベースエラー** - マイグレーション失敗、接続エラー
6. **環境構築エラー** - パッケージインストール失敗、設定エラー

### イシュー記録のメリット

- 同じエラーに再度遭遇した時の参考資料
- チームメンバーへの知識共有
- トラブルシューティングの体系化
- プロジェクトの学習履歴

**注意**: 「後で記録する」は禁止。エラー発生時に即座に記録すること。

## 利用可能なエージェント

このフォルダでは、以下のカスタムエージェントが利用可能です:

- `/doc-generator`: ドキュメント完全セット生成（要件定義書・仕様書・作業指示書・イシュー管理システム）
- `/best-practices-generator`: 最新ベストプラクティスドキュメント自動生成（WebSearch/WebFetch活用）
- `/debug-helper`: デバッグ専門エージェント（Playwright MCP・Chrome DevTools MCP対応）
- `/requirements-analyst`: 要件分析（存在する場合）
- その他: `.claude/agents/` 配下のエージェント

各エージェントの詳細は `.claude/agents/` を参照してください。

### 主要エージェントの役割

**doc-generator** - プロジェクト立ち上げ時に自動実行
- 新規プロジェクト開発の依頼を受けたら必ず最初に実行
- 要件定義・仕様・作業指示・イシュー管理システムを一括生成

**best-practices-generator** - doc-generator実行後に自動実行
- プロジェクトの技術スタック（package.json）を検出
- 公式ドキュメントから2025年最新のベストプラクティスを収集
- プロジェクト固有の `docs/BEST_PRACTICES.md` を生成

**debug-helper** - エラー・バグ発生時に自動起動
- ビルドエラー、テスト失敗、実行時エラー、デプロイエラー発生時
- Playwright MCPでE2Eテストデバッグ
- Chrome DevTools MCPでブラウザ分析・パフォーマンス計測
- 自動的にイシューファイルを作成し、原因分析・修正提案を実施

## 開発のベストプラクティス

### プロジェクト開始時
1. `/doc-generator` で初期ドキュメント作成
2. 要件定義書でスコープ確認
3. 詳細仕様書で技術詳細確認
4. 作業指示書に従って段階的に実装

### 実装中
- 1つのPhaseを完了してから次へ
- エラーは必ずイシューとして記録
- 定期的にドキュメントを更新

### コミット前
- 作業指示書の完了条件を満たしているか確認
- イシューがすべて解決されているか確認
- テストが通るか確認

## 重要な注意事項

**IMPORTANT**:
- ユーザーが新規プロジェクトの開発を依頼した場合、実装を開始する前に必ず `/doc-generator` の実行を提案すること
- doc-generatorを使わずに開発を開始しないこと
- イシュー管理システムを活用して、問題を体系的に管理すること

## エージェント活用ガイドライン

**重要**: 複雑なタスクに対しては、ユーザーからの依頼がなくても積極的にエージェントを使用すること。以下のシナリオでTaskツールを使用してエージェントを起動すること：

### エージェントを使用すべき場面（自動）

1. **ファイル/コード検索タスク** - `general-purpose`エージェントを使用：
   - 複数ディレクトリにまたがるファイルやシンボルの検索
   - 特定の実装の場所が不明な場合
   - コードベース全体でパターンを検索する場合
   - 最初の2-3回のGlob/Grep試行で見つからない場合

2. **複雑な多段階タスク** - `general-purpose`エージェントを使用：
   - 異なるディレクトリの5つ以上のファイル編集が必要なタスク
   - 大規模なリファクタリング（コンポーネントのリネーム、フォルダ構造の変更など）
   - 複数レイヤーにまたがる新機能の実装（API + DB + UI）
   - 依存関係や使用パターンの追跡が必要な場合

3. **調査タスク** - `general-purpose`エージェントを使用：
   - 大規模で未知のコードベースセクションの理解
   - アーキテクチャやデータフローのドキュメント化
   - 機能が複数ファイルにまたがって実装されている調査

### 並列エージェント実行

- 複数の独立したタスクが存在する場合、単一のメッセージで複数のTaskツール呼び出しを使用してエージェントを並列起動すること
- 例：機能実装にバックエンドとフロントエンドの両方の作業が必要な場合、2つのエージェントを同時に起動

### 使用例

```
ユーザー: "アプリに認証機能を追加して"
→ general-purposeエージェントを起動して既存の認証パターンを検索し、実装

ユーザー: "Userモデルが使用されている場所をすべて見つけて"
→ general-purposeエージェントを起動してコードベースを包括的に検索

ユーザー: "APIルートを一貫したエラーハンドリングパターンにリファクタリングして"
→ general-purposeエージェントを起動して大規模リファクタリング
```

**重要**: エージェントの使用は効率的で期待されています。適切なタスクには躊躇せずに使用してください。

## エージェント自動起動ルール

**重要**: 以下の条件に該当する場合、ユーザーからの指示なしで自動的にエージェントを起動すること。

### デバッグ時（最優先）

以下の場合は**即座に** `/debug-helper` エージェントを起動すること：

1. **ビルドエラー発生時**
   - ESLintエラー
   - TypeScriptコンパイルエラー
   - Webpackビルドエラー
   - Next.jsビルドエラー

2. **テスト失敗時**
   - E2Eテスト（Playwright）失敗
   - ユニットテスト失敗
   - テストがタイムアウト

3. **実行時エラー**
   - アプリケーションクラッシュ
   - 未処理の例外
   - APIエラー
   - データベース接続エラー

4. **パフォーマンス問題**
   - ページ読み込みが著しく遅い
   - メモリリーク疑い
   - 無限ループ検出

5. **デプロイエラー**
   - Dockerビルド失敗
   - Cloud Runデプロイエラー
   - Vercelデプロイエラー

**debug-helperの特徴**:
- Playwright MCPを使用したE2Eテストデバッグ
- Chrome DevTools MCPを使用したブラウザ分析
- スクリーンショット取得
- コンソールログ・ネットワークログ解析
- パフォーマンスメトリクス取得
- メモリリーク検出

**ワークフロー**:
```
エラー検出
    ↓
【自動】debug-helperエージェント起動
    ↓
【必須】イシューファイル作成（debug-helperが実行）
    ↓
原因分析・修正提案
    ↓
修正実装
    ↓
イシューに解決方法を記録
```

### プロジェクト初期化時

新規プロジェクト開始時は、以下のエージェントを**順番に自動実行**すること：

1. **`/doc-generator`** - ドキュメント完全セット生成
   - 要件定義書
   - 詳細仕様書
   - 作業指示書
   - イシュー管理システム
   - 開発ガイド

2. **`/best-practices-generator`** - 最新ベストプラクティス生成
   - プロジェクトの技術スタックを検出
   - 公式ドキュメントから最新情報を収集
   - プロジェクト固有のベストプラクティスドキュメント生成
   - `docs/BEST_PRACTICES.md` に出力

**実行タイミング**:
- ユーザーが「新しいアプリを作りたい」と言った時
- ユーザーが「〇〇システムを開発したい」と言った時
- ユーザーが「プロジェクトを始めたい」と言った時

**実行順序**:
```
ユーザーの新規プロジェクト要求
    ↓
【自動】/doc-generator 提案・実行
    ↓
ドキュメント生成完了
    ↓
【自動】/best-practices-generator 実行
    ↓
最新ベストプラクティス生成完了
    ↓
実装開始
```

### その他の自動起動条件

- **大規模検索タスク**: 2-3回のGlob/Grep試行で見つからない場合 → `general-purpose`エージェント
- **大規模リファクタリング**: 5つ以上のファイル編集が必要 → `general-purpose`エージェント
- **複雑な調査**: アーキテクチャ全体の理解が必要 → `general-purpose`エージェント

## 共通技術スタック

### 推奨技術

プロジェクトで一般的に使用する技術スタック：

#### フロントエンド
- **Next.js** - React フレームワーク（App Router推奨）
- **React** - UIライブラリ
- **TypeScript** - 型安全な開発
- **Tailwind CSS** - スタイリング

#### バックエンド
- **Next.js API Routes** - サーバーサイドAPI（TypeScript/JavaScript）
- **Python** - バックエンドスクリプト・API開発（FastAPI、Flask等）
- **Prisma** - ORM（TypeScript対応）
- **PostgreSQL** - リレーショナルデータベース
- **Firestore** - NoSQLデータベース（Firebase/Google Cloud）

#### インフラ・デプロイ
- **Supabase** - PostgreSQL ホスティング
- **Firebase/Firestore** - NoSQLデータベース・認証・ホスティング
- **Docker** - コンテナ化
- **Google Cloud Run** - サーバーレスデプロイ
- **Vercel** - Next.js最適化ホスティング（代替）

#### テスト
- **Playwright** - E2Eテスト
- **Jest** - ユニットテスト（オプション）
- **pytest** - Pythonユニットテスト（Python使用時）

## 開発ベストプラクティス

### E2Eテスト

#### Playwright設定
```typescript
// テストは常にシリアル実行を推奨（DB状態の競合を防ぐ）
test.describe.configure({ mode: 'serial' })

// テスト前にデータベースをクリーンアップ
test.beforeEach(async () => {
  await resetDatabase()
})
```

**重要**: 並列実行は避ける。データベース状態の競合が発生する可能性が高い。

### Docker・デプロイ

#### マルチステージビルド
```dockerfile
# 依存関係のインストールステージを分離
FROM node:20-alpine AS deps
# ビルドステージを分離
FROM node:20-alpine AS builder
# 実行ステージは最小限に
FROM node:20-alpine AS runner
```

**利点**: 最終イメージサイズを大幅削減

#### Next.js Standaloneモード
```typescript
// next.config.ts
export default {
  output: 'standalone',  // Docker用最適化
  eslint: {
    ignoreDuringBuilds: true,  // Prisma生成ファイルのエラー回避
  },
}
```

### データベースベストプラクティス

#### ビルド時のDB接続を避ける
```typescript
// ページで動的レンダリングを強制
export const dynamic = 'force-dynamic'
```

**理由**: ビルド時にDATABASE_URLが不要になる

#### Lazy Initialization（オプション機能）
```typescript
// オプション機能は遅延初期化
let instance: Service | null = null

function getInstance() {
  if (!instance && process.env.API_KEY) {
    instance = new Service(process.env.API_KEY)
  }
  return instance
}

export async function useService() {
  const service = getInstance()
  if (!service) {
    console.log('Service not available. Skipping.')
    return { success: true, skipped: true }
  }
  // 処理続行
}
```

**利点**: API KEYがなくてもアプリがビルド・実行可能

### TypeScriptベストプラクティス

#### any型を避ける
```typescript
// ❌ Bad
const updateData: any = {}

// ✅ Good
const updateData: {
  title?: string
  description?: string | null
  completed?: boolean
  completedAt?: Date | null
} = {}
```

### エラーハンドリングパターン

#### 共通エラーパターンと解決策

1. **ESLint ビルドエラー**
   - 原因: Prisma生成ファイルがESLintに引っかかる
   - 解決: `next.config.ts`で`eslint.ignoreDuringBuilds: true`

2. **ビルド時にDATABASE_URLが見つからない**
   - 原因: ページの静的生成時にDB接続が必要
   - 解決: `export const dynamic = 'force-dynamic'`

3. **ビルド時にAPI Keyが見つからない**
   - 原因: モジュールレベルでインスタンス生成
   - 解決: Lazy initialization パターン

4. **Artifact Registryリポジトリが見つからない**
   - 原因: 手動でDocker pushしようとした
   - 解決: `gcloud run deploy --source .`を使用

### Cloud Runデプロイ

#### 推奨デプロイコマンド
```bash
gcloud run deploy <service-name> \
  --source . \
  --region asia-northeast1 \
  --allow-unauthenticated \
  --set-env-vars="DATABASE_URL=..."
```

**利点**: Artifact Registryリポジトリを自動作成

#### 環境変数管理
```bash
# 本番環境変数を.env.productionに保存
DATABASE_URL="postgresql://..."

# デプロイ時に明示的に指定
--set-env-vars="DATABASE_URL=..."
```

### Gitワークフロー

#### .gitignoreに含めるべきもの
```
node_modules
.next
.env.local
.env.production
e2e/test-results
playwright-report
```

#### コミット前チェックリスト
- [ ] すべてのテストが通る
- [ ] ESLintエラーがない（またはビルド時無視設定）
- [ ] イシューがすべて解決済み
- [ ] 環境変数が適切に設定されている

## トラブルシューティングガイド

### よくあるエラーと解決方法

#### ビルドエラー
1. ESLintエラー → `ignoreDuringBuilds: true`
2. TypeScriptエラー → 型定義を明示的に
3. Prismaエラー → `npx prisma generate`実行

#### デプロイエラー
1. Dockerビルド失敗 → Dockerfileの各ステージを確認
2. Artifact Registryエラー → `--source`フラグを使用
3. データベース接続失敗 → 環境変数を確認

#### テストエラー
1. テストが互いに干渉 → `mode: 'serial'`
2. データベース状態の問題 → `resetDatabase()`を追加
3. タイムアウトエラー → `timeout`を延長

## 更新履歴

- 2025-10-07: エージェント自動起動ルール追加（デバッグ時・プロジェクト初期化時）。debug-helper自動起動、best-practices-generator自動実行の設定完了。エージェント活用ガイドライン、共通技術スタック、開発ベストプラクティス、エラーイシュー管理強化ルール追加。全文日本語化完了
- 2025-10-06: 初版作成、doc-generator自動提案ルール追加
