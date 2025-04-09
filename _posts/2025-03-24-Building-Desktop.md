---
title: "Building Desktop PC"
date: "2025-03-24 00:00:00 +0900"
categories: [Hardware, Desktop]
tags: [Desktop]
---

# Building Desktop PC

## 概要

背景：
- 軽量Linuxサーバー, NAS, マイニング等様々な用途に対応できる環境を作りたかった
- 出来るだけ低コストで試験的に実装してみたかった。
- マザーボードやGPUなどの理解も同時に深めたかった

結果：
- 

所感：
- 

## 1. 構成

- ケース、ストレージ(6TB HDD, 500GB HDD)、WiFiカードは転用で余っていたパーツを利用 
- 実質：49,763円 


| パーツ                           | 型番                                                        | 値段   |
|----------------------------------|-----------------------------------------------------------|--------|
| **タワーケース**                 | IW-PE663                                                   | 0      |
| **マザーボード + CPU + メモリセット** | QIYIDA X99 Motherboard + Xeon E5 2680 V4 + 32GB DDR4 REG ECC | 11,068 |
| **電源ユニット（PSU）**           | Thermalright TR-TGFX850 750W                               | 11,071 |
| **CPU冷却**                       | Lightless-3fan (LGA2011-X79-X99-E5対応)                   | 1,292  |
| **GPU**                           | SOYO AMD Radeon RX5700XT 8GB GDDR6 Memory                  | 20,147 |
| **6TB HDD**                       | ST6000DM003                                               | 14,800 |
| **500GB HDD**                     | ST500LM021                                                | 1,000  |
| **無線LANカード**                 | Intel 8265NGW                                            | 500    |
| **HUADISK NVMe M.2 SSD 1TB**     | HUADISK NVMe M.2 SSD 1TB (read:2100MB/s)                   | 5,185  |
| **合計**                          |                                                           | 66,063 |



## マザーボード

- マザボの選び方：
- 1. 使いたいCPUを決める
- 2. CPUに対応したチップセットを決める
- 3. マザーボードを決める（ソケットタイプに注意）

- CPU Socket Type：
  - 型番：`LGA2011-v3 (Socket R3)`
  - CPUとマザーボードを接続するための物理的な規格で、マザーボードには、対応するCPUソケットがあり、対応するCPUしか取り付けられない。CPUクーラーを選ぶ際も注意する。
  - [LGA2011詳細](https://ja.wikipedia.org/wiki/LGA2011)

- Chipset：
  - 型番：`Intel X99 Express`
  - 役割：CPUやメモリ、PCIe、USB、SATAなどの接続を制御する役割
  - 対応するCPUやメモリの種類があり、事前に調べる必要がある
    - [Intel X99 Express 対応CPU一覧](https://www.intel.com/content/www/us/en/products/sku/81761/intel-x99-chipset/compatible.html) + Intel Xeon E5 v3 / v4 シリーズ（Haswell-EP / Broadwell-EP）: Xeon E5-1600 v3/v4 + Xeon E5-2600 v3/v4

- Size：
  - `QIYIDA X99 Motherboard`は`Micro-ATX`
  - サイズ：`ATX` / `Micro-ATX` / `Mini-ITX`
    - ケース選びにも関わってくる部分
    - `ATX`: 標準的なサイズで、拡張性に優れており、組みやすい。キャプチャーボードを入れたいならこれ。
    - `Micro-ATX`: 少し小さめで、手ごろなモデルが多い。
    - `Mini-ITX`: 小さい。拡張性は無く、値段は高め。

- RAM(Random Access Memory)：
  - 購入メモリ：`DDR4-3200 REG ECCメモリ（サーバーやワークステーション向け）16GB×2`
　- 役割：CPUが計算や処理を行うために必要なデータを一時的に保存

- VRMフェーズ
  - 8つ確認
  - 役割：CPUに供給される電圧を調整するパーツで数が多いほど電圧を一定に保てる。

- メモ
  - `Chipset`が様々なCPUに対応していても`Socket Type`によって取り付けられるCPUが決まる
  - `Chipset`が様々なMemory(DDR4, DDR5など)に対応していても、マザーボードによってどちらかしか対応してない場合がある。

## 3. M.2 SSDにUbuntu Server 22.04 LTSをインストール

- M.2 SSDの準備
  - Windowsキー + X を押して「ディスクの管理」を開く
  - GPTを選ぶと、初期化されて未割当の状態になる。
    - ![alt text](../assets/images/Screenshot_2025-03-28_180802.png)
- USBの準備
  - OSのisoをダウンロード：[Ubuntu Server 22.04 LTS](https://ubuntu.com/download/server)
  - RufusでisoをUSBに焼く
    - ![alt text](../assets/images/Screenshot_2025-03-28_182302.png)
- 設定とインストール
  - サブ機のBIOSに入ってbootの順番を変える。(USB)
  - 再起動して、USBからブートする
  - Try or Install Ubuntu Serverを選ぶ
  - 動画を参照しながらインスト―ルを行う

## 4. 構築

- system information確認
  - `landscape-sysinfo`
- ネットワーク設定：
  - `sudo nano /etc/netplan/50-cloud-init.yaml`
  - `sudo netplan apply`
- GUIのインストール
  - `sudo apt install xfce xfce4-goodies xrdp -y`
  - `sudo systemctl enable xrdp`
- Login画面のインストール
  - `sudo apt install gdm3`
  - `sudo dpkg-reconfigure gdm3`
  - `sudo systemctl start gdm3`
  - `sudo reboot`
  - GUIが表示されたら成功
- Wi-Fiを表示可能に設定を変更
  - `sudo nano /etc/NetworkManager/NetworkManager.conf`
  - `managed=true`に変更
  - `sudo nano /etc/netplan/50-cloud-init.yamal`
    - `renderer: NetworkManager`を`version`の下に追加
  - `sudo netplan apply`
  - `sudo systemctl restart NetworkManager`
  - `nmcli device`: connectedになっていれば成功
  - GUIのSystemからWi-Fiの表示を確認
- ブラウザ
  - `sudo apt install firefox`
- Nextcloud
  - 準備：
    - HDDをクラウドで利用できるようにする。ここにObsidianのvaultをおいてどのデバイスからでもアクセス可能にさせる
    - Linuxでは、すべてのストレージは「どこかのフォルダ」に“マウント”しないと使えないので、どこかのフォルダにマウントポイントを指定する
    - `lsblk`：デバイス名の確認
    - `sudo mount /dev/sdb1 /mnt/nextcloud`
      - `/dev/sdb1`(HDDのパーティション)を`/mnt/nextcloud`を通じて中見を見れる
    - `df -h | grep nextcloud`：マウントされているか中身を確認。
      - 昔のデータが入っていたので、完全に削除してフォーマットし、クリーンな状態にする
      - `sudo umount /mnt/nextcloud`
      - `sudo fdisk /dev/sdb` → `d` → `w`: パーティションを削除
      - `sudo fdisk /dev/sdb` → `n` → `w`: パーティションを新しく作成
      - `sudo mkfs.ext4 /dev/sdb1`: ファイルシステムを新しく作成（フォーマット）
    - もう一度マウント：`sudo mount /dev/sdb1 /mnt/nextcloud`
 - Nextcloudをインストールしていく
   - MariaDBをインストール
     - 用途：
       - Nextcloudのデータを保存するために必要な**データベース管理システム（DBMS）**
       - Nextcloudは、ユーザー情報、ファイルメタデータ、設定情報、アプリケーションの設定など、多くの情報をデータベースに保存する
     - `sudo apt install mariadb-server -y`: インストール
     - `sudo systemctl daemon-reload`
     - `sudo mysql_secure_installation`: MariaDBのセキュリティ設定
   - NextcloudのデータベースをMariaDBに作成
     - `sudo mysql -u root -p`: MariaDBにログイン
     - `CREATE DATABASE nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;`: Nextcloud用のデータベースを作成
     - `SHOW DATABASES;`: DBが作成されたか確認
     - `CREATE USER 'bentham'@'localhost' IDENTIFIED BY 'yourpassword';`: Nextcloud用のデータベースユーザーを作成
     - `SELECT User, Host FROM mysql.user;`: ユーザーが作成されたか確認
     - `GRANT ALL PRIVILEGES ON nextcloud.* TO 'bentham'@'localhost';`: 適切な権限を付与
     - `SHOW GRANTS FOR 'yourusername'@'localhost';`: 権限が適切に設定されたのか確認
     - `FLUSH PRIVILEGES;`: 変更を適応
     - `EXIT;`: MariaDBを終了
   - ApacheとPHPをセットアップ：
     - 用途：Nextcloudをホストするために、ApacheとPHPをインストールする必要がある
     - `sudo apt install apache2 libapache2-mod-php -y`: インストール
     - `sudo apt install php php-gd php-json php-mysql php-curl php-xml php-zip php-mbstring php-bz2 php-intl php-imagick php-xmlrpc php-gmp php-soap -y`: PHPの必要な拡張モジュールをインストール
     - `sudo systemctl enable apache2`: Apacheを有効化
     - `sudo systemctl start apache2`: Apacheを起動
     - `sudo systemctl status apache2`: 起動確認
   - Nextcloudのダウンロードとインストール
     - `cd /var/www/html`
     - `sudo wget https://download.nextcloud.com/server/releases/nextcloud-31.0.2.zip`
     - `sudo apt install unzip`
     - `sudo unzip nextcloud-31.0.2.zip`: 解凍
     - `sudo chown -R www-data:www-data /var/www/html/nextcloud`: NextcloudのファイルをApacheがアクセスできるように所有権を設定
     - `sudo nano /etc/apache2/sites-available/nextcloud.conf`: NextcloudをWebサーバーで動作させるために、Apacheを設定
     - ![alt text](../assets/images/Screenshot_2025-04-04_183509.png)
     - Apacheの設定を有効にして、Apacheを再起動
       - `sudo a2ensite nextcloud.conf`
       - `sudo a2enmod rewrite`
       - `sudo systemctl restart apache2`
   - Nextcloudのセットアップ
     - ブラウザで、`http://{your-server-ip}/nextcloud` にアクセス
     - Nextcloudのインストール画面が表示されるので、設定。
     - installをクリック：Error `cannot create or write into the data directory /mnt/nextcloud`
       - `sudo chown -R www-data:www-data /mnt/nextcloud`: データディレクトリの所有者を変更
       - `sudo chmod -R 755 /mnt/nextcloud`: 適切な権限を設定
     - installをクリック → ページ(`http://nextcloud/index.php/core/apps/recommended`)に移動された
       - Error: `Hmm. We're having trouble finding that site`
       - `sudo mount /dev/sdb1 /mnt/nextcloud`：マウントをやり直す
       - `sudo nano /etc/fstab`: 永続的なマウントを設定
         - `/dev/sdb1 /mnt/nextcloud ext4 rw,auto,user,exec,suid 0 2`
       - `sudo mkdir /mnt/nextcloud/data`: /mnt/nextcloud ディレクトリ内にデータディレクトリを作成
       - www-data ユーザーとグループに所有権を変更
         - `sudo chown -R www-data:www-data /mnt/nextcloud`
         - `sudo chmod -R 755 /mnt/nextcloud`
         - `sudo touch /mnt/nextcloud/.ncdata`: 手動で .ncdata を作成
         - `echo "# Nextcloud data directory" | sudo tee /mnt/nextcloud/.ncdata`
       - ダッシュボードにアクセス出来た：`http://{your-server-ip}/nextcloud/index.php/apps/dashboard/`
  - Nextcloudを設定：
    - `Security & Setup warnings`にはエラーが表示されているので、上から解消していく
    - 赤エラー:1
      - ![alt text](../assets/images/Screenshot_2025-04-05_152159.png)
      - 解消：
        - `sudo -u www-data php /var/www/html/nextcloud/cron.php`: 手動で実行し成功。
        - ダッシュボードで更新してエラー解消を確認
        - `sudo crontab -u www-data -e`: 自動実行を設定
          - `*/5 * * * * php -f /var/www/html/nextcloud/cron.php`
          - `www-data` ユーザーが5分ごとに`cron.php`をコマンドラインで実行するようにした。
          - つまり、Nextcloudのバックグラウンド処理が自動で定期実行される状態
    - 赤エラー2: 
      - `Accessing site insecurely via HTTP. You are strongly advised to set up your server to require HTTPS instead. Without it some important web functionality like "copy to clipboard" or "service workers" will not work! For more details see the documentation`
      - 解消：自己署名証明書でHTTPS化（ローカル向け）
        - OpenSSLで証明書を作る
          - `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nextcloud-selfsigned.key -out /etc/ssl/certs/nextcloud-selfsigned.crt`
            - Common Name には nextcloud.local とか your-server-ip を書くと吉
        -  ApacheにSSL設定を追加
          - `sudo nano /etc/apache2/sites-available/nextcloud-ssl.conf`
          - ![alt text](../assets/images/Screenshot_2025-04-05_152159.png)
        - サイトを有効化してApache再起動
          - `sudo a2enmod ssl`
          - `sudo a2ensite nextcloud-ssl.conf`
          - `sudo systemctl restart apache2`
        - ブラウザでアクセス：`https://your-server-ip/nextcloud でアクセス`
          - 「この接続は安全ではありません」みたいな警告は出るけど、「続行」を押せばOK（自己署名証明書だから）
    - 赤エラー3: 
      - `The PHP memory limit is below the recommended value of 512MB. Some features or apps - including the Updater - may not function properly.`
      - 解消：
        - `sudo apt install libapache2-mod-php8.3`: Apache + PHP 8.3 用のモジュールをインストール
        - `php -v`: phpのバージョン確認 → `8.3.6`
        - `php --ini`: 該当の php.ini を探す
        - `sudo nano /etc/php/8.3/apache2/php.ini` → `memory_limit = 512M`に変更
        - `sudo systemctl restart apache2`: Apacheを再起動
    - 黄エラー: 1
      - `Server has no maintenance window start time configured. This means resource intensive daily background jobs will also be executed during your main usage time. We recommend to set it to a time of low usage, so users are less impacted by the load caused from these heavy tasks`
      - 解消：
        - `sudo nano /var/www/html/nextcloud/config/config.php`
          - `'maintenance_window_start' => 5,`: UTCで午前5時、つまり日本時間で午前2時に設定
    - 黄エラー: 2
      - `One or more mimetype migrations are available. Occasionally new mimetypes are added to better handle certain file types. Migrating the mimetypes take a long time on larger instances so this is not done automatically during upgrades. Use the command occ maintenance:repair --include-expensive to perform the migrations`
      - 解消：
        - `sudo -u www-data php /var/www/html/nextcloud/occ maintenance:repair --include-expensive`
    - 黄エラー：3
      - `Some headers are not set correctly on your instance - The Strict-Transport-Security HTTP header is not set (should be at least 15552000 seconds). For enhanced security, it is recommended to enable HSTS.`
      - 解消：
        - nextcloud.conf ファイル（HTTPの設定）を編集: HTTP（ポート80）へのアクセスをHTTPS（ポート443）にリダイレクトするために、nextcloud.conf ファイルを編集
          - `sudo nano /etc/apache2/sites-available/nextcloud.conf`
          - `<VirtualHost *:80>` セクションに以下の設定を追加
          - `Redirect permanent / https://nextcloud.local/`
        - nextcloud-ssl.conf ファイル（HTTPSの設定）を編集: 次に、HTTPS（ポート443）でアクセスされた場合にHSTSヘッダーを設定
          - `sudo nano /etc/apache2/sites-available/nextcloud-ssl.conf`
          - `Header always set Strict-Transport-Security "max-age=15552000; includeSubDomains; preload"`: 追加
        - headers モジュールを有効にする
          - `sudo a2enmod headers`
        - Apacheの再起動
          - `sudo systemctl restart apache2`
    - 黄エラー：4
      - `The PHP OPcache module is not properly configured. The OPcache buffer is nearly full. To assure that repeating strings can be effectively cached. it is recommended to apply "opcache.interned_strings_buffer" to your PHP configuration with a value higher than "8"`
      - 解消：
        - `sudo nano /etc/php/8.3/apache2/php.ini` 
        - `sudo nano /etc/php/8.3/apache2/php.ini` → `opcache.interned_strings_buffer = 16`に変更
          - `Ctrl` + `W` → `interned_strings_buffer`で検索できる
        - `sudo systemctl restart apache2`
    - 細かい設定：
      - `http://192.168.1.110`にアクセスしたときにapache2のデフォルトページに入ってしまう。
        - 解消：
          - `sudo a2dissite 000-default.conf`
          - `sudo systemctl restart apache2`
      - `png`や`pdf`がnextcloud上で表示出来ない
        - 解消：
          - `sudo apt install php-imagick ghostscript -y`
          - `sudo systemctl restart apache2`
    - Nextcloudの構造
      - ダッシュボードの「All files」で見えるファイル: `/mnt/nextcloud/bentham/files`
- 外出先から自宅のNextcloudサーバーへTailscaleでアクセス出来るようにする
  - Tailscaleをインストール
    - `curl -fsSL https://tailscale.com/install.sh | sh`
  - サービス起動
    - `sudo tailscale up`
    - 初回実行時にログイン用のURLが表示されるので、それをブラウザで開いてGoogleアカウントなどで認証
  - 外出先のノートPC側（Archなど）にも導入
    - `sudo pacman -S tailscale`
    - `sudo systemctl enable --now tailscaled`
    - `sudo tailscale up`: サーバーと同じ方法で認証
  - 接続の確認
    - `tailscale status`
  - ArchのlaptopからTailscaleを利用してサーバーのNextcloud上のObsidian Vaultフォルダを指定する
    - Server側の準備：
      - SSHサーバインストール: `sudo apt install openssh-server`
      - 起動：`sudo systemctl start ssh`
      - サーバーのSSHポートが開いているか確認: `sudo ufw allow 22`
    - Arch側の準備
      - SSHキー作成：`ssh-keygen -t rsa -b 4096`
        - 生成した鍵ペアは`~/.ssh/id_rsa`と`~/.ssh/id_rsa.pub`に保存される
      - 公開鍵をサーバーに追加
        - `ssh-copy-id bentham@{Tailscale_IP}`
    - archからSSHでサーバに接続
      - `ssh bentham@{Tailscale_IP}`: 接続完了
    - ファイルシステムへのマウント
      - `/mnt/nextcloud/bentham/files/obsidian_vault`フォルダをArchのローカルにマウント
        - `sudo pacman -S sshfs`: SSHFSをインストール
        - `mkdir ~/nextcloud_vault`: マウントするディレクトリを作成
        - `chmod 755 ~/nextcloud_vault`
        - `sshfs bentham@100.120.163.68:/mnt/nextcloud/bentham/files/obsidian_vault ~/nextcloud_vault`
          - これでArch obsidianでvaultを指定できる
        - `Permission Denied`で上手くいかない。権限変更しても不可
- Nextcloud + WebDAV + Tailscale の構成でやってみる
  - davfs2 パッケージをインストール: `yay -S davfs2`
  - マウントポイントの作成：`mkdir -p ~/nextcloud_vault`
  - 自己署名証明書を許可する: `sudo nano /etc/davfs2/davfs2.conf`
    - `use_locks 0`にする
  - Server側の設定：
    - `sudo nano /var/www/html/nextcloud/config/config.php`
      - tailscaleのサーバーのIPを追加：`2 => 'tailscale_IP'`
    - `sudo systemctl restart apache2`
  - nextcloudのフォルダをローカルにマウント
    - `sudo mount -t davfs https://{tailscale_IP}/remote.php/dav/files/bentham/ ~/nextcloud_vault`
    - `ls -la /nextcloud_vault`：nextcloudのフォルダが表示されたら成功。
  - 自動的にマウントされるように設定する
    - `sudo umount /home/bentham/nextcloud_vault`: いったんマウント解除
    - `sudo mkdir Nextcloud`: マウントポイント作成
    - `sudo nano /etc/fstab`
      - `https://{tailscale_IP}/remote.php/dav/files/bentham/  /home/bentham/Nextcloud  davfs  _netdev,auto,rw,user  0  0`
    - `sudo nano /etc/davfs2/secrets`:認証情報を設定
      - `https://{tailscript_IP}/remote.php/dav/files/bentham/obsidian_vault/ bentham your_password`
    - 自動マウントのテスト
      - `sudo mount -a`: 成功
      - これで、Archのラップトップの`/home/bentham/Nextcloud`は常にNextcloudと繋がっている状態になった。
      - エラー：起動時に接続待ちがかなり長くなり、使い勝手が悪いのでこの方法での自動マウントをやめる
  - `systemd`タイマーを使って、立ち上げ後に自動的にマウントするように設定する
    - systemd サービスの作成: `sudo nano /etc/systemd/system/mount-nextcloud.service`
    - systemd タイマーを作成: `sudo nano /etc/systemd/system/mount-nextcloud.timer`
    - サービスとタイマーを有効化：`sudo systemctl start mount-nextcloud.service`と`sudo systemctl start mount-nextcloud.timer`
    - 再起動したが、自動マウントされず。
      - ログを確認：`journalctl -u mount-nextcloud.service`
      - 自己署名証明書に対して、承認エラーが発生していると発覚
      - 自己署名証明書を信頼させるには、サーバー証明書をシステム側で「信頼済み」に登録する必要がある
        - Nextcloudサーバーの証明書を取得: `echo -n | openssl s_client -connect {tailscale_IP}:443 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ~/nextcloud-selfsigned.crt`
          - 自己署名証明書が ~/nextcloud-selfsigned.crt として保存される
        - 証明書を davfs2 に認識させる
          - `sudo mkdir -p /etc/davfs2/certs`
          - `sudo cp ~/nextcloud-selfsigned.crt /etc/davfs2/certs/`
        - 一応システム全体で信頼させておく（curlやwebブラウザでアクセスしたときの面倒を排除するため）
          - `sudo cp ~/nextcloud-selfsigned.crt /etc/ca-certificates/trust-source/anchors/`
          - `sudo trust extract-compat`
      - まだエラーが出るので、davfs2.conf で証明書チェックを完全にスキップする
        - `sudo nano /etc/davfs2/davfs2.conf`
        - `trust_server_cert {tailscale_IP}`
        - 証明書を davfs2 に認識させる
          - `sudo cp /etc/davfs2/certs/nextcloud-selfsigned.crt /etc/davfs2/certs/{tailscale_IP}`
        - 権限を変更：
          - `sudo chmod 644 /etc/davfs2/certs/{tailscale_IP}`
          - `sudo chown root:root /etc/davfs2/certs/{tailscale_IP}`

### Nextcloud連携・全体構成図（外部からファイル同期まで）
- ここまでの構成を確認

┌────────────────────────────┐
│       Arch Linux（外部PC）            │
│ ─ Obsidian                          │
│ ─ systemd mount + davfs2           │
└────────────────────────────┘
         │
         │(1) 起動時に Tailscale で VPN接続
         ▼
┌────────────────────────────┐
│     Tailscale Virtual Network         │
│ ─ Tailscale IP: 100.xxx.xxx.xxx       │
└────────────────────────────┘
         │
         │(2) WebDAV 接続 (HTTPS)
         ▼
┌──────────────────────────────────────┐
│ Nextcloud サーバー（Ubuntu）                  │
│ ─ Local IP: 192.168.1.110                       │
│ ─ Hostname: nextcloud.local                    │
│ ─ Tailscale IP: 100.xxx.xxx.xxx                │
│                                                │
│ ┌────────────────────────────────────────┐     │
│ │ Webサーバ（Apache2） + PHP              │     │
│ │  └─ サービス: https://nextcloud.local   │     │
│ │                     or https://192.168.1.110 │
│ │  └─ WebDAVエンドポイント：             │     │
│ │      /remote.php/dav/files/bentham/     │     │
│ └────────────────────────────────────────┘     │
│                                                │
│ ┌────────────────────────────────────────┐     │
│ │ ストレージ構成                              │
│ │ ─ /dev/sdb1（HDD） → /mnt/nextcloud にマウント│
│ │ ─ Nextcloud のデータディレクトリがここにある │
│ └────────────────────────────────────────┘     │
└──────────────────────────────────────┘
         ▲
         │
         │(3) Arch 側に `/home/bentham/Nextcloud` としてマウント
         ▼
┌────────────────────────────┐
│   ローカル編集＋即反映（双方向同期）     │
│ → Obsidianでノート編集など              │
└────────────────────────────┘


- Nextcloudアクセス構成:  

| **構成要素**                 | **内容**                                                                 |
|-----------------------------|--------------------------------------------------------------------------|
| **VPN接続**                 | 外出先から Tailscale を使って Nextcloud サーバに安全にアクセス        |
| **Nextcloudサーバ**         | Ubuntu + Apache2 + PHP。Nextcloud の実データは `/mnt/nextcloud` に配置 |
| **ローカル接続名**           | `nextcloud.local`（LAN内向け）、`192.168.1.110`                          |
| **WebDAVアクセス（外部）** | `https://{Tailscale_IP}/remote.php/dav/files/bentham/` 経由で接続       |
| **証明書**                  | Self-signed 証明書を使用、`trust_server_cert` によりバイパス設定済み    |
| **自動マウント**            | `systemd` によりログイン後にマウント実行、認証情報は `/etc/davfs2/secrets` に保存 |
| **同期ディレクトリ（Arch）**| Arch 上の `/home/bentham/Nextcloud` に WebDAV をマウントし、Obsidian などで編集可能 |

## WindowsのメインPCにも設定

- windows11のノートPCもArchと同じように設定する
  - Tailscale を Windows にインストールし接続
    - 証明書を信頼させる
      - 証明書をWindowsPCに移す
        - `scp bentham@192.168.1.110:/etc/ssl/certs/nextcloud-selfsigned.crt .`
    - Windowsに証明書を信頼させる
      - Win + R キーを押して、「certmgr.msc」と入力 → Enter
      - 左側メニューから: 「信頼されたルート証明機関」 > 「証明書」 を選択
      - 右クリック → 「すべてのタスク」 → 「インポート」
      - ウィザードに従って、nextcloud-selfsigned.cer を選択
      - 「証明書をすべて次のストアに配置する」を選び、「信頼されたルート証明機関」 を選択
      - インポート完了後、**一度再起動 or ネットワーク関係の再接続（ObsidianやWebDAVなど）**を実行
  - WebDAV を Windows にマウント
    - 「エクスプローラー」 → 「PC」→「ネットワーク ドライブの割り当て」
      - `https://<TailscaleのNextcloudサーバーIP>/remote.php/dav/files/bentham/`：フォルダに入力
      - 「別の資格情報を使用して接続する」にチェック
      - Nextcloudのユーザー名とアプリパスワードを入力
        - アプリパスワードは、Nextcloud Web UIの「セキュリティ」から生成

### 設定継続

- メイン(Windows11)からサーバーへ接続
  - `ssh bentham@192.168.1.110`
  - 公開鍵認証を設定する（パスワード不要に）
    - `ssh-keygen -t ed25519 -C "Windows11 main key"`
    - 公開鍵をサーバへコピー
      - コピーして貼り付け(`ssh-ed25519 {KEY} ebisu@LAPTOP-NMSNU7B7`の形式)
      - `nano ~/.ssh/authorized_keys`
    - サーバー側のSSH設定が公開鍵認証を許可するように設定
      - `sudo nano /etc/ssh/sshd_config`
      - コメントアウトされていたら修正：`PubkeyAuthentication yes`, `AuthorizedKeysFile     .ssh/authorized_keys`, `PasswordAuthentication no`
    - なぜかうまくいかないので、PW毎回入力する
  - 
  - 




## 参考

- 理解：
- [マザーボードの選び方＆基礎知識をわかりやすく解説](https://www.youtube.com/watch?v=2Q6KD1FEJDg)
- [How do computers work?](https://www.youtube.com/watch?v=4knBXkN1GEU)
- [Install Ubuntu Server 22.04 LTS](https://www.youtube.com/watch?v=zs2zdVPwZ7E)




