# Getting Started - AWS
Connect to your AWS account

```shell
ssh -i ~/Desktop/guacamole.pem ubuntu@ec2-54-226-175-208.compute-1.amazonaws.com
```
# Get set

* [Guacamole 0.9.9 documentation](https://guacamole.incubator.apache.org/doc/gug/index.html)
* [Guacamole files](https://guacamole.incubator.apache.org/releases/)
**Get dependencies**
```shell
* sudo apt-get update
* sudo apt-get install libcairo2-dev libpng12-dev libossp-uuid-dev libfreerdp-dev libpango1.0-dev libssh2-1-dev libtelnet-dev libvncserver-dev libpulse-dev libssl-dev libvorbis-dev libwebp-dev
sudo apt-get install build-essential
```
**Get jpegturbo**
```shell
wget -O libjpeg-turbo-official_1.5.0_amd64.deb http://downloads.sourceforge.net/project/libjpeg-turbo/1.5.0/libjpeg-turbo-official_1.5.0_amd64.deb
sudo dpkg -i libjpeg-turbo-official_1.5.0_amd64.deb
```

## Install tomcat
```shell
sudo apt-get install tomcat7
```
## Install your Graphical Environment
There was a time when the standard way to interact with a computer was through a command line.  

Since most servers are administered by people familiar with the CLI they don't come with a GUI pre-installed.  We'll install one now.

Only one of the desktop environments below is required.
```shell
sudo apt-get install gnome-core
sudo apt-get install xfce4
```

##Install VNC
```shell
sudo apt-get install vnc4server
```
##Install RDP
RDP can make use of caching which is supposed to be improve perfomance.  
```shell
sudo apt-get install xrdp
```

**Get the server and .war files**
```shell
wget -O guacserver-0.9.9.tar.gz http://sourceforge.net/projects/guacamole/files/current/source/guacamole-server-0.9.9.tar.gz

wget -O guacclient-0.9.9.tar.gz https://sourceforge.net/projects/guacamole/files/current/source/guacamole-client-0.9.9.tar.gz/download

wget -O guac-0.9.9.war http://downloads.sourceforge.net/project/guacamole/current/binary/guacamole-0.9.9.war
```
**Prep the files**
```shell
tar -xzf guacserver-0.9.9.tar.gz

cd guacamole-server-0.9.9
./configure --with-init-dir=/etc/init.d

make
sudo make install
sudo ldconfig
```
##Deploy
At this we've downloaded the files for guacserver.  We then installed these files by making the required files and linking them to your system.  Now we deploy those files by placing them in the correct directory.

```shell
sudo cp ~/guac-0.9.9.war /var/lib/tomcat7/webapps/guacamole.war
sudo ln -s /etc/guacamole/ /usr/share/tomcat7/.guacamole

sudo /etc/init.d/tomcat7 restart
sudo /etc/init.d/guacd start

sudo mkdir /etc/guacamole
sudo mkdir /etc/guacamole/lib
sudo mkdir /etc/guacamole/extensions

sudo cp ~/guac-0.9.9.war /etc/guacamole/guacamole.war
sudo ln -s /etc/guacamole/guacamole.war /var/lib/tomcat7/webapps/

sudo touch /etc/guacamole/guacamole.properties
sudo touch /etc/guacamole/user-mapping.xml
```

#Example guacamole.properties file
```shell
# Hostname and port of guacamole proxy
guacd-hostname: localhost
guacd-port:     4822
user-mapping: /path/to/user-mapping.xml
```
#Example user-mapping.xml
```shell
<user-mapping>

    <!-- Per-user authentication and config information -->
    <authorize username="USERNAME" password="PASSWORD">
        <protocol>vnc</protocol>
        <param name="hostname">localhost</param>
        <param name="port">5900</param>
        <param name="password">VNCPASS</param>
    </authorize>

    <!-- Another user, but using md5 to hash the password
         (example below uses the md5 hash of "PASSWORD") -->
    <authorize
            username="USERNAME2"
            password="319f4d26e3c536b5dd871bb2c52e3178"
            encoding="md5">

        <!-- First authorized connection -->
        <connection name="localhost">
            <protocol>vnc</protocol>
            <param name="hostname">localhost</param>
            <param name="port">5901</param>
            <param name="password">VNCPASS</param>
        </connection>

        <!-- Second authorized connection -->
        <connection name="otherhost">
            <protocol>vnc</protocol>
            <param name="hostname">otherhost</param>
            <param name="port">5900</param>
            <param name="password">VNCPASS</param>
        </connection>

    </authorize>

</user-mapping>
```
