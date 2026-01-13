# Hostinger Setup Guide for Flask Apps

## ⚠️ Important: Hostinger Shared Hosting vs. VPS

### Shared Hosting (NOT Recommended for Flask)
- **Cost**: $2-4/month
- **Problem**: Doesn't support Python/Flask applications
- **Best for**: WordPress, static sites, PHP
- **Not suitable**: For your ConstructOS Flask website

### VPS Hosting (Required for Flask)
- **Cost**: $4-10/month
- **Supports**: Python, Flask, custom applications
- **Requires**: Server management knowledge
- **Best for**: Users comfortable with Linux/server setup

---

## 🚀 Setting Up Flask on Hostinger VPS

### Step 1: Choose Hostinger VPS Plan
1. Go to [hostinger.com](https://www.hostinger.com)
2. Select **VPS Hosting**
3. Choose a plan:
   - **VPS 1**: $4.99/month (1 vCPU, 1GB RAM) - Good for small sites
   - **VPS 2**: $8.99/month (2 vCPU, 2GB RAM) - Recommended
   - **VPS 3**: $12.99/month (3 vCPU, 3GB RAM) - For larger sites

### Step 2: Initial Server Setup
1. **Access your VPS** via SSH
2. **Update system**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

3. **Install Python and dependencies**:
   ```bash
   sudo apt install python3 python3-pip python3-venv nginx -y
   ```

### Step 3: Deploy Your Flask App
1. **Clone your repository** or upload files:
   ```bash
   cd /var/www
   sudo mkdir constructos
   sudo chown $USER:$USER constructos
   cd constructos
   git clone <your-repo-url> .
   ```

2. **Create virtual environment**:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

3. **Test your app**:
   ```bash
   gunicorn -c gunicorn_config.py app:app
   ```

### Step 4: Set Up Gunicorn Service
1. **Create systemd service** (`/etc/systemd/system/constructos.service`):
   ```ini
   [Unit]
   Description=ConstructOS Flask Application
   After=network.target

   [Service]
   User=www-data
   Group=www-data
   WorkingDirectory=/var/www/constructos
   Environment="PATH=/var/www/constructos/venv/bin"
   ExecStart=/var/www/constructos/venv/bin/gunicorn -c gunicorn_config.py app:app

   [Install]
   WantedBy=multi-user.target
   ```

2. **Enable and start service**:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable constructos
   sudo systemctl start constructos
   ```

### Step 5: Configure Nginx
1. **Create Nginx config** (`/etc/nginx/sites-available/constructos`):
   ```nginx
   server {
       listen 80;
       server_name constructos.com www.constructos.com;

       location / {
           proxy_pass http://127.0.0.1:5002;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }

       location /static {
           alias /var/www/constructos/static;
           expires 30d;
           add_header Cache-Control "public, immutable";
       }
   }
   ```

2. **Enable site**:
   ```bash
   sudo ln -s /etc/nginx/sites-available/constructos /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl reload nginx
   ```

### Step 6: Set Up SSL (HTTPS)
1. **Install Certbot**:
   ```bash
   sudo apt install certbot python3-certbot-nginx -y
   ```

2. **Get SSL certificate**:
   ```bash
   sudo certbot --nginx -d constructos.com -d www.constructos.com
   ```

---

## 💰 Hostinger Pricing Comparison

### Option A: Hostinger VPS
- **VPS 1**: $4.99/month = ~$60/year
- **Domain**: ~$10/year (if bought separately)
- **Total**: ~$70/year
- **Pros**: Very affordable, full control
- **Cons**: Requires technical setup, server management

### Option B: DigitalOcean App Platform (Easier)
- **Hosting**: $5/month = $60/year
- **Domain**: ~$12/year (Namecheap)
- **Total**: ~$72/year
- **Pros**: Managed, easier setup, professional support
- **Cons**: Slightly more expensive

**Difference**: ~$2/year more for much easier setup!

---

## ⚖️ Hostinger vs. Managed Platforms

| Feature | Hostinger VPS | DigitalOcean App Platform |
|---------|---------------|---------------------------|
| **Cost** | $4.99/month | $5/month |
| **Setup Difficulty** | Advanced | Easy |
| **Server Management** | You manage | Managed |
| **Automatic Deployments** | Manual | Automatic (Git) |
| **SSL/HTTPS Setup** | Manual (Certbot) | Automatic |
| **Support** | Good | Excellent |
| **Best For** | Budget + Technical | Commercial + Easy |

---

## 🎯 Recommendation

### Choose Hostinger VPS if:
- ✅ You're comfortable with Linux/server management
- ✅ You want the absolute lowest cost
- ✅ You don't mind manual setup and maintenance
- ✅ You have technical skills

### Choose DigitalOcean App Platform if:
- ✅ You want the easiest setup
- ✅ You prefer managed hosting
- ✅ You want automatic deployments
- ✅ You value time over saving $2/year
- ✅ **This is recommended for commercial use**

---

## 📝 Quick Setup Checklist for Hostinger VPS

- [ ] Purchase Hostinger VPS plan
- [ ] Access server via SSH
- [ ] Install Python, pip, venv, nginx
- [ ] Upload/clone your Flask app
- [ ] Create virtual environment
- [ ] Install dependencies
- [ ] Set up Gunicorn service
- [ ] Configure Nginx
- [ ] Set up SSL with Certbot
- [ ] Point domain DNS to VPS IP
- [ ] Test website

**Estimated setup time**: 2-4 hours (if you're familiar with Linux)

---

## 🆘 Need Help?

If you're not comfortable with server management, **DigitalOcean App Platform** is worth the extra $2/year for:
- Automatic setup
- Managed hosting
- Professional support
- Time saved

For a commercial company, the managed platform is usually the better choice unless you have technical staff.


