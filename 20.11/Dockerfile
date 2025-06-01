FROM	alpine:3.21

RUN	apk add asterisk asterisk-sample-config asterisk-sounds-en asterisk-sounds-moh asterisk-opus asterisk-fax certbot && \
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
	chmod 755 /usr/local/bin/entrypoint.sh

ENTRYPOINT	["entrypoint.sh"]
CMD	["-f"]

