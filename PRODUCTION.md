
#  PRODUCTION SERVER SETUP GUIDE 

Yeh file aapko kabhi bhi production setup revise karne me help karegi. Har command ke sath uska matlab diya gaya hai.

---

## 🔧 STEP 1: System Update aur Node.js Install karo

```bash
sudo apt update && sudo apt upgrade -y
```
➡️ System ke sabhi packages ko update aur upgrade karta hai.

```bash
sudo apt install curl -y
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs -y
node -v
npm -v
```
➡️ Node.js v18 aur npm install karta hai.

---

## 🔧 STEP 2: PM2 Install karo (Node.js process manage karne ke liye)

```bash
sudo npm install -g pm2
```
➡️ PM2 ek process manager hai jo Node.js app ko background me chalane, crash hone pr restart karne aur reboot ke baad automatically chalu karne ke kaam aata hai.

---

## 🌐 STEP 3: NGINX Install karo aur Configure karo

```bash
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo ufw allow 'Nginx Full'
sudo ufw enable
sudo systemctl status nginx
```
➡️ NGINX web server ko install, start, aur firewall me allow karta hai.

Check aur restart NGINX:

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

## 🔁 STEP 4: PM2 Commands for Backend

```bash
pm2 start SERVER.js --name SERVER
pm2 save
pm2 startup
pm2 status
```

➡️ App ko PM2 se start karna, save karna, aur boot hone pr automatic start setup.

---

## 🛠️ STEP 5: NGINX Config File Edit karo

```bash
sudo nano /etc/nginx/sites-available/default
```
```
ls /etc/nginx/sites-available/
```
Aur file me ye likho:

```nginx
server {
    listen 80;
    listen [::]:80;
    server_name resultatenquiry.univindia.com;

    root /var/www/react-app;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    location /api/ {
        proxy_pass http://localhost:5001/api/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

➡️ Ye React frontend serve karta hai aur `/api/` wali requests ko backend Node.js server pr forward karta hai.

---

## 📦 Backend Zip File se Upload karna

```bash
scp backend.zip root@139.84.167.22:/var/www/
cd /var/www/
unzip backend.zip
mv extracted-folder-name node-backend
```

➡️ Agar backend zip me hai to isse upload aur extract kar sakte ho.

---

## 📁 Backend Folder Banana aur Permission Set Karna

```bash
sudo mkdir -p /var/www/node-backend
sudo chown -R $USER:$USER /var/www/node-backend
# Ya root ho to:
sudo chown -R root:root /var/www/node-backend
```

---

## 🐬 MySQL Related Commands

```bash
systemctl status mysql
sudo systemctl stop mysql
sudo systemctl start mysql
sudo systemctl restart mysql
mysqld_safe --skip-grant-tables &
mysql -u root -p
```

➡️ MySQL ko start/stop karne aur root user se login karne ke liye.

---

## 🔒 Firewall aur SSH

```bash
sudo ufw allow 'Nginx Full'
sudo ufw enable
sudo ufw status
ssh root@139.84.167.22
```

➡️ Firewall allow aur SSH connection ke liye.

---

## 📂 File aur Port Checking Commands

```bash
nano .env             # .env file banaye/edit karein
cat .env              # .env file dekhein
ls -a                 # Hidden files dekhein
sudo lsof -i -P -n | grep LISTEN  # Port usage check karein
```

---

## 🌀 PM2 Important Commands

```bash
pm2 start app/index.js --name backend-app --update-env
pm2 restart backend-app --update-env
pm2 stop backend-app
pm2 delete backend-app
pm2 logs backend-app
pm2 list
```

➡️ App start/restart, stop, delete, aur logs dekhne ke liye.

---

## 🧠 Linux Useful Commands

```bash
lscpu               # CPU info
free -h             # RAM info
lsblk               # Disk partitions
df -h               # Disk usage
dpkg -l | less      # Installed packages
apt-mark showmanual # Manually installed packages
systemctl list-units --type=service --state=running  # Running services
```

---

## 🔍 grep Example (search command)

```bash
cat .env | grep DB_
cat /var/www/Backend/.env | grep DB_DIALECT
```

➡️ kisi file me keyword search karne ke liye.

---

## 🧼 Disable Default NGINX Site (Optional)

```bash
sudo rm /etc/nginx/sites-enabled/default
```

---

## 🧠 curl aur apt ka kaam

```bash
curl - command-line tool for fetching data
apt – Ubuntu package installer/upgrader
```

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
➡️ Node.js source setup karta hai.

---

✅ **Ab aap ise GitHub pe push karke kabhi bhi refer kar sakte ho bina kisi change ke.**
