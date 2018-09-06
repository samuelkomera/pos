# First Plum Device (FPD)

1. sudo yum update && sudo yum upgrade
2. Create User Accounts
   * Create 2 users. One user is already created -- the account you’re logged in as. For this we’ll assume the account name        is __pos__. Create a second user named __kiosk__. This will become the auto-logged in user.<br>
   
     As a __root user__ run the following commands:
     ~~~
     adduser kiosk
     passwd kiosk
     ~~~  
   * Create a group named __netpos__,and the 2 users i.e,pos and kiosk to the netpos group.<br>
   
     As a __root user__ run the following commands:
     ~~~
     sudo groupadd netpos
     usermod -aG netpos pos
     usermod -aG netpos kiosk
3. Add Users To sudoers file so it wouldn’t get prompted for passwords all the time.Run __visudo__ and at the bottom add
    ~~~
      pos ALL=(ALL) NOPASSWD: ALL
      kiosk ALL=(ALL) NOPASSWD: ALL
4. Setup Auto Login. Now we can setup the auto login process. We need the user Kiosk to auto login on every reboot. 
   * edit the following lines to the file __/etc/gdm/custom.conf__
    ~~~~~ 
      # Enable automatic login for user
      [daemon]
      AutomaticLogin=kiosk
      AutomaticLoginEnable=True
    ~~~~~ 
5. Create folders and copy pa project
   * Create a folder __np__ in __opt__ directory.
   * Create a folder __/netpos/icr3rd__ in __opt__ directory.
   * Change the owner and group of __np__ and __icr3rd__ folder. Run following commands. 
     ~~~
     cd /opt/
     sudo chown pos:netpos np
     cd /netpos/
     sudo chown pos:netpos icr3rd
   * Download and untar  __pa__ to __/opt/np/__
   * Download and untar  __rf14-0-1__ to __/opt/netpos/icr3rd/__
   
7. Install Postgres as root user
   * yum install postgresql.x86_64  postgresql-server postgresql-contrib  libnsl.x86_64
   * systemctl enable postgresql
   * sudo postgresql-setup initdb 
   * yum install epel-release
   * yum install php-mcrypt
   * yum install libnghttp2
   * yum install libpsl
   * systemctl start postgresql
8. Setup PA instance as __pos user__
   * cd /opt/np/pa/
   * install/setup.sh /opt/np/tools
   * The following steps are made by referring to pa/install/README.md.<br>
 __Create a static configuration.__
   * bin/paSetupConf.sh 100 PTqCPtHEt8Y388aFxS3eiNpCZSIeLu9YDsCOVku5gOM 9101
   * bin/paCreateDb.sh 9101 <br>
 __Create a software instance__
   * cp install/pa.ini
   * install/mkpa.php <br>
 __Setup txserver access__
   * cd releases/pa1-0-0
   * cp ../../install/conf/paCentral.json .
   * vi paCentral.json                (set password)
   * bin/paCentralSetup.php -j paCentral.json <br>
 __run pa instance that automatically runs with a script using systemd__
   * As a __root user__ create __paservice.sh__ script in __/usr/local/sbin/__ with following lines
       ~~~~
       #!/bin/sh
       sudo su - pos -c "cd /opt/np/pa/releases/pa1-0-0/;bin/pa.sh start > /dev/null 2>&1"
   * Make the __paservice.sh__ script executable
       ~~~~
       sudo chmod +x paservice.sh
   * As a __root user__ create a service named __pa.service__ in __/etc/systemd/system/__ with the following lines
      ~~~~
      [Unit]
      Description=pa service

      [Service]
      ExecStart=/usr/local/sbin/paservice.sh

      [Install]
      WantedBy=multi-user.target

      
9. 

