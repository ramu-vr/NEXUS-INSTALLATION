# NEXUS-INSTALLATION
sudo apt-get update
sudo apt install openjdk-8-jre-headless
cd /opt
sudo wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
tar -zxvf latest-unix.tarsudo visudo.gz
sudo mv /opt/nexus-3.30.1-01 /opt/nexus

sudo adduser nexus
sudo visudo
nexus ALL=(ALL) NOPASSWD: ALL
sudo chown -R nexus:nexus /opt/nexus
sudo chown -R nexus:nexus /opt/sonatype-work
sudo vi /opt/nexus/bin/nexus.rc
run_as_user="nexus"
vi /opt/nexus/bin/nexus.vmoptions 
-Xms1024m
-Xmx1024m
-XX:MaxDirectMemorySize=1024m

-XX:LogFile=./sonatype-work/nexus3/log/jvm.log
-XX:-OmitStackTraceInFastThrow
-Djava.net.preferIPv4Stack=true
-Dkaraf.home=.
-Dkaraf.base=.
-Dkaraf.etc=etc/karaf
-Djava.util.logging.config.file=/etc/karaf/java.util.logging.properties
-Dkaraf.data=./sonatype-work/nexus3
-Dkaraf.log=./sonatype-work/nexus3/log
-Djava.io.tmpdir=./sonatype-work/nexus3/tmp
sudo vi /etc/systemd/system/nexus.service
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Restart=on-abort

[Install]
WantedBy=multi-user.target
sudo systemctl start nexus
sudo systemctl enable nexus
sudo systemctl status nexus
sudo systemctl stop nexus
tail -f /opt/sonatype-work/nexus3/log/nexus.log

cat /opt/nexus/sonatype-work/nexus3/admin.password

