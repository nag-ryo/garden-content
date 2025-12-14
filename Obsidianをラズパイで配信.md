## ラズパイ側に受け口を作る
デプロイ用ユーザー作成
```
sudo adduser --disabled-password --gecos "" deployer
sudo usermod -aG www-data deployer
```

公開用ディレクトリ作成
```
sudo mkdir -p /var/www/garden
sudo chown -R deployer:www-data /var/www/garden
sudo chmod -R 775 /var/www/garden
```

SSH鍵ログインを許可

GitHub Actionsから入る鍵の公開鍵を`~deployer/.ssh/authorized_keys`に入れる
```
sudo -u deployer mkdir -p /home/deployer/.ssh
sudo -u deployer chmod 700 /home/deployer/.ssh
sudo -u deployer nano /home/deployer/.ssh/authorized_keys
sudo -u deployer chmod 600 /home/deployer/.ssh/authorized_keys
```

## GitHub Actions用のSSH鍵を作る

Macで鍵を作って、秘密鍵はGitHub Secretsヘ、公開鍵はラズパイへ。
```
ssh-keygen -t ed25519 -C "github-actions-garden" -f ./github-actions-garden -N ""
```

- github-actions-garden（秘密鍵）→ GitHub Secretsへ
- github-actions-garden.pub（公開鍵）→ ラズパイの authorized_keys へ貼る

ラズパイに公開鍵を入れるならMacからこうしてもいい
```
ssh-copy-id -i ./github-actions-garden.pub deployer@<RASPI_IP>
```

## Nginxで配信する

Nginxインストール
```
sudo apt update
sudo apt install -y nginx
```

