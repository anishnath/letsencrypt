Video POC : https://youtu.be/Ui_ZsOV2ioA

1. Install certbot and add letsencrypt folder
	wget https://dl.eff.org/certbot-auto
	chmod a+x certbot-auto

2. Create the file for letssncrypt  
	mkdir -p /var/www/letsencrypt/

2. Create the file /tmp/my-domain.conf, where my‑domain is your fully qualified domain name (for example, 7gwifi.org).
	vim /tmp/my-domain.conf
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

3. Allowing Let’s Encrypt to Access the Temporary File

	location /.well-known/acme-challenge {
		root /var/www/letsencrypt;
	    }
		
4. Validate the nginx configuration and restart it 
	sudo nginx -t && sudo nginx -s reload

5. Run the certbot-auto command 
	./certbot-auto --config /tmp/my-domain.conf certonly
		
		