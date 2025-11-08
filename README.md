## Install Requirements:
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```
## Setup Project:
**1. Downloading and installing**
```bash
git clone https://github.com/gabriel-guelfi/ssl-gen.git

sudo rm -Rf ssl-gen/.git
cd ssl-gen
```

**2. Setting your DNS on the script:** In the marked locations, change the DNS, replacing it with your own:
Type this on the terminal:
```bash
sudo nano init-letsencrypt.sh
```
Then edit the following lines:
```bash
...
domains=(your-domain-here.com www.your-domain-here.com) # <== CONFIGURE YOUR DOMAINS HERE **
...
```
Save and exit.

**3. Set your DNS on the nginx config file:** In the marked locations, change the DNS, replacing it with your own:]
```bash
sudo nano /data/nginx/app.conf
```
Then edit the following lines:
```bash
server {
   listen 80;
    server_name your-domain-here.com; # <== CONFIGURE YOUR DOMAIN HERE **

   ...
}
server {
    listen 443 ssl;
    server_name your-domain-here.com; # <== CONFIGURE YOUR DOMAIN HERE **

    ssl_certificate /etc/letsencrypt/live/your-domain-here.com/fullchain.pem; # <== CONFIGURE YOUR DOMAIN HERE (folder name) **
    ssl_certificate_key /etc/letsencrypt/live/your-domain-here.com/privkey.pem; # <== CONFIGURE YOUR DOMAIN HERE (folder name) **
    ...
}
```
Save and exit.

**4. Give the exec permission, then execute:**
```bash
sudo chmod +x init-letsencrypt.sh
```
```bash
sudo ./init-letsencrypt.sh
```

After executing, stop the containers: `sudo docker compose down`
