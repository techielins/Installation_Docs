## Install Docker on CentOS 7 based server.

Docker is an application that makes it simple and easy to run application processes in a container, which are like virtual machines

Step 1 — Installing Docker

To get the latest and greatest version, install Docker from the official Docker repository.
```
sudo yum check-update
```
Now run this command. It will add the official Docker repository, download the latest version of Docker, and install it:
```
curl -fsSL https://get.docker.com/ | sh
```
After installation has completed, start the Docker daemon:
```
sudo systemctl start docker
```
Verify that it’s running:
```
sudo systemctl status docker
```
The output should be similar to the following, showing that the service is active and running:
```
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: active (running) since Thu 2020-07-09 11:57:50 UTC; 54s ago
     Docs: https://docs.docker.com
 Main PID: 5474 (dockerd)
    Tasks: 10
   Memory: 41.9M
   CGroup: /system.slice/docker.service
           └─5474 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```
Lastly, make sure it starts at every server reboot:
```
sudo systemctl enable docker
```

## Install Docker on Ubuntu 22

First, update your existing list of packages:
```
sudo apt update
```
Next, install a few prerequisite packages which let apt use packages over HTTPS:
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
Then add the GPG key for the official Docker repository to your system:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
Add the Docker repository to APT sources:
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Update your existing list of packages again for the addition to be recognized:
```
sudo apt update
```
Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:
```
apt-cache policy docker-ce
```
Finally, install Docker:
```
sudo apt install docker-ce
```
Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:
```
sudo systemctl status docker
```

