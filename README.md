# syno-acme
通過 acme 協議更新群暉 HTTPS 泛域名憑證的自動腳本，支持配置多個憑證
使用方法參見：

1、[https://www.up4dev.com/2018/05/29/synology-ssl-wildcard-cert-update/](http://www.up4dev.com/2018/05/29/synology-ssl-wildcard-cert-update/)

2、[https://hadb.me/synology-letsencrypt-multiple-domain-cert-configuration/](https://hadb.me/synology-letsencrypt-multiple-domain-cert-configuration/)

兔費域名建議使用 ZeroSSL
---
2022/6/22 更新群暉7.0以上支持，同時更新acme版本採用自動更新，acme經常更新，以後會自動更新最新版 感謝@iihong 
---
通過設置 CERT_SERVER 為 zerossl 或 letsencrypt 來決定憑證服務商
設置為 zerossl 時：必須設置 ACCOUNT_EMAIL，並以 ZeroSSL 提供憑證服務更新
設置為 letsencrypt 時：以 Let's Encrypt 提供憑證服務更新，如果出現code:60錯誤，無法建立SSL連接，請升級群暉內建CA機構根憑證，下面有方法：

## 方法一：

直接一條SSH命令更新 CA 庫

```sh
sudo mv /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt.bak && sudo curl -Lko /etc/ssl/certs/ca-certificates.crt https://curl.se/ca/cacert.pem
```
如果無法連接 https://curl.se/ca/cacert.pem 時，請選用方法二手動翻牆下載並更新

方法二：

1、下載CA機構根憑證
下載地址 https://curl.se/ca/cacert.pem
如無法下載請翻牆

2、將 **cacert.pem** 文件上傳到群暉某個目錄

3、執行以下2條SSH命令更新 CA 庫
請替換以下 /volume1/nas/cacert.pem 為你的文件路徑地址

```sh
cp /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt.bak
cp /volume1/nas/cacert.pem /etc/ssl/certs/ca-certificates.crt
```

以上方法可以利用 Putty 或 “任務計劃-新增-觸發任務-使用者定義指令碼” 來執行SSH命令備份和更新根憑證
