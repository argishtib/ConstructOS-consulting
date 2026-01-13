# Complete Setup Walkthrough: Domain + Hosting + Email

Step-by-step guide to get your ConstructOS website online with domain, hosting, and email.

---

## 📋 Overview

This walkthrough will help you set up:
1. **Domain Registration** (constructos.com)
2. **Website Hosting** (DigitalOcean App Platform)
3. **Professional Email** (Google Workspace)

**Total Setup Time**: ~1-2 hours
**Total Cost**: ~$144/year (~$12/month)

---

## 🌐 STEP 1: Register Your Domain

### Where to Go: Namecheap.com

### Step-by-Step Instructions:

1. **Go to Namecheap**
   - Visit: [https://www.namecheap.com](https://www.namecheap.com)
   - Click "Sign Up" in the top right (or "Log In" if you have an account)

2. **Search for Your Domain**
   - In the search box, type: `constructos.com`
   - Click "Search" or press Enter
   - Check if `constructos.com` is available

3. **Add Domain to Cart**
   - If available, click "Add to Cart"
   - Click "View Cart" or checkout button

4. **Review Cart**
   - You'll see your domain in the cart
   - **Domain Privacy**: Already included FREE (don't pay extra)
   - **Auto-Renew**: Enable this to avoid losing your domain
   - **Registration Period**: 1 year is fine (can renew later)

5. **Create Account** (if new)
   - Fill in your information:
     - Email address
     - Password
     - Name
     - Address
     - Phone number

6. **Payment**
   - Select payment method (Credit Card, PayPal, etc.)
   - Review total (should be ~$12-15 for .com domain)
   - Click "Complete Purchase"

7. **Confirmation**
   - You'll receive an email confirmation
   - Domain is now registered under your account
   - **Write down your login credentials!**

### ✅ What You'll Have After This Step:
- Domain registered: `constructos.com`
- Access to Namecheap account
- Domain management dashboard

**Cost**: ~$12-15/year
**Time**: ~10 minutes

---

## 🚀 STEP 2: Set Up Website Hosting (DigitalOcean App Platform)

### Where to Go: DigitalOcean.com

### Step-by-Step Instructions:

#### Part A: Create DigitalOcean Account

1. **Go to DigitalOcean**
   - Visit: [https://www.digitalocean.com](https://www.digitalocean.com)
   - Click "Sign Up" in the top right

2. **Create Account**
   - Enter your email address
   - Create a password
   - Click "Create Account"

3. **Verify Email**
   - Check your email for verification link
   - Click the verification link
   - Log in to DigitalOcean

4. **Add Payment Method**
   - DigitalOcean requires a payment method (won't charge until you create resources)
   - Go to: Billing → Payment Methods
   - Add credit card or PayPal
   - You may get $200 free credit for 60 days (promotional)

#### Part B: Prepare Your Code

1. **Push Code to GitHub** (if not already done)
   - Go to [github.com](https://github.com)
   - Create a new repository (name it `constructos` or similar)
   - Push your Flask code to this repository
   - Make sure these files are in the repo:
     - `app.py`
     - `requirements.txt`
     - `gunicorn_config.py`
     - `Procfile`
     - `templates/` folder
     - `static/` folder

#### Part C: Create App on DigitalOcean

1. **Navigate to App Platform**
   - In DigitalOcean dashboard, click "Create" button (top right)
   - Select "Apps" from dropdown
   - Or go directly to: [cloud.digitalocean.com/apps](https://cloud.digitalocean.com/apps)

2. **Connect GitHub**
   - Click "GitHub" tab
   - Click "Authorize DigitalOcean"
   - Grant access to your repositories (or select specific repo)
   - Click "Authorize" on GitHub

3. **Select Repository**
   - Find and select your `constructos` repository
   - Select the branch (usually `main` or `master`)
   - Click "Next"

4. **Configure Your App**
   - **App Name**: `constructos` (or your choice)
   - **Region**: Choose closest to your users (e.g., "New York" for US)
   - DigitalOcean will auto-detect Flask

5. **Edit App Spec** (if needed)
   - **Build Command**: Should auto-detect as `pip install -r requirements.txt`
   - **Run Command**: Should auto-detect as `gunicorn -c gunicorn_config.py app:app`
   - **Type**: Web Service
   - **HTTP Port**: `5002` (or check your gunicorn_config.py)
   - Click "Edit" if you need to adjust

6. **Set Environment Variables**
   - Click "Add Variable"
   - Add these variables:
     ```
     FLASK_ENV = production
     SECRET_KEY = [generate a secure key - see below]
     ```
   - To generate SECRET_KEY:
     - In your terminal: `python -c "import secrets; print(secrets.token_hex(32))"`
     - Copy the output and paste as SECRET_KEY value

7. **Choose Plan**
   - Select "Basic" plan ($5/month)
   - Review resources (512 MB RAM, 0.5 vCPU should be fine for start)
   - Click "Next"

8. **Review & Launch**
   - Review your configuration
   - Click "Create Resources" or "Launch App"
   - DigitalOcean will start building your app

9. **Wait for Deployment**
   - First deployment takes 5-10 minutes
   - You'll see build logs in the dashboard
   - Status will change to "Live" when ready

10. **Get Your App URL**
    - Once live, you'll get a URL like: `constructos-xxxxx.ondigitalocean.app`
    - Test it in your browser to make sure it works

### ✅ What You'll Have After This Step:
- Website hosted at: `constructos-xxxxx.ondigitalocean.app`
- DigitalOcean account
- App running and accessible
- Automatic deployments from GitHub

**Cost**: $5/month (~$60/year)
**Time**: ~30-45 minutes

---

## 🔗 STEP 3: Connect Domain to Hosting

### Where to Go: Namecheap (Domain) + DigitalOcean (Hosting)

### Step-by-Step Instructions:

#### Part A: Get DNS Information from DigitalOcean

1. **Go to DigitalOcean App Dashboard**
   - Navigate to your app
   - Click "Settings" tab
   - Click "Domains" section

2. **Add Custom Domain**
   - Click "Add Domain"
   - Enter: `constructos.com`
   - Click "Add Domain"
   - Also add: `www.constructos.com`
   - DigitalOcean will show you DNS records

3. **Get DNS Records**
   - DigitalOcean will show something like:
     ```
     Type: A
     Name: @
     Value: 157.230.xxx.xxx (your app's IP)
     
     Type: CNAME
     Name: www
     Value: constructos-xxxxx.ondigitalocean.app
     ```
   - **Write these down** or keep this page open

#### Part B: Update DNS at Namecheap

1. **Log into Namecheap**
   - Go to [namecheap.com](https://www.namecheap.com)
   - Click "Account" → "Domain List"

2. **Access Domain Management**
   - Find `constructos.com`
   - Click "Manage" button

3. **Go to Advanced DNS**
   - Click "Advanced DNS" tab
   - You'll see current DNS records

4. **Update A Record**
   - Find or add A Record:
     - **Type**: A Record
     - **Host**: @ (or blank)
     - **Value**: [The IP address from DigitalOcean]
     - **TTL**: Automatic (or 3600)
   - Click checkmark to save

5. **Update CNAME Record**
   - Find or add CNAME Record:
     - **Type**: CNAME Record
     - **Host**: www
     - **Value**: [Your DigitalOcean app URL, e.g., constructos-xxxxx.ondigitalocean.app]
     - **TTL**: Automatic (or 3600)
   - Click checkmark to save

6. **Remove Old Records** (if any)
   - Delete any old A or CNAME records that conflict
   - Keep only the new ones you just added

7. **Save Changes**
   - All changes should save automatically
   - Note: DNS changes can take 24-48 hours to propagate

#### Part C: Wait and Verify

1. **Wait for DNS Propagation**
   - Can take 1-24 hours (usually 1-2 hours)
   - Check DNS propagation: [whatsmydns.net](https://www.whatsmydns.net)

2. **Test Your Domain**
   - Try visiting: `http://constructos.com`
   - Try: `http://www.constructos.com`
   - DigitalOcean will automatically add SSL/HTTPS after DNS propagates

3. **SSL Certificate**
   - DigitalOcean automatically issues SSL certificate
   - Can take up to 1 hour after DNS propagates
   - Once ready, both `http://` and `https://` will work

### ✅ What You'll Have After This Step:
- Domain pointing to your website: `constructos.com` and `www.constructos.com`
- Automatic HTTPS/SSL (free)
- Website accessible via custom domain

**Cost**: $0 (free - part of hosting)
**Time**: ~15 minutes (plus 1-24 hour wait for DNS)

---

## 📧 STEP 4: Set Up Professional Email (Google Workspace)

### Where to Go: Google Workspace (workspace.google.com)

### Step-by-Step Instructions:

#### Part A: Sign Up for Google Workspace

1. **Go to Google Workspace**
   - Visit: [https://workspace.google.com](https://workspace.google.com)
   - Click "Get Started" or "Start Free Trial"

2. **Choose Plan**
   - Select "Business Starter" ($6/month per user)
   - Click "Get Started" under that plan

3. **Enter Your Information**
   - **How many users?**: Start with 1 (you can add more later)
   - **Business name**: ConstructOS
   - **Your name**: Your name
   - **Current email**: Your personal email (for setup)
   - Click "Next"

4. **Enter Domain**
   - Select "Yes, I have a domain I can use"
   - Enter: `constructos.com`
   - Click "Next"

5. **Create Account**
   - **Username**: Choose your email username (e.g., `info`, `contact`, `yourname`)
   - This will be: `info@constructos.com` (or your choice)
   - Create a password
   - Click "Next"

6. **Account Details**
   - Enter your business information
   - Phone number
   - Click "Next"

7. **Start Free Trial**
   - Review your plan (usually 14-day free trial)
   - Enter payment information (won't charge until trial ends)
   - Click "Start Free Trial"

#### Part B: Verify Domain Ownership

1. **Go to Google Admin Console**
   - You'll be redirected to admin.google.com
   - Complete the setup wizard

2. **Verify Domain**
   - Google will ask you to verify domain ownership
   - You'll see several verification methods
   - **Recommended**: DNS TXT record method

3. **Add DNS Record at Namecheap**
   - Google will show you a TXT record to add
   - Example:
     ```
     Type: TXT
     Host: @
     Value: google-site-verification=xxxxxxxxxxxxx
     TTL: Automatic
     ```
   - Go back to Namecheap → Domain List → Manage → Advanced DNS
   - Add this TXT record
   - Wait 10-30 minutes for verification

4. **Verify in Google**
   - Go back to Google Admin Console
   - Click "Verify" button
   - Google will check the DNS record

#### Part C: Set Up Email Routing (MX Records)

1. **Get MX Records from Google**
   - In Google Admin Console, go to Setup → Email Routing
   - Google will show you MX records to add
   - They'll look like:
     ```
     Priority: 1
     Value: aspmx.l.google.com.
     ```
     (Multiple records will be shown)

2. **Add MX Records at Namecheap**
   - Go to Namecheap → Domain List → Manage → Advanced DNS
   - Find existing MX records (if any)
   - **Delete old MX records** (if you had email elsewhere)
   - Add new MX records from Google:
     - Type: MX Record
     - Host: @
     - Value: [First Google MX record, e.g., aspmx.l.google.com.]
     - Priority: [From Google, e.g., 1]
     - TTL: Automatic
   - Repeat for all MX records Google provides (usually 4-5 records)

3. **Wait for Propagation**
   - MX record changes can take 24-48 hours
   - Usually works within 1-4 hours

#### Part D: Access Your Email

1. **Access Gmail**
   - Go to [mail.google.com](https://mail.google.com)
   - Or visit: [https://mail.constructos.com](https://mail.constructos.com) (after DNS propagates)
   - Sign in with: `info@constructos.com` (or your chosen username)
   - Use the password you created

2. **Download Gmail App** (optional)
   - Install Gmail app on phone
   - Add your new email account
   - Sign in with `info@constructos.com`

### ✅ What You'll Have After This Step:
- Professional email: `info@constructos.com` (or your chosen username)
- Access to Gmail
- Calendar, Drive, Meet, and other Google Workspace tools
- Professional email for your business

**Cost**: $6/month per user (~$72/year for 1 user)
**Time**: ~30-45 minutes (plus DNS propagation wait)

---

## ✅ Final Checklist

After completing all steps, verify everything works:

- [ ] Domain registered: `constructos.com` ✓
- [ ] Website accessible at: `https://constructos.com` ✓
- [ ] HTTPS/SSL working (padlock icon in browser) ✓
- [ ] Email working: Can send/receive at `info@constructos.com` ✓
- [ ] DNS records properly configured ✓
- [ ] All services active and working ✓

---

## 💰 Total Cost Summary

### One-Time Costs:
- **Domain Registration**: $12-15/year

### Monthly/Annual Costs:
- **Hosting (DigitalOcean)**: $5/month = $60/year
- **Email (Google Workspace)**: $6/month = $72/year

### **Total: ~$144/year (~$12/month)**

---

## 🆘 Troubleshooting

### Domain Not Working?
- Wait 24-48 hours for DNS propagation
- Check DNS at [whatsmydns.net](https://www.whatsmydns.net)
- Verify DNS records are correct at Namecheap

### Website Not Loading?
- Check DigitalOcean app status (should be "Live")
- Verify environment variables are set correctly
- Check app logs in DigitalOcean dashboard

### Email Not Working?
- Wait 24-48 hours for MX record propagation
- Verify MX records are correct at Namecheap
- Check Google Admin Console for any errors

### Need Help?
- **Namecheap Support**: [support.namecheap.com](https://support.namecheap.com)
- **DigitalOcean Support**: [digitalocean.com/support](https://www.digitalocean.com/support)
- **Google Workspace Support**: Available in Admin Console

---

## 📚 Quick Links

### Domain Registration:
- Namecheap: [namecheap.com](https://www.namecheap.com)

### Hosting:
- DigitalOcean: [digitalocean.com](https://www.digitalocean.com)
- App Platform: [cloud.digitalocean.com/apps](https://cloud.digitalocean.com/apps)

### Email:
- Google Workspace: [workspace.google.com](https://workspace.google.com)
- Admin Console: [admin.google.com](https://admin.google.com)

### DNS Tools:
- DNS Checker: [whatsmydns.net](https://www.whatsmydns.net)

---

## 🎉 You're Done!

Your ConstructOS website is now live with:
✅ Professional domain
✅ Reliable hosting
✅ Professional email

All set up and ready for business!

