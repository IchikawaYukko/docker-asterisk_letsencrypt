# docker-asterisk_letsencrypt
Asterisk PBX with certbot client for serving SIP/TLS connection &amp; auto renew feature.

# 使い方
## 証明書の取得
`docker-compose up` すると証明書がないというメッセージが出るので
```
[+] Running 1/0
 ✔ Container asterisk-pbx-1  Created                                                                                                                                                 0.0s
Attaching to pbx-1
pbx-1  | ls: /etc/letsencrypt: No such file or directory
pbx-1  | No certificates exist!
pbx-1  | Run "certbot certonly --standalone --agree-tos -m YOUR_EMAIL_ADDRESS -d YOUR_PBX_DOMAIN --elliptic-curve secp384r1" to get certificates
```
別ターミナルから`docker compose exec pbx sh`でコンテナ内に入り `certbot certonly --standalone --agree-tos -m YOUR_EMAIL_ADDRESS -d YOUR_PBX_DOMAIN --elliptic-curve secp384r1` で証明書を取得します。
```
docker compose exec pbx sh
/ # certbot certonly --standalone --agree-tos -m YOUR_EMAIL_ADDRESS -d YOUR_PBX_DOMAIN --elliptic-curve secp384r1
```
取得したら一度`docker compose down`しておきます。
## 証明書を取得せず使う場合(TLS機能を使わない)
`docker-compose up -d` したあと`docker compose exec pbx sh`でコンテナ内に入り`/etc/letsencrypt/empty`でダミーファイルを作成しておきます。

```
# docker compose exec pbx sh
/ # touch /etc/letsencrypt/empty
/ # exit
# docker compose down
```
`exit`して`docker compose down`しておきます。
## 起動
`docker compose up -d`で起動します。必要な .conf ファイルはcompose.ymlの`volume:`項に書いておきます  
取得した証明書はコンテナ起動時に`/var/lib/asterisk/keys/`へコピーされるので、 pjsip.conf などから参照して使用します。（sample-config/ 参照）
