# Deployment Guide for ConstructOS Website

This guide covers different ways to launch your Flask website, from local development to production deployment.

## 🚀 Quick Start (Local Development)

### Option 1: Simple Flask Development Server

```bash
# Install dependencies
pip install -r requirements.txt

# Run the development server
python app.py
```

The site will be available at: `http://localhost:5002`

**Note**: This is only for development. The Flask development server is not suitable for production.

---

## 🌐 Production Deployment Options (Commercial/Production)

### Option 1: DigitalOcean App Platform (Recommended for Commercial Use)

**DigitalOcean App Platform** is ideal for commercial websites with reliable uptime and professional support.

**Pricing**: Starts at $5/month (Basic plan)

1. **Sign up** at [digitalocean.com](https://www.digitalocean.com)
2. **Create App** from GitHub repository
3. **Configure**:
   - Build Command: `pip install -r requirements.txt`
   - Run Command: `gunicorn -c gunicorn_config.py app:app`
   - Environment: Python 3.11
4. **Set Environment Variables**:
   - `FLASK_ENV=production`
   - `SECRET_KEY=your-generated-secret-key`
5. **Add Custom Domain** (included)
6. **Deploy!**

**Pros**: 
- Reliable uptime (99.95% SLA)
- Automatic HTTPS with Let's Encrypt
- Custom domains included
- Professional support
- Scales easily
- No spin-downs
- Built-in monitoring

**Cons**: Paid service (but very affordable)

**Best for**: Commercial websites that need reliability

---

### Option 2: Railway (Good for Commercial Use)

**Railway** offers easy deployment with good performance.

**Pricing**: $5/month (Hobby plan) or $20/month (Pro plan)

1. **Sign up** at [railway.app](https://railway.app)
2. **Create New Project**
3. **Connect GitHub repository**
4. **Railway auto-detects Flask** and sets up
5. **Set Environment Variables**:
   - `FLASK_ENV=production`
   - `SECRET_KEY=your-generated-secret-key`
6. **Add Custom Domain** (included in paid plans)
7. **Deploy!**

**Pros**: 
- Very easy setup
- Automatic HTTPS
- Custom domains
- Good performance
- No spin-downs on paid plans

**Cons**: Paid service required for commercial use

**Best for**: Quick deployment with minimal configuration

---

### Option 3: Render (Commercial Plans)

**Render** offers professional hosting with good features.

**Pricing**: $7/month (Starter plan) or $25/month (Standard plan)

1. **Sign up** at [render.com](https://render.com)
2. **Create a new Web Service**
3. **Connect your GitHub repository**
4. **Configure**:
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `gunicorn -c gunicorn_config.py app:app`
   - Environment: Python 3
5. **Set Environment Variables**:
   - `FLASK_ENV=production`
   - `SECRET_KEY=your-generated-secret-key`
6. **Add Custom Domain** (included)
7. **Deploy!**

**Pros**: 
- Automatic HTTPS
- Custom domains
- No spin-downs on paid plans
- Good documentation

**Cons**: More expensive than DigitalOcean

**Best for**: Teams that want managed hosting

---

### Option 3: PythonAnywhere

**PythonAnywhere** is great for Python web apps.

1. **Sign up** at [pythonanywhere.com](https://www.pythonanywhere.com)
2. **Upload your files** via web interface or Git
3. **Create a Web App**:
   - Choose Flask
   - Python 3.10 or 3.11
   - Point to your `app.py`
4. **Configure WSGI file**:
   ```python
   import sys
   path = '/home/yourusername/ConstructOS'
   if path not in sys.path:
       sys.path.append(path)
   
   from app import app as application
   ```
5. **Reload** the web app

**Pros**: Free tier available, Python-focused
**Cons**: Limited free tier features

---

### Option 4: AWS Elastic Beanstalk (Enterprise)

**AWS Elastic Beanstalk** for larger scale deployments.

**Pricing**: Pay for underlying resources (~$15-30/month)

1. **Sign up** at [aws.amazon.com](https://aws.amazon.com)
2. **Create Elastic Beanstalk application**
3. **Upload your code** or connect Git
4. **Configure environment**
5. **Deploy**

**Pros**: Highly scalable, enterprise-grade
**Cons**: More complex, higher cost

**Best for**: Large-scale commercial applications

---

### Option 5: VPS with Gunicorn + Nginx (Most Control)

For full control, deploy on a VPS (DigitalOcean, Linode, AWS EC2, etc.).

#### Step 1: Update requirements.txt
```bash
pip install gunicorn
echo "gunicorn==21.2.0" >> requirements.txt
```

#### Step 2: Update app.py for production
```python
if __name__ == '__main__':
    # Only use this for development
    app.run(debug=False, port=5002)
```

#### Step 3: Create systemd service
Create `/etc/systemd/system/constructos.service`:
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

#### Step 4: Configure Nginx
Create `/etc/nginx/sites-available/constructos`:
```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

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

#### Step 5: Enable SSL with Let's Encrypt
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

**Pros**: Full control, best performance, scalable
**Cons**: Requires server management knowledge

---

## 🔒 Pre-Production Checklist

Before deploying to production:

### 1. Update Secret Key
```python
import secrets
print(secrets.token_hex(32))
```
Then set it as an environment variable:
```bash
export SECRET_KEY='your-generated-key-here'
```

### 2. Update app.py
```python
import os

app = Flask(__name__)
app.secret_key = os.environ.get('SECRET_KEY', 'fallback-for-dev-only')
app.config['DEBUG'] = os.environ.get('FLASK_ENV') != 'production'
```

### 3. Update requirements.txt
Add gunicorn for production:
```
Flask==3.0.0
gunicorn==21.2.0
```

### 4. Test Locally with Gunicorn
```bash
pip install gunicorn
gunicorn -c gunicorn_config.py app:app
```

### 5. Add Images
Make sure to add your construction images to `static/images/`:
- hero-construction.jpg
- construction-site-1.jpg
- site-plan-1.jpg
- construction-site-2.jpg
- site-plan-2.jpg

---

## 📝 Environment Variables

Set these in your hosting platform:

- `FLASK_ENV=production` - Disables debug mode
- `SECRET_KEY=your-secret-key` - For session security
- `PORT=5002` - Port (if needed by platform)

---

## 🎯 Recommended Approach for Commercial Use

**Best Overall**: **DigitalOcean App Platform** ($5/month)
- Best balance of price, reliability, and features
- Professional support
- Perfect for commercial websites

**Easiest Setup**: **Railway** ($5/month)
- Fastest deployment
- Great for getting online quickly

**Most Control**: **VPS with Nginx** ($5-10/month)
- Full control over server
- Best for custom requirements

**For Testing**: Use the local Flask development server (`python app.py`)

---

## 🆘 Troubleshooting

### Port Already in Use
If port 5002 is busy:
```bash
# Windows
netstat -ano | findstr :5002
taskkill /PID <PID> /F

# Linux/Mac
lsof -ti:5002 | xargs kill -9
```

### Static Files Not Loading
Make sure `static/` folder is in the root directory and images are in `static/images/`.

### Gunicorn Not Found
```bash
pip install gunicorn
```

---

## 📚 Additional Resources

- [Flask Deployment Options](https://flask.palletsprojects.com/en/latest/deploying/)
- [Gunicorn Documentation](https://docs.gunicorn.org/)
- [Railway Documentation](https://docs.railway.app/)
- [Render Documentation](https://render.com/docs)

