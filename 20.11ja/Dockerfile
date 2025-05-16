FROM	ichikawayukko/asterisk:20.11

RUN	wget http://downloads.asterisk.org/pub/telephony/sounds/releases/asterisk-core-sounds-ja-wav-1.6.1.tar.gz && \
	mkdir -p /var/lib/asterisk/sounds/ja && \
	tar zxfv asterisk-core-sounds-ja-wav-1.6.1.tar.gz -C /var/lib/asterisk/sounds/ja && \
	rm -f asterisk-core-sounds-ja-wav-1.6.1.tar.gz && \
	echo 'Copy moh sound for asterisk Playback()' && \
	cp -a /var/lib/asterisk/moh/*.wav /var/lib/asterisk/sounds/en/

