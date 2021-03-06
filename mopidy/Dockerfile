FROM debian:buster

LABEL Author Kassim Benhaddad <kassim@kass.im>

RUN apt-get update \
       && DEBIAN_FRONTEND=noninteractive apt-get install -y --fix-missing \
       gcc \
       curl \
       python-crypto \
       python3-setuptools \
       python-pip \
       wget \
       git

RUN curl -L https://apt.mopidy.com/mopidy.gpg -o /tmp/mopidy.gpg \
       && wget -q -O /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/buster.list \
       && apt-key add /tmp/mopidy.gpg \
       && apt-get update \
       && apt-get install -y mopidy \
       && apt-get install -y mopidy-spotify \
       && apt-get install -y mopidy-mpd \
       && apt-get install -y mpc \
       && apt-get install -y python3-venv \
       && apt-get install -y python3-pip \
       && apt-get install -y gstreamer1.0-plugins-bad \
       && python3 -m pip install Mopidy-Mixcloud \
       && python3 -m pip install Mopidy-RadioNet \
       && python3 -m pip install Mopidy-Mobile \
       && python3 -m pip install Mopidy-YouTube \
       && python3 -m pip install Mopidy-SomaFM \
       && python3 -m pip install Mopidy-SoundCloud \
       && python3 -m pip install Mopidy-RadioNet \
       && python3 -m pip install Mopidy-Subidy \
       && python3 -m pip install Mopidy-YouTube \
       && python3 -m pip install Mopidy-MusicBox-Webclient \
       && python3 -m pip install Mopidy-Party
  
RUN cd /tmp/ \
       && git clone --single-branch --branch develop https://github.com/jaedb/Iris.git \
       && cd Iris \
       && python3 setup.py install

RUN echo "mopidy ALL=NOPASSWD: /usr/local/lib/python3.7/dist-packages/Mopidy_Iris-3.44.0-py3.7.egg/mopidy_iris/system.sh" >> /etc/sudoers

RUN apt-get purge --auto-remove -y \
       curl \
       gcc \
       && apt-get clean \
       && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* ~/.cache \
       && mkdir -p /var/lib/mopidy/.config 

COPY mopidy.conf /var/lib/mopidy/.config/mopidy/mopidy.conf

RUN chown mopidy:audio -R /var/lib/mopidy/.config  
  
USER mopidy

VOLUME /var/lib/mopidy

EXPOSE 6600
EXPOSE 6680

CMD ["/usr/bin/mopidy"]
