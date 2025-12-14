#note 
## Phase.1 公開の準備
### ゴール
- ローカル環境でビルドできる

Quartzを使用する。
理由
- Obsidianの思想と一致している
- Inboxだけでも成立する
- 軽い
- あとからデザインを拡張できる

GitHubリポジトリを１つ作る。
nag-ryo/garden

ローカルで一度ビルドしてみる。

## Phase.2 ラズパイへのデプロイ
### ゴール
- Raspberry Pi のIP or ローカルDNSで見える
- Nginx / Caddy からHTMLが配信される

### 不要なもの
- 独自ドメイン
- HTTPS
- 自動化

scpでも、rsyncでもいいので、動いていることを目指す。

## Phase.3 公開URLを安定させる

### 選択肢
- 自宅回線 + ポート開放
- Cloudflare Tunnel
- VPSへ逃がす（将来）

たぶんCloudflare Tunnel
理由
- 自宅IP非公開
- HTTPS自動
- ドメイン後付けが簡単

学習目的の比重を高めるなら、自宅回線でやりたい。

## Phase.4 独自ドメイン

1. ドメイン取得（あるいは既存ドメインを使用）
2. DNS設定
3. Cloudflare に紐づけ


