# Timecode Display for OBS

現在のタイムコード（時刻）をリアルタイム表示するシンプルな1ファイルWebページです。
文字色・サイズ・フォント・縁取りなどを自由に変更でき、**背景透過**に対応しているので
OBSのブラウザソースに重ねて使えます。

## 使い方

### ローカルで開く
`public/index.html` をブラウザで開くだけです。

### OBSで使う
1. ソース → **ブラウザ** を追加
2. **ローカルファイル** にチェックを入れ、`public/index.html` を指定
   （もしくはCloudflare Pages等で公開したURLを指定）
3. 幅・高さを配信解像度に合わせる（例: 1920 × 1080）
4. 背景設定が「透過」になっていれば、そのまま透けて合成されます

> OBSの「カスタムCSS」欄は **空のままでOK** です。

## 設定パネル

- 画面右上の **歯車アイコン**、または **S キー** で設定パネルを開閉します。
- パネルはOBSのキャプチャには映りません（操作用UIです）。
- 変更内容はブラウザに自動保存されます。

### 変更できる項目
- 表示形式（時:分:秒 / ミリ秒付き / フレーム付きタイムコード / 時:分）
- フレームレート（タイムコード形式のとき）
- 24時間 / 12時間表示、日付の表示
- 文字色、サイズ、フォント、太さ、文字間隔
- 縁取り・影（色とぼかし量）
- 背景（透過 / 単色）
- 透過確認用の市松模様（編集中だけ表示）

## 複数シーンで使い分ける

設定パネルの **「設定付きURLをコピー」** ボタンで、現在の見た目をすべて
クエリパラメータに含んだURLを生成できます。OBSの各ブラウザソースに別々のURLを
指定すれば、シーンごとに色やサイズを変えられます。

例: `index.html?format=HMSf&color=%2300ff66&size=14&shadowOn=1`

## Cloudflare Pages へのデプロイ

静的サイトなのでビルド不要です。配信ディレクトリは `public/` です。

### A. CLIで直接デプロイ（最速）
[APIトークン](https://dash.cloudflare.com/profile/api-tokens)（**Cloudflare Pages: Edit** 権限）と
アカウントIDを用意して、次を実行します。

```bash
export CLOUDFLARE_API_TOKEN=xxxxxxxx
export CLOUDFLARE_ACCOUNT_ID=xxxxxxxx
npx wrangler pages deploy public --project-name=web-times
```

### B. GitHub Actionsで自動デプロイ
`.github/workflows/deploy.yml` を同梱しています。GitHubリポジトリの
**Settings → Secrets and variables → Actions** に以下を登録すると、push時に自動デプロイされます。
- `CLOUDFLARE_API_TOKEN`
- `CLOUDFLARE_ACCOUNT_ID`

### C. ダッシュボードでGit連携
Cloudflareダッシュボード → **Workers & Pages → Create → Pages → Connect to Git** で
このリポジトリを選択。ビルドコマンドは空、**ビルド出力ディレクトリ**を `public` に設定します。
（トークンを共有せずに済む方法です）
