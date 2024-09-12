#### Reputation:
- [DNSWL.org](https://www.dnswl.org/) - Mail Whitelist
- [Spamhaus](https://www.spamhaus.org/) - Mail Blocklist monitor

#### System Accounts:
- siteadmin@the-bwc.com - Main site admin. All email sent to any other system account is automatically redirected to this one.
- status@the-bwc.com - Status Alerts
- passbolt@the-bwc.com - Passbolt Transactional Email
- grafana@the-bwc.com - Grafana Alerts

#### Shop Accounts:
- S-1@the-bwc.com - IT services owner
- S-9@the-bwc.com - Social Media owner

#### Mail Server:
BWC currently runs our own mailserver [Mailcow](https://mailcow.email/) located on our Main [[Server]] in our [[Docker]] environment.
Mailcow manages the entire mail server and automates a bunch of the processes. The instance can be reached from: https://mail.the-bwc.com

#### DNS Records:
All DNS records are located in our [[Cloudflare]] account.
*Note: IP Addresses have been redacted*
```
;; A Records
mail.the-bwc.com.	1	IN	A	<<REDACTED>>

;; AAAA Records
mail.the-bwc.com.	1	IN	AAAA	<<REDACTED>>

;; CNAME Records
autoconfig.the-bwc.com.	1	IN	CNAME	mail.the-bwc.com.
autodiscover.the-bwc.com.	1	IN	CNAME	mail.the-bwc.com.

;; MX Records
the-bwc.com.	1	IN	MX	10 mail.the-bwc.com.

;; SRV Records
_autodiscover._tcp.the-bwc.com.	1	IN	SRV	0 1 443 mail.the-bwc.com.
_caldavs._tcp.the-bwc.com.	1	IN	SRV	0 1 443 mail.the-bwc.com.
_carddavs._tcp.the-bwc.com.	1	IN	SRV	0 1 443 mail.the-bwc.com.
_imaps._tcp.the-bwc.com.	1	IN	SRV	0 1 993 mail.the-bwc.com.
_imap._tcp.the-bwc.com.	1	IN	SRV	0 1 143 mail.the-bwc.com.
_pop3s._tcp.the-bwc.com.	1	IN	SRV	0 1 995 mail.the-bwc.com.
_pop3._tcp.the-bwc.com.	1	IN	SRV	0 1 110 mail.the-bwc.com.
_sieve._tcp.the-bwc.com.	1	IN	SRV	0 1 4190 mail.the-bwc.com.
_smtps._tcp.the-bwc.com.	1	IN	SRV	0 1 465 mail.the-bwc.com.
_submission._tcp.the-bwc.com.	1	IN	SRV	0 1 587 mail.the-bwc.com.

;; TLSA Records
_25._tcp.mail.the-bwc.com.	1	IN	TLSA	3 1 1 3870a1117eab1b7d7dff475371c501ae3fe1ce3fb4e7c8c26a119bafead9495e

;; TXT Records
_adsp._domainkey.the-bwc.com.	1	IN	TXT	"dkim=all"
_caldavs._tcp.the-bwc.com.	1	IN	TXT	"path=/SOGo/dav/"
_carddavs._tcp.the-bwc.com.	1	IN	TXT	"path=/SOGo/dav/"
dkim._domainkey.the-bwc.com.	1	IN	TXT	"v=DKIM1;k=rsa;t=s;s=email;p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArRSE7iJZbmkYAlPLar+/5f7YCsK4fG8avYa1grBM5LtytdYIYWvhiXo63saLuAMoTQk8O8yif+PaFVxyFKKAkaAL3jJyjlb1ue6fjybjf9YH3yKa+RtcI3js31OUEHynMIBBWTxoSlkKmpwRCUD5Mkj96Y+9m6rI0Hk9sorJ8xsPwrXdjwiTEIF" "3npnZVnL6vx1s0QQLYMMGceJgR9vrm3iJCj41wGx7CoFrDgEIu37SquOkyEvzfIrCoBEMcUWCAIiDue13jwQYs4dIndG7N0XdvkJCnqFehbGO1UcE/jBcllPtE10p5iF16IQ34Hs51AvAuVCX9pKWBVQ//x/kuwIDAQAB"
_dmarc.the-bwc.com.	1	IN	TXT	"v=DMARC1; p=quarantine; pct=100; adkim=r; aspf=r; rf=afrf; ri=86400; rua=mailto:noreply-dmarc@the-bwc.com; ruf=mailto:noreply-dmarc@the-bwc.com;"
the-bwc.com.	1	IN	TXT	"v=spf1 mx a -all"
_token._dnswl.the-bwc.com.	1	IN	TXT	"efasna5c6wzcqfnbnnajkewnswcozacn"
```