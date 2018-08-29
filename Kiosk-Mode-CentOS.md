# Single Application Mode(SAM) - CentOS

1. sudo yum update && sudo yum upgrade
2. Create User Accounts
   * Create 2 users. One user is already created -- the account you’re logged in as. For this we’ll assume the account name is pos. Create a second user named kiosk. This will become the auto-logged in user.
   ~~~
     sudo su -
     adduser kiosk
     passwd kiosk
3. Add Users To sudoers file so it wouldn’t get prompted for passwords all the time.Run visudo and at the bottom add
    ~~~
      kiosk ALL=(ALL) NOPASSWD: ALL
      pos ALL=(ALL) NOPASSWD: ALL
4. Setup Auto Login. Now we can setup the auto login process. We need the user Kiosk to auto login on every reboot. 
   * edit the following lines to the file __/etc/gdm/custom.conf__
    ~~~~~ 
      # Enable automatic login for user
      [daemon]
      AutomaticLogin=kiosk
      AutomaticLoginEnable=True
    ~~~~~ 
    Reference [AutoLogin](https://wiki.archlinux.org/index.php/GDM)
    
5. Create a session with a name matching your user name by writing a /usr/share/xsessions/kiosk.desktop file and setting the Exec line to be:
   ~~~~
     cd /usr/share/xsessions
     vi kiosk.desktop
  ~~~~~   
     Exec=gnome-session --session kiosk
    
5. * Chrome asks for password to unlock keyring on startup
   * google-chrome --password-store=basic
6. Disable the userlist on lock screen : [UserList Disabling](https://help.gnome.org/admin/system-admin-guide/stable/login-userlist-disable.html.en)
7. Disabling auto screen Locking
    ~~~~~~
   * su - gdm -s /bin/sh
   * export $(dbus-launch)
   * GSETTINGS_BACKEND=dconf gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-ac-type 'nothing'
   * Exit or Ctrl+D
   * systemctl restart gdm
   
8. Automatic Network On 
        * cd /etc/sysconfig/network-scripts/ [Reference](https://wiki.centos.org/FAQ/CentOS7) 
        
9. Key ring for automatic login [Reference](https://askubuntu.com/questions/31786/chrome-asks-for-password-to-unlock-keyring-on-startup)
     
