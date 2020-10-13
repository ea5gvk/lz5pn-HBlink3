# HBlink



It allowed communication from DMR radio via Pi-Star via HBlink to another Pi-Star and DMR radio connected to it.
It allowed communication from DMR radio via Pi-Star via HBlink to BrandMeister server as peer and DMR radios connected to talk groups in BrandMeister server.
It allowed communication from DMR radio via Pi-Star via HBlink to IPSC2 server as peer and DMR radios connected to talk groups in IPSC2 server.
It allowed communication from DMR radio via Pi-Star via HBlink to XLX as peer and DMR or D-Star radios connected to XLX.
It allowed communication from DMR radio via Pi-Star via HBlink to HBlink as peer and DMR radios connected to talk groups in HBlink.

Originaly this software was created as software for connection between IPSC2 and BrandMeister

Follow next command in terminal to install HBlin and HBmonitor on raspbian buster or Linux Mint:

apt update

apt upgrade

apt dist-upgrade

apt autoremove

apt autoclean

#install hblink

apt install git

apt install python3-distutils

cd /opt/

wget https://bootstrap.pypa.io/get-pip.py

python3 get-pip.py

apt install python3-twisted

apt install python3-bitarray

apt install python3-dev

git clone https://github.com/lz5pn/HBlink

cd /opt/HBlink

mv dmr_utils3 /opt/

mv HBlink3 /opt/

mv HBmonitor /opt/

cd /opt/

rm -r HBlink

cd /opt/dmr_utils3

chmod +x install.sh

./install.sh

cd /opt/HBlink3

cp hblink-SAMPLE.cfg hblink.cfg

cp rules-SAMPLE.py rules.py

Autostart HBLink:

nano /lib/systemd/system/hblink.service

Copy and paste the next:

------------------------------------------------------------------------------------------------------------------------
[Unit]

Description=Start HBlink

After=multi-user.target

[Service]

ExecStart=/usr/bin/python3 /opt/HBlink3/bridge.py

[Install]

WantedBy=multi-user.target

------------------------------------------------------------------------------------------------------------------------

systemctl daemon-reload

systemctl enable hblink


Install Parrot for Echotest:

chmod +x playback.py


Create directory for registration files, if /var/log/hblink is not created.

mkdir /var/log/hblink

To start Parrot service must use file /lib/systemd/system/parrot.service 

nano /lib/systemd/system/parrot.service

Copy and paste the next:

------------------------------------------------------------------------------------------------------------------------
[Unit]

Description=HB bridge all Service

After=network-online.target syslog.target

Wants=network-online.target

[Service]

StandardOutput=null

WorkingDirectory=/opt/HBlink3

RestartSec=3

ExecStart=/usr/bin/python3 /opt/HBlink3/playback.py -c /opt/HBlink3/playback.cfg

Restart=on-abort

[Install]

WantedBy=multi-user.target

------------------------------------------------------------------------------------------------------------------------

Start Parrot service:

systemctl enable parrot.service

systemctl start parrot.service

systemctl status parrot.service

nano /opt/HBlink3/rules.py

Test configuration:

python3 /opt/HBlink3/bridge.py

systemctl start hblink

systemctl status hblink

Install web monitor for HBLink.

cd /opt/HBmonitor

chmod +x install.sh

./install.sh

cp config_SAMPLE.py config.py

nano /opt/HBmonitor/config.py

Start monitor as system service:

cp utils/hbmon.service /lib/systemd/system/

systemctl enable hbmon

systemctl start hbmon

systemctl status hbmon

forward TCP ports 8080 and 9000 in router firewall


My HBlink http://kario88.dynamic-dns.net:8184/ and http://lz5pn.freeddns.com:8184/


73 de LZ5PN
