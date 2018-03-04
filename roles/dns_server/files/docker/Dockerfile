FROM alpine:latest
EXPOSE 53 53/udp

RUN apk --update upgrade && apk add bind

RUN mkdir -m 0750 -p /etc/bind ; \
    mkdir -m 0770 -p /var/bind && chown -R root:named /var/bind ; \
    mkdir -m 0770 -p /var/run/named && chown -R root:named /var/run/named ; \
    wget -q -O /etc/bind/bind.keys https://ftp.isc.org/isc/bind9/keys/9.11/bind.keys.v9_11 ; \
    rndc-confgen -a -r /dev/urandom && chmod 0640 /etc/bind/rndc.key; \
    chown -R root:named /etc/bind

COPY configs/. /etc/bind/

CMD ["named", "-c", "/etc/bind/named.conf", "-g", "-u", "named"]
