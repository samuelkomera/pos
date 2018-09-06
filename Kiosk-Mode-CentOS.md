# First Plum Device (FPD)

1. sudo yum update && sudo yum upgrade
2. Create User Accounts
   * Create 2 users. One user is already created -- the account you’re logged in as. For this we’ll assume the account name        is pos. Create a second user named kiosk. This will become the auto-logged in user.
     As a __root user__ Run the following commands.
     ~~~
     adduser kiosk
     passwd kiosk
     ~~~  
   * Create a group named __netpos__,and the 2 users i.e,pos and kiosk to the netpos group.
     As a __root user__ Run the              following commands.
     ~~~
     sudo groupadd netpos
     usermod -aG netpos pos
     usermod -aG netpos pos
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
5. Download and copy  __pa__ to __/opt/np/__
6. Download and copy  __rf14-0-1__ to __/opt/netpos/icr3rd/__
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
   * The following steps are made by referring to pa/install/README.md
    __Create a static configuration.__
    bin/paSetupConf.sh 100 PTqCPtHEt8Y388aFxS3eiNpCZSIeLu9YDsCOVku5gOM 9101
    bin/paCreateDb.sh 9101
    __Create a software instance__
    cp install/pa.ini
    install/mkpa.php
    __Setup txserver access__
    cd releases/pa1-0-0
    cp ../../install/conf/paCentral.json .
    vi paCentral.json                (set password)
    bin/paCentralSetup.php -j paCentral.json
    __run instance__
    bin/pa.sh start
    
5. * Chrome asks for password to unlock keyring on startup
   * google-chrome --password-store=basic
6. Disable the userlist on lock screen : [UserList Disabling](https://help.gnome.org/admin/system-admin-guide/stable/login-userlist-disable.html.en)

8. Automatic Network On 
        * cd /etc/sysconfig/network-scripts/ [Reference](https://wiki.centos.org/FAQ/CentOS7) 

