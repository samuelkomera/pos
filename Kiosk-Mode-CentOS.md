# Kiosk Mode - CentOS

1. sudo apt-get update && sudo apt-get upgrade
2. Create User Accounts
   * Create 2 users. One user is already created -- the account you’re logged in as. For this we’ll assume the account name is      pos. Create a second user named kiosk. This will become the auto-logged in user.
3. Add Users To sudoers I choose to add the 2 users to the sudoers file so I wouldn’t get prompted for passwords all the time.    Run visudo and at the bottom add
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
    
5. * Chrome asks for password to unlock keyring on startup
   * google-chrome --password-store=basic
6. Disable the user list on lock screen : [UserList Disabling](https://help.gnome.org/admin/system-admin-guide/stable/login-userlist-disable.html.en)
7. Disabling auto screen Locking
    ~~~~~~
   * su - gdm -s /bin/sh
   * export $(dbus-launch)
   * GSETTINGS_BACKEND=dconf gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-ac-type 'nothing'
   * Exit or Ctrl+D
   * systemctl restart gdm
