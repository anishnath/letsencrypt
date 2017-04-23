##LetsEncrypt Install and Configure on Nginx 

Video POC : https://youtu.be/Ui_ZsOV2ioA

1. Install certbot and add letsencrypt folder

```
bash ~ wget https://dl.eff.org/certbot-auto
bash ~chmod a+x certbot-auto
```

2. Create the file for letssncrypt  
```
bash ~ mkdir -p /var/www/letsencrypt/
```
3. Create the file /tmp/my-domain.conf, where my‑domain is your fully qualified domain name (for example, 7gwifi.org).
```
bash ~ vim /tmp/my-domain.conf
```
the Content of the file working with example domain __7gwifi.org__
```
		# the domain we want to get the cert for;
		# technically it's possible to have multiple of this lines, but it only worked
		# with one domain for me, another one only got one cert, so I would recommend
		# separate config files per domain.
		domains = 7gwifi.org

		# increase key size
		rsa-key-size = 2048 # Or 4096

		# the current closed beta (as of 2015-Nov-07) is using this server
		server = https://acme-v01.api.letsencrypt.org/directory

		# this address will receive renewal reminders
		email = my-email

		# turn off the ncurses UI, we want this to be run as a cronjob
		text = True

		# authenticate by placing a file in the webroot (under .well-known/acme-challenge/)
		# and then letting LE fetch it
		authenticator = webroot
		webroot-path = /var/www/letsencrypt/
```

4. Allowing Let’s Encrypt to Access the Temporary File

```
	location /.well-known/acme-challenge {
		root /var/www/letsencrypt;
	    }
```	    
		
5. Validate the nginx configuration and restart it 
```
	sudo nginx -t && sudo nginx -s reload
```
6. Run the certbot-auto command 
```
	./certbot-auto --config /tmp/my-domain.conf certonly
```
7. Your Certificate will be provided by the Letsencrypt 
		
