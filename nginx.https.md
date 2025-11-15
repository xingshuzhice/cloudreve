Https证书

0. sudo apt install nginx 

1. 安装软件
	sudo apt install  snapd
	sudo snap install --classic certbot

2. 申请证书 按步骤执行
	sudo certbot --nginx


```log
 sudo certbot --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address or hit Enter to skip.
 (Enter 'c' to cancel): weizihuaing@gmail.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at:
https://letsencrypt.org/documents/LE-SA-v1.5-February-24-2025.pdf
You must agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y
Account registered.
Please enter the domain name(s) you would like on your certificate (comma and/or
space separated) (Enter 'c' to cancel): xingshuzhice.com
Requesting a certificate for xingshuzhice.com

Certbot failed to authenticate some domains (authenticator: nginx). The Certificate Authority reported these problems:
  Domain: xingshuzhice.com
  Type:   dns
  Detail: no valid A records found for xingshuzhice.com; no valid AAAA records found for xingshuzhice.com

Hint: The Certificate Authority failed to verify the temporary nginx configuration changes made by Certbot. Ensure the listed domains point to this nginx server and that it is accessible from the internet.

Some challenges have failed.
Ask for help or search for solutions at https://community.letsencrypt.org. See the logfile /var/log/letsencrypt/letsencrypt.log or re-run Certbot with -v for more details.
web@iZt4nfgpam8ylh60ybpfffZ:~$ sudo certbot --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Please enter the domain name(s) you would like on your certificate (comma and/or
space separated) (Enter 'c' to cancel): file.xingshuzhice.com
Requesting a certificate for file.xingshuzhice.com

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/file.xingshuzhice.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/file.xingshuzhice.com/privkey.pem
This certificate expires on 2026-01-20.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for file.xingshuzhice.com to /etc/nginx/sites-enabled/default
Congratulations! You have successfully enabled HTTPS on https://file.xingshuzhice.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

```

3.检查证书是否成功更新,列出所有证书，包括它们的日期
	sudo certbot certificates

4.手动添加定时任务
	sudo crontab -e
	0 3 * * * certbot renew --quiet && systemctl reload nginx		//这将是凌晨3点自动巡

5.检查是否已有定时任务（cron job 或 systemd）
	sudo systemctl list-timers | grep certbot


6.这个会检查所有已经安装的证书，并自动更新命令
	sudo certbot renew

7.强制更新单个证书
	sudo certbot certonly --force-renew -d file.xingshuzhice.com

8.停止定时任务
	sudo systemctl stop snap.certbot.renew.timer
	sudo systemctl disable snap.certbot.renew.timer