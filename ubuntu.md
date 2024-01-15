After installing ubuntu 20.4 in virtual box the terminal may not work. It just cannot be open due to some reason.

To fix it, you need to open Regional settings and change language to any other language. After that you need to reboot.

Temrinal should work after that and you can put back the default language or select a proper one.

Sudo command may also not work on a clean ubuntu installation.

To fix it:

1) You need to open the terminal (you fixed it in the step above) 
2) Type su root
  (Root user/pass should be set when you install .iso image in virtual box)
3) apt-get install sudo -y
   (Install sudo command)
5) adduser username sudo
  (Add user to sudo group)
6) chmod 0440 /etc/sudoers
   (Setting permissions may not be necessary)
7) shutdown/start
8) Open terminal and check if sudo works
