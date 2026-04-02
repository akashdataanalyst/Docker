# Docker
Erpnext install by Docker

# Step 2 — SSH se Connect Karo
ssh -i /path/to/yourkey.pem ubuntu@<EC2-PUBLIC-IP>

Step 3 — Docker Install Karo
# System update
sudo apt update && sudo apt upgrade -y

# Docker install karo (official script se)
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Docker bina sudo ke chalao
sudo usermod -aG docker $USER
newgrp docker

# Docker Compose install karo
sudo apt install docker-compose-plugin -y

# Check karo dono install hue ya nahi
docker --version
docker compose version

Step 4 — ERPNext Docker Repo Clone Karo
# Git install
sudo apt install git -y

# Frappe Docker repo clone karo
git clone https://github.com/frappe/frappe_docker.git
cd frappe_docker

Step 5 — Environment File Configure Karo
# Example env file copy karo
cp example.env .env

# .env file edit karo
nano .env
.env file mein yeh values set karo:
envFRAPPE_VERSION=version-15
DB_PASSWORD=apna_strong_password_yahan
SITE_NAME=mysite.localhost
INSTALL_APPS=erpnext

Step 6 — Containers Start Karo
# Sab containers ek saath start karo
docker compose -f compose.yaml \
  -f overrides/compose.mariadb.yaml \
  -f overrides/compose.redis.yaml \
  up -d

# Site create karo (pehli baar sirf ek baar)
docker compose exec backend \
  bench new-site mysite.localhost \
  --db-root-password apna_strong_password_yahan \
  --admin-password admin@123

# ERPNext app install karo site pe
docker compose exec backend \
  bench --site mysite.localhost install-app erpnext

# Migrate karo
docker compose exec backend \
  bench --site mysite.localhost migrate
```

---

## Step 7 — Access Karo

Browser mein jaao aur type karo:
```
http://<EC2-PUBLIC-IP>
Login: Administrator / jo bhi admin password set kiya

