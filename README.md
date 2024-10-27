# iap-token-getter
Identity Aware Proxy で保護したリソースに対して M2M の REST API を通すための認証トークンを取得する方法。  
ポイントは IAP で保護されたアプリへの認証トークンを取得するときにシェルスクリプトを実行する必要があるところで、このリポジトリではトークン取得のためのシェルスクリプトを扱う。

## 使い方

### 前提
- あらかじめIAPを設定したサーバは用意しておく

### Google Cloud の準備
- IAP での API アクセス用 Service Account を作成
- Service Account のキーファイルをJSON形式で作成しダウンロード
- IAPを開き、対象リソース > 「プリンシパルを追加」で SA のアカウント(example@hogehoge)を指定
- アカウントには IAP-secured Web App User のロールを付与して保存
- IAPの画面に戻り、設定するIAPの3点メニューから「OAuth 構成」に移動、「クライアントID」を取得
    - 「OAuth 構成」がグレーアウトしている場合がある。その場合は IAP を一旦無効化、「OAuth 構成」が選択できるようになるので選択してクライアントIDを取得、その後再度IAPを有効化する。

### ローカルのスクリプトでIAPで保護されたアプリへの認証トークンを取得する（ここがキモ！）
- 本プロジェクトの `iap_token_gen.sh` の環境変数 `.env` に、キーファイルのパス・クライアントID・アクセスしたいREST APIのURLを入力する
- bash で実行するとトークンが表示される（めちゃくちゃ長い）
- 表示されるトークンをコピーする

### 疎通を確認する
- `curl -XGET -H 'Proxy-Authorization: Bearer <IAP-token>' <アクセスしたいREST APIのURL>` でIAPで保護されたアプリに対して通信が通ることを確認する
- 通ればOK

# 参考
- https://qiita.com/ikea_shark_jk/items/9b0e8753648a6fd47e4d