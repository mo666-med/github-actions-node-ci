# Github_Actions Templates

このフォルダは、任意のNode/Next.jsプロジェクトで再利用できるCIテンプレートを提供します。

## 含まれるファイル
- `.github/workflows/node-ci-reusable.yml` : 再利用可能ワークフロー本体（workflow_call）
- `.github/workflows/ci.yml` : 呼び出し用の最小エントリ（同一リポ内での例）

## 使い方（同一リポ内）
`.github/workflows/ci.yml` の `uses:` が、同フォルダの `node-ci-reusable.yml` を参照します。

## 使い方（別リポから再利用）
このテンプレートを別リポジトリに配置し、タグ `v1` を付与した上で、呼び出し側で以下のように指定します。

### Google Sheets 連携（任意）
- リポジトリの Secrets に `GOOGLE_SERVICE_ACCOUNT_JSON`（サービスアカウントJSON文字列）を登録
- 呼び出し時に以下を指定（必要に応じて）
  - `sheets-enabled: true`
  - `sheets-spreadsheet-id: '<your-spreadsheet-id>'`
  - `sheets-worksheet: 'Sheet1'`
- 成功/失敗ごとに1行追記（時刻/リポジトリ/ブランチ/結果/SHA）

```yaml
jobs:
  node-ci:
    uses: owner/repo/.github/workflows/node-ci-reusable.yml@v1
    with:
      node-version: '20'
      working-directory: '.'
      run-lint: true
      run-test: true
      run-build: true
```

## オプション
- `node-version` (default: `20`)
- `os` (default: `ubuntu-latest`)
- `working-directory` (default: `.`)
- `run-lint` / `run-test` / `run-build` (default: `true`)
- `pkg-manager` : 空なら自動判別（pnpm > yarn > npm）
- `extra-setup` : 任意の事前セットアップコマンド

## 注意
- `package.json` にスクリプトが無い場合は自動スキップします。
- プライベートレジストリ利用時は `NPM_TOKEN` 等のSecretsが必要です。
