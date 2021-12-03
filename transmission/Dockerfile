FROM ubuntu:focal

RUN apt update && apt -y upgrade && apt clean


ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Berlin
RUN apt -y install ca-certificates wget net-tools gnupg iputils-ping jq curl transmission-daemon supervisor


# install openvpn
ADD https://swupdate.openvpn.net/repos/repo-public.gpg repo-public.gpg
RUN apt-key add repo-public.gpg
RUN echo "deb http://build.openvpn.net/debian/openvpn/release/2.5 focal main" > /etc/apt/sources.list.d/openvpn-aptrepo.list
RUN apt-get update && apt-get -y install openvpn

# config
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf


ENTRYPOINT [ "/usr/bin/supervisord" ]


# healthcheck
HEALTHCHECK --interval=120s --start-period=120s CMD curl -s ipinfo.io/ | jq ".country" |grep \"HU\" || pkill --signal SIGHUP -F /var/run/openvpn.pid
#HEALTHCHECK --interval=120s --start-period=120s CMD curl -s ipinfo.io/ | jq ".country" |grep \"HU\" || kill 1

