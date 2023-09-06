This doc contains the steps to install Hashicopr Vault in Ubuntu 22.4 LTS

Step 1: Prerequisites
a) You should have a running Ubuntu 22.04 LTS Server.

b) You should have sudo or root access to run privileged commands.

c) You should have apt or apt-get utility installed in your Server.

d) You should also have curl, apt-key and apt-add-repository utility installed in your Server.

Step 2: Install Vault using following commands
```
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg 
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vault
```
You should be able to type vault and it will display the common vault commands.
```
vault
```
Step – 3 Create the Hashicorp Vault Configuration File

We will store the HCL configuration file in the /etc/vault directory and the vault data in the /var/lib/vault/data directory. The -p switch makes all the directories in the chain that are needed.
Please use the following commands
```
sudo mkdir /etc/vault
sudo mkdir -p /var/lib/vault/data
```
Let’s make the Hashicorp Vault configuration file in the /etc/vault directory. Name this config.hcl
```
disable_cache = true
disable_mlock = true
ui = true
listener "tcp" {
   address          = "0.0.0.0:8200"
   tls_disable      = 1
    
}
storage "file" {
   path  = "/var/lib/vault/data"
 }
api_addr         = "http://0.0.0.0:8200"
max_lease_ttl         = "10h"
default_lease_ttl    = "10h"
cluster_name         = "vault"
raw_storage_endpoint     = true
disable_sealwrap     = true
disable_printable_check = true
```
Step 4 – Create the Hashicorp Vault Service File

Create service file /etc/systemd/system/vault.service and put the following contents to it.
```
[Unit]
Description="HashiCorp Vault - A tool for managing secrets"
Documentation=https://www.vaultproject.io/docs/
Requires=network-online.target
After=network-online.target
ConditionFileNotEmpty=/etc/vault/config.hcl

[Service]
ProtectSystem=full
ProtectHome=read-only
PrivateTmp=yes
PrivateDevices=yes
SecureBits=keep-caps
AmbientCapabilities=CAP_IPC_LOCK
NoNewPrivileges=yes
ExecStart=/usr/bin/vault server -config=/etc/vault/config.hcl
ExecReload=/bin/kill --signal HUP 
KillMode=process
KillSignal=SIGINT
Restart=on-failure
RestartSec=5
TimeoutStopSec=30
StartLimitBurst=3
LimitNOFILE=6553

[Install]
WantedBy=multi-user.target
```
Step 5 – Enable and Start and Vault Service
```
systemctl daemon-reload
systemctl start vault
systemctl enable vault
systemctl status vault
```
As you can see below, the vault service is running and no errors are displayed.

![image](https://github.com/techielins/Installation_Docs/assets/68058598/6c488869-0164-4d2a-8a94-3d6f77f88a64)

Step 6 – Open port 81200 in firewall
```
sudo ufw allow 8200/tcp
sudo ufw status |grep 8200  #To check the status if port is opened
```
Step 7 - Access Vault UI
At this point, you should be able to browse to the Vault UI and you will be seeing the following info in the page.

![image](https://github.com/techielins/Installation_Docs/assets/68058598/2c71f45e-5fa9-4581-a720-bf76788f33b0)

Step 7 (Optional) – Enable SSL for Vault UI and API
We can use a self-signed certificate to enable SSL in Vault in order to establish a secure connection. Please do the following steps for setting up SSL connection.
Create self signed SSL certificate
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vault-selfsigned.key -out /etc/ssl/certs/vault-selfsigned.crt
```
After you have the certificate and key file in place, you can edit your Vault config file /etc/vault/config.hcl to add the configuration for TLS, certificate, and key.
```
disable_cache = true
disable_mlock = true
ui = true
listener "tcp" {
   address          = "0.0.0.0:8200"
   tls_disable      = 0
   tls_cert_file = "/etc/ssl/certs/vault-selfsigned.crt"
   tls_key_file = "/etc/ssl/private/vault-selfsigned.key"

}
storage "file" {
   path  = "/var/lib/vault/data"
 }
api_addr         = "http://0.0.0.0:8200"
max_lease_ttl         = "10h"
default_lease_ttl    = "10h"
cluster_name         = "vault"
raw_storage_endpoint     = true
disable_sealwrap     = true
disable_printable_check = true
```
Note : The tls_disable is now set to 0. Also, we have added the tls_cert_file and tls_key_file pointing to the path where we created the self-signed certificate.
Restart the Vault service:
```
systemctl restart vault
```
Your Vault server should now be up and running on HTTPS.
![image](https://github.com/techielins/Installation_Docs/assets/68058598/45a85f76-303b-4a84-b60e-948ca54070f2)














 
