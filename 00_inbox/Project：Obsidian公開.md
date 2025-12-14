#note 
開始日：2025/12/14 10:00
終了日：2025/12/14 23:10
## 目的
- Obsidianを公開して、外部へナレッジや情報を公開する
- 自宅のサーバーでホスティングする技術を習得する
## 現在揃っているもの
- ガーデンの思想
- 常時稼働しているRaspberry Pi

[[Obsidian公開までのステップ]]

## ログ
### [[2025-12-14]]
- やること
	- Quartzをclone
	- gardenを流し込む
	- index.mdが表示されるか確認
- Quartzは`~/my-project/garden`にクローン
- node.jsを使用しているので`npm i`を実行
- まずは素のQuartzが動くか確認
	- `npx quartz build --serve`
	- `http://localhost:8080`にアクセスすると、404ページが開く
- Vaultを載せ替えて動作確認
	- Quartzの公開されるもの
		- content/
			- index.md
			- posts/
	- 最小構成にしてみる
		- contentの中にindex.mdだけ作成して、再ビルド
		- 表示に成功した。
- 仕組みは理解した。content内がVaultの中身である。
- 現状の疑問点
	- contentの中を最新化する方法
		- まさか手作業で毎回コピペするわけじゃあるまいし。
		- Obsidianのコミュニティプラグインを組み合わせるのか？
			- Obsidian→自分のGitHub→githubActionでビルド、デプロイ
			- のはず。
- 最小コストの方法を調べた。
	- contentの中をVaultにして、そこをObsidianで直接編集する。
	- Obsidian→content/直編集→Git push→CI/CD
- 流れの案
	- 方式１：GitHub Actionsでビルド→ラズパイへデプロイ
		- Obsidian(Quartz repo)
			- GitHub push
			- GitHub Actionsで npx quartz build
			- 生成物`public/`をラズパイに転送
			- Nginx/Caddy で配信
		- 転送方法の定番２つ
			- rsync over SSH（差分転送で速い）
			- scp（最初だけならOK）
	- 方式２：ラズパイでpullしてビルド
		- 流れ
			- ラズパイでgit pull
			- npm ci && npx quartz build
			- 配信
		- デメリット
			- ビルド環境をラズパイに維持する必要がある
			- Nodeの更新が増える
			- ビルド失敗時の復旧が面倒
- リポジトリをVaultにすると、iCloudに置けない
	- MacとiPhoneの両刀使いが実現できない。
	- iCloudとgitの相性は悪い。競合する。
	- VaultをiCloudに置くことは必須条件。
	- iCloudにはVaultだけ置きたい。
- もしかして、gardenのリポジトリはローカルで意識しなくてもいいのでは？
	- iCloud/Obsidian/gardenをgit管理して、git pushだけMacでやればいい？
		- garden-contentを別リポジトリにして、submoduleとして読み込む。
	- あるいはシンボリックリンク
		- VaultはiCloudに置く
		- Macのmy-project/gardenのcontent配下をiCloudから読み込む。
- まとめる
	- iCloudに"garden-content"を置いて、macやiphoneのObsidianでそれをVaultとする。
	- Githubにリポジトリ作成
		- garden-quartz
		- garden-content
	- iCloudのgarden-contentはgit管理する。macでgit commit && git push
	- GithubActionsでCI/CD
- Githubにそれぞれリポジトリは作った。
- garden-quartzでactionsを入れる。
	- まずはビルドまで。
	- 流れ
		- garden-quartzをcheckout
		- garden-contentをcheckout
		- garden-quartz/content/ を garden-content で置き換え
		- npx quartz build
		- （あとで）ラズパイへデプロイ
- [[Obsidianをラズパイで配信]]
- CloudFlareで公開に成功。
- ドメインは過去にブログで使っていたものを流用。
- もう寝る時間なので、全体的な流れは後でまとめる。