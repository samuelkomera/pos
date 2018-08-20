# Kiosk Mode - CentOS

1. sudo apt-get update && sudo apt-get upgrade
2. Create User Accounts
   * Create 2 users. One user is already created -- the account you’re logged in as. For this we’ll assume the account name is      pos. Create a second user named kiosk. This will become the auto-logged in user.
3. Add Users To sudoers I choose to add the 2 users to the sudoers file so I wouldn’t get prompted for passwords all the time.    Run visudo and at the bottom add
   * kiosk ALL=(ALL) NOPASSWD: ALL
   * pos ALL=(ALL) NOPASSWD: ALL4.
4. Setup Auto Login. Now we can setup the auto login process. We need the user Kiosk to auto login on every reboot.
   * /etc/gdm/custom.conf
       
      # Enable automatic login for user
      ~~~~~ [daemon]
      AutomaticLogin=username
      AutomaticLoginEnable=True ~~~~~
