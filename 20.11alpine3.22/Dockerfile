FROM	golang:1.24.4-alpine3.22

WORKDIR	/opt
RUN	apk add asterisk asterisk-sample-config asterisk-sounds-en asterisk-sounds-moh asterisk-opus asterisk-fax asterisk-speex asterisk-srtp certbot git && \
	echo '0 0 * * Mon certbot renew; kill 1 # Auto renew & reload Asterisk' > /var/spool/cron/crontabs/root && \
	echo '#!/bin/sh' >> /usr/local/bin/entrypoint.sh && \
	echo 'crond -d 8 -L /var/log/crond.log' >> /usr/local/bin/entrypoint.sh && \
	echo 'if [ -z "`ls /etc/letsencrypt`" ]; then' >> /usr/local/bin/entrypoint.sh && \
        echo '  echo No certificates exist!' >> /usr/local/bin/entrypoint.sh && \
	echo '  echo Run \"certbot certonly --standalone --agree-tos -m YOUR_EMAIL_ADDRESS -d YOUR_PBX_DOMAIN --elliptic-curve secp384r1\" to get certificates' >> /usr/local/bin/entrypoint.sh && \
	echo '  echo "     or"'							>> /usr/local/bin/entrypoint.sh && \
	echo '  echo Run \"touch /etc/letsencrypt\" to supress this message.'	>> /usr/local/bin/entrypoint.sh && \
 	echo '  exec tail -f /dev/null' >> /usr/local/bin/entrypoint.sh && \
	echo 'else' >> /usr/local/bin/entrypoint.sh && \
        echo '  cp -uv /etc/letsencrypt/live/*/*.pem /var/lib/asterisk/keys/' >> /usr/local/bin/entrypoint.sh && \
	echo '  exec asterisk "$@"' >> /usr/local/bin/entrypoint.sh && \
        echo 'fi' >> /usr/local/bin/entrypoint.sh && \
	chmod 755 /usr/local/bin/entrypoint.sh && \
	git clone https://github.com/miconda/sipexer && \
	cd sipexer/ && \
	go get ./... && \
	go build -ldflags="-s -w" . && \
	mv sipexer /usr/local/bin

WORKDIR /var/lib/asterisk
VOLUME	["/etc/letsencrypt"]
HEALTHCHECK --interval=120s --timeout=10s --start-period=15s --retries=3 CMD ["sh", "-c", "sipexer -co -fu healthcheck -tu healthcheck -timeout 1000 \"${HEALTHCHECK_SIPURL}\"|grep -q 'Server: Asterisk PBX 20.11.1'"]
ENTRYPOINT	["entrypoint.sh"]
CMD	["-f"]
