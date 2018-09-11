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
     ~~~
5. DISABLING COMMAND-LINE ACCESS : as a __root__ user
   * Create the directory __/etc/dconf/db/local.d/__ if it does not already exist.
   * Create a local database for machine-wide settings in /etc/dconf/db/local.d/00-lockdown:
     ~~~
     [org/gnome/desktop/lockdown]
     # Disable command-line access
     disable-command-line=true
   * Override the user's setting and prevent the user from changing it in __/etc/dconf/db/local.d/locks/lockdown__:
     ~~~
     # Lock the disabled command-line access
     /org/gnome/desktop/lockdown  
   * Update the system databases:
     ~~~
     # dconf update
6. LOCKING DOWN USER LOGOUT : as a __root__ user
   * Create the key file __/etc/dconf/db/local.d/00-logout__ to provide information for the local database:
     ~~~
      [org/gnome/desktop/lockdown]
      # Prevent the user from user switching
      disable-log-out=true
     ~~~ 
   * Override the user's setting and prevent the user from changing it in __/etc/dconf/db/local.d/locks/lockdown__:
     ~~~
      # Lock this key to disable user logout
      /org/gnome/desktop/lockdown/disable-log-out
     ~~~ 
   * Update the system databases:
     ~~~
      # dconf update
     ~~~  
7. LOCKING DOWN USER SWITCHING : As a __root__ user    .
   * Create the key file __/etc/dconf/db/local.d/00-user-switching__ to provide information for the local database:
     ~~~
     [org/gnome/desktop/lockdown]
     # Prevent the user from user switching
     disable-user-switching=true
     ~~~
   * Override the user's setting and prevent the user from changing it in __/etc/dconf/db/local.d/locks/lockdown__:
     ~~~
     # Lock this key to disable user switching
     /org/gnome/desktop/lockdown/disable-user-switching
     ~~~
   * Update the system databases:
     ~~~
     # dconf update 
     ~~~
8. Disable (and re-enable) notifications : 
     * As a __root__ user to __disable__ run the command
       ~~~
        gsettings set org.gnome.desktop.notifications show-banners false 
     * As a __root__ user to __enable__ run the command
       ~~~
        gsettings set org.gnome.desktop.notifications show-banners true
9. Disable auto lock screen : 
     * As a __root__ user to __disable__ run the command
       ~~~
        gsettings set org.gnome.desktop.lockdown disable-lock-screen true
10. Enabling Screensaver when the device is idle
     * Install xscreensaver - run the following command  
        ~~~
         sudo yum install xscreen-saver xscreensaver-data xscreensaver-data-extra
     * As a __kiosk user__ create a desktop file named __xscreensaver.desktop__ at __~/.config/autostart/__ 
        ~~~
        [Desktop Entry]
        Type=Application
        Exec=/usr/bin/xscreensaver -nosplash
        Name=Google-Chrome     
     * Launch Screensaver set Mode : Only one Screen Saver    
11. Disabling Hot Corners and Touch screen Guestures.
     * Install gnome-tweak-tools 
       ~~~
        sudo yum install gnome-tweak-tool
     * Launch and navigate to the 'Extensions' tab and download extensions __Disable Gestures__ and __Custom Hot Corners__.
     * Switch the downloaded extensions.  
12. Disable annoying automatic onscreen keyboard gnome
     * comment the executable line in __/usr/share/dbus-1/services/org.gnome.Caribou.Daemon.service__
       
12. Create folders and copy pa project
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
   
13. Install Postgres as root user (* Postgres version must be > 10.X )
    * yum install postgresql.x86_64  postgresql-server postgresql-contrib  libnsl.x86_64
    * systemctl enable postgresql 
    * sudo postgresql-setup initdb 
    * yum install epel-release
    * yum install php-mcrypt
    * yum install libnghttp2
    * yum install libpsl
    * systemctl start postgresql
14. Setup PA instance as __pos user__
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
15. Autostart Application for Kiosk user
      * As a __kiosk user__ create a desktop file named __google-autostart.desktop__ at __~/.config/autostart/__ 
        ~~~~~
        [Desktop Entry]
        Type=Application
        Exec=/usr/bin/google-chrome --password-store=basic --kiosk --incognito --disable-pinch --overscroll-history-                 navigation=0 https://localhost:9101
        Name=Google-Chrome   

16. Lauch Gnome Applications from ipa Application
      1. In order to launch the gnome application as __pos__ user in __kiosk__ session need to run the xhost +.
      2. As a __kiosk__ user write a script named __xhostScript.sh__ in __home/kiosk/__ 
          ~~~
          xhost + >> /dev/null 2>&1
          ~~~
          and run the script in  __.bashrc__
          ~~~
          /home/kiosk/xhostScript.sh
          ~~~
        
      3. As __pos__ user to launch Network Manager write script named network.sh in __/opt/np/pa/htdocs/__
          ~~~
          #!/bin/sh
          export DISPLAY=:0.0;
          /usr/bin/nm-connection-editor;
      4. To execute the script from php page on button click 
          ```
          <?php
               if(isset($_POST['network'])){
                    shell_exec("/opt/np/pa/htdocs/gnomeApps.sh print");
                }
                 if(isset($_POST['print'])){
                    shell_exec("/opt/np/pa/htdocs/gnomeApps.sh network");
                }

            ?>
          <form method="post" action="">
             <input type="submit" name="network" value="Network Manager">
             <input type="submit" name="print" value="Print Manager">
          </form>
  
       5. Follow steps 3 and 4 to run other gnome applications.            
