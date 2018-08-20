# Kiosk Mode - CentOS

1. sudo apt-get update && sudo apt-get upgrade
2. Create User Accounts
   * Create 2 users. One user is already created -- the account you’re logged in as. For this we’ll assume the account name is      pos. Create a second user named kiosk. This will become the auto-logged in user.
3. Add Users To sudoers I choose to add the 2 users to the sudoers file so I wouldn’t get prompted for passwords all the time.    Run visudo and at the bottom add
    ~~~
    * __kiosk ALL=(ALL) NOPASSWD: ALL__
    * __pos ALL=(ALL) NOPASSWD: ALL__
4. Setup Auto Login. Now we can setup the auto login process. We need the user Kiosk to auto login on every reboot.
   * edit the following lines to the file __/etc/gdm/custom.conf__
      ~~~~~ 
      __# Enable automatic login for user__
      __[daemon]__
      __AutomaticLogin=username__
      __AutomaticLoginEnable=True__ 
