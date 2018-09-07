# First Plum Device (FPD)

1. sudo yum update && sudo yum upgrade
2. Create User Accounts and Groups
   * Create 2 users. One user is already created -- the account you’re logged in as. For this we’ll assume the account name        is __pos__. Create a second user named __kiosk__. This will become the auto-logged in user.
      As a __root user__ run the following commands:
      ~~~
      adduser kiosk
      passwd kiosk
      ~~~  
   * Create a group named __netpos__,and the 2 users i.e,pos and kiosk to the netpos group.
      As a __root user__ run the following commands:
      ~~~
      sudo groupadd netpos
      usermod -aG netpos pos
      usermod -aG netpos kiosk
3. Add Users To sudoers file so it wouldn’t get prompted for passwords all the time.Run __visudo__ and at the bottom add.
     ~~~
       pos ALL=(ALL) NOPASSWD: ALL
       kiosk ALL=(ALL) NOPASSWD: ALL
4. Setup Auto Login. Now we can setup the auto login process. We need the user Kiosk to auto login on every reboot. 
   * Edit the following lines to the file __/etc/gdm/custom.conf__
       ~~~ 
       # Enable automatic login for user
       [daemon]
       AutomaticLogin=kiosk
       AutomaticLoginEnable=True
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
   
6. Install Postgres as root user
    * yum install postgresql.x86_64  postgresql-server postgresql-contrib  libnsl.x86_64
    * systemctl enable postgresql
    * sudo postgresql-setup initdb 
    * yum install epel-release
    * yum install php-mcrypt
    * yum install libnghttp2
    * yum install libpsl
    * systemctl start postgresql
7. Setup PA instance as __pos user__
    * cd /opt/np/pa/
    * install/setup.sh /opt/np/tools

    ** The following steps are made by referring to pa/install/README.md.
    1. __Create a static configuration.__
      * bin/paSetupConf.sh 100 PTqCPtHEt8Y388aFxS3eiNpCZSIeLu9YDsCOVku5gOM 9101
      * bin/paCreateDb.sh 9101 <br>
    2.  __Create a software instance__
      * cp install/pa.ini
      * install/mkpa.php  <br>

    3. __Setup txserver access__
      * cd releases/pa1-0-0
      * cp ../../install/conf/paCentral.json .
      * vi paCentral.json                (set password)
      * bin/paCentralSetup.php -j paCentral.json <br>

    4. __Run pa instance that automatically runs at boot with a script using systemd__
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
      * Start the service and  enable it to run at boot:
         ~~~~~
         sudo systemctl start myfirst
         sudo systemctl enable myfirst        
8. Autostart Application for Kiosk user
   * As a __kiosk user__ create a desktop file named __google-autostart.desktop__ at __~/.config/autostart/__ 
      ~~~~~
      [Desktop Entry]
      Type=Application
      Exec=/usr/bin/google-chrome --password-store=basic --kiosk https://localhost:9101
      Name=Google-Chrome   
9. DISABLING COMMAND-LINE ACCESS : as a __root__ user
   * Create the directory __/etc/dconf/db/local.d/__ if it does not already exist.
   * Create a local database for machine-wide settings in /etc/dconf/db/local.d/00-lockdown:
         ~~~~~~~~
         [org/gnome/desktop/lockdown]
         # Disable command-line access
         disable-command-line=true
   * Override the user's setting and prevent the user from changing it in __/etc/dconf/db/local.d/locks/lockdown__:
         ~~~~~~~
         # Lock the disabled command-line access
         /org/gnome/desktop/lockdown  
   * Update the system databases:
          ~~~
           # dconf update
10. LOCKING DOWN USER LOGOUT : as a __root__ user
    * Create the key file __/etc/dconf/db/local.d/00-logout__ to provide information for the local database:
       ~~~~
        [org/gnome/desktop/lockdown]
        # Prevent the user from user switching
        disable-log-out=true
    * Override the user's setting and prevent the user from changing it in __/etc/dconf/db/local.d/locks/lockdown__:
       ~~~~
        # Lock this key to disable user logout
        /org/gnome/desktop/lockdown/disable-log-out
    * Update the system databases:
       ~~~~
        # dconf update
11. LOCKING DOWN USER SWITCHING : As a __root__ user    .
    * Create the key file __/etc/dconf/db/local.d/00-user-switching__ to provide information for the local database:
        ~~~~
        [org/gnome/desktop/lockdown]
        # Prevent the user from user switching
        disable-user-switching=true
    * Override the user's setting and prevent the user from changing it in __/etc/dconf/db/local.d/locks/lockdown__:
        ~~~~
        # Lock this key to disable user switching
        /org/gnome/desktop/lockdown/disable-user-switching
    * Update the system databases:
        ~~~~
         # dconf update 
12. Disable (and re-enable) notifications : 
       * As a __root__ user to __disable__ run the command
         ~~~
          gsettings set org.gnome.desktop.notifications show-banners false 
       * As a __root__ user to __enable__ run the command
         ~~~
          gsettings set org.gnome.desktop.notifications show-banners true
13. Disable auto lock screen : 
       * As a __root__ user to __disable__ run the command
        ~~~
          gsettings set org.gnome.desktop.lockdown disable-lock-screen true
