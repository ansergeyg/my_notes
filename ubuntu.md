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

Use docker without sudo:

https://docs.docker.com/engine/install/linux-postinstall/


Linux post-installation steps for Docker Engine
These optional post-installation procedures describe how to configure your Linux host machine to work better with Docker.

Manage Docker as a non-root user
The Docker daemon binds to a Unix socket, not a TCP port. By default it's the root user that owns the Unix socket, and other users can only access it using sudo. The Docker daemon always runs as the root user.

If you don't want to preface the docker command with sudo, create a Unix group called docker and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the docker group. On some Linux distributions, the system automatically creates this group when installing Docker Engine using a package manager. In that case, there is no need for you to manually create the group.

Warning

The docker group grants root-level privileges to the user. For details on how this impacts security in your system, see Docker Daemon Attack Surface.

Note

To run Docker without root privileges, see Run the Docker daemon as a non-root user (Rootless mode).

To create the docker group and add your user:

Create the docker group.

```
 sudo groupadd docker
```
Add your user to the docker group.


 sudo usermod -aG docker $USER
Log out and log back in so that your group membership is re-evaluated.

If you're running Linux in a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.

You can also run the following command to activate the changes to groups:


 newgrp docker
Verify that you can run docker commands without sudo.


 docker run hello-world
This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.

If you initially ran Docker CLI commands using sudo before adding your user to the docker group, you may see the following error:


WARNING: Error loading config file: /home/user/.docker/config.json -
stat /home/user/.docker/config.json: permission denied
This error indicates that the permission settings for the ~/.docker/ directory are incorrect, due to having used the sudo command earlier.

To fix this problem, either remove the ~/.docker/ directory (it's recreated automatically, but any custom settings are lost), or change its ownership and permissions using the following commands:


 sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
 sudo chmod g+rwx "$HOME/.docker" -R
Configure Docker to start on boot with systemd
Many modern Linux distributions use systemd to manage which services start when the system boots. On Debian and Ubuntu, the Docker service starts on boot by default. To automatically start Docker and containerd on boot for other Linux distributions using systemd, run the following commands:


 sudo systemctl enable docker.service
 sudo systemctl enable containerd.service
To stop this behavior, use disable instead.


 sudo systemctl disable docker.service
 sudo systemctl disable containerd.service
If you need to add an HTTP proxy, set a different directory or partition for the Docker runtime files, or make other customizations, see customize your systemd Docker daemon options.

Configure default logging driver
Docker provides logging drivers for collecting and viewing log data from all containers running on a host. The default logging driver, json-file, writes log data to JSON-formatted files on the host filesystem. Over time, these log files expand in size, leading to potential exhaustion of disk resources.

To avoid issues with overusing disk for log data, consider one of the following options:

Configure the json-file logging driver to turn on log rotation.
Use an alternative logging driver such as the "local" logging driver that performs log rotation by default.
Use a logging driver that sends logs to a remote logging aggregator.
