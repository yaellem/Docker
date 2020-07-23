FROM debian
EXPOSE  9987/udp 10011 30033
ENV TS3SERVER_LICENSE=accept
WORKDIR /home/
RUN apt-get update -yq && apt-get upgrade -yq && apt-get install -yq wget bzip2  && wget http://dl.4players.de/ts/releases/3.0.13.8/teamspeak3-server_linux_amd64-3.0.13.8.tar.bz2 && tar xvf *.tar.bz2 && cd teamspeak3-server_linux_amd64 && touch .ts3server_license_accepted
WORKDIR teamspeak3-server_linux_amd64
CMD sh ts3server_minimal_runscript.sh
#docker built -t <image name>
# according to hub.docker doc : docker run --rm -p 9987:9987/udp -p 10011:10011 -p 30033:30033 my    -image2
