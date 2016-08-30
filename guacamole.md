# Getting Started - AWS
Connect to your AWS account
ssh -i "~./Desktop/guacamoleDev.pem" ubuntu@ec2-54-196-16-98.compute-1.amazonaws.com
# Get set

* [Guacamole 0.9.9 documentation](https://guacamole.incubator.apache.org/doc/gug/index.html)
* [Guacamole files](https://guacamole.incubator.apache.org/releases/)
**Get dependencies**
* sudo apt-get update
* sudo apt-get install libcairo2-dev libpng12-dev libossp-uuid-dev libfreerdp-dev libpango1.0-dev libssh2-1-dev libtelnet-dev libvncserver-dev libpulse-dev libssl-dev libvorbis-dev libwebp-dev
sudo apt-get install build-essential
**Get jpegturbo**
wget -O libjpeg-turbo-official_1.5.0_amd64.deb http://downloads.sourceforge.net/project/libjpeg-turbo/1.5.0/libjpeg-turbo-official_1.5.0_amd64.deb
dpkg -i libjpeg-turbo-official_1.5.0_amd64.deb

## Install tomcat
sudo apt-get install tomcat7

**Get the server and .war files**
wget -O guacserver-0.9.9.tar.gz http://sourceforge.net/projects/guacamole/files/current/source/guacamole-server-0.9.9.tar.gz

wget -O guacclient-0.9.9.tar.gz https://sourceforge.net/projects/guacamole/files/current/source/guacamole-client-0.9.9.tar.gz/download

wget -O guac-0.9.9.war http://downloads.sourceforge.net/project/guacamole/current/binary/guacamole-0.9.9.war

**Prep the files**
tar -xzf guacserver-0.9.9.tar.gz

cd guacamole-server-0.9.9
./configure --with-init-dir=/etc/init.d

make
sudo make install
sudo ldconfig

##Deploy
At this we've downloaded the files for guacserver.  We then installed these files by making the required files and linking them to your system.  Now we deploy those files by placing them in the correct directory.

sudo cp ~/guac-0.9.9.war /var/lib/tomcat7/webapps/guacamole.war

sudo /etc/init.d/tomcat7 restart
sudo /etc/init.d/guacd start

sudo mkdir /etc/guacamole
sudo mkdir /etc/guacamole/lib
sudo mkdir /etc/guacamole/extensions

sudo cp ~/guac-0.9.9.war /etc/guacamole/guacamole.war
sudo ln -s /etc/guacamole/guacamole.war /var/lib/tomcat7/webapps/

sudo touch /etc/guacamole/guacamole.properties
sudo touch /etc/guacamole/user-mapping.xml
