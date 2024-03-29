# RHEL/CentOS

Add repository setting to /etc/yum.repos.d.
```
$ sudo vim /etc/yum.repos.d/trivy.repo
[trivy]
name=Trivy repository
baseurl=https://aquasecurity.github.io/trivy-repo/rpm/releases/$releasever/$basearch/
gpgcheck=0
enabled=1
$ sudo yum -y update
$ sudo yum -y install trivy
```

# Debian/Ubuntu

Add repository to /etc/apt/sources.list.d.
```
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

# Install Script

```
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin v0.18.3
```
# Docker

Replace [YOUR_CACHE_DIR] with the cache directory on your machine.
```
docker pull aquasec/trivy:0.18.3
```
Example for Linux:
```
docker run --rm -v [YOUR_CACHE_DIR]:/root/.cache/ aquasec/trivy:0.18.3 [YOUR_IMAGE_NAME]
```
Please re-pull latest aquasec/trivy if an error occurred.

# Quick Start
Simply specify an image name (and a tag).
```
trivy image [YOUR_IMAGE_NAME]
```


