# ConstructOS Website

A modern, professional Flask website for ConstructOS - a construction project controls and scheduling company with XER2XLSXGANTT technology solutions.

## Project Structure

```
ConstructOS/
├── app.py                 # Flask application
├── requirements.txt       # Python dependencies
├── templates/             # Jinja2 templates
│   ├── base.html         # Base template with navigation and footer
│   ├── index.html        # Home page
│   ├── about.html        # About page
│   ├── services.html     # Services page
│   └── contact.html      # Contact page
└── static/               # Static files
    ├── css/
    │   └── styles.css    # Main stylesheet
    ├── js/
    │   └── script.js     # JavaScript for interactivity
    └── images/           # Image files (logo, etc.)
```

## Installation

1. **Install Python** (3.8 or higher recommended)

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

## Running the Application

1. **Start the Flask development server**:
   ```bash
   python app.py
   ```

2. **Open your browser** and navigate to:
   ```
   http://localhost:5002
   ```

The application will run in debug mode, which means:
- Automatic reloading when you make changes
- Detailed error messages
- Debug toolbar (in development)

## Features

- **Flask Backend**: Server-side rendering with Jinja2 templates
- **Responsive Design**: Works on desktop, tablet, and mobile devices
- **Modern UI**: Clean, professional design with smooth animations
- **Easy Navigation**: Sticky header with mobile hamburger menu
- **Contact Form**: Functional contact form with Flask validation and flash messages
- **Professional Color Scheme**: Modern purple color palette with elegant gradients
- **Image Gallery**: Showcase section for construction sites and site plans

## Image Setup

### Logo
The website references `static/images/logo-placeholder.png` in the navigation. When you have your logo ready:

1. Place your logo file in `static/images/` directory
2. Name it `logo-placeholder.png` (or update the filename in `templates/base.html`)
3. The logo will automatically display in the navigation (currently hidden via CSS until file exists)
4. Recommended logo size: 40px height (width will scale proportionally)

### Construction Images
The website uses SVG placeholders in `static/images/` for the hero and gallery. You can replace them with your own photos by adding JPGs with the same base names (the app will need to reference `.jpg` in the template if you prefer photos over the built-in placeholders):

- `hero-construction.svg` (or `.jpg`) - Hero background (recommended: 1920×1080px or larger)
- `construction-site-1.svg` (or `.jpg`) - Construction site photo (recommended: 800×600px or larger)
- `site-plan-1.svg` (or `.jpg`) - Site plan image
- `construction-site-2.svg` (or `.jpg`) - Additional construction photo
- `site-plan-2.svg` (or `.jpg`) - Additional site plan

**Note**: SVG placeholders are included so the site works without 404s. For best results, replace with high-quality construction and planning images (use the same filenames with `.jpg` and update the template to use `.jpg` if desired).

## Customization

### Colors
Edit the CSS variables in `static/css/styles.css`:
- `--primary-color`: Main brand color (currently #1a5490)
- `--secondary-color`: Secondary brand color (currently #2d7bc6)
- `--accent-color`: Accent color (currently #ff6b35)

### Contact Information
Update contact details in:
- `templates/base.html` (footer section)
- `templates/contact.html` (contact information section)

### Form Submission
The contact form is currently set up to show flash messages. To add email functionality:

1. Install an email library (e.g., `Flask-Mail` or use SMTP)
2. Update the `contact()` function in `app.py` to send emails
3. Configure email settings (SMTP server, credentials, etc.)

Example:
```python
from flask_mail import Mail, Message

# Configure mail settings
app.config['MAIL_SERVER'] = 'smtp.gmail.com'
app.config['MAIL_PORT'] = 587
app.config['MAIL_USE_TLS'] = True
app.config['MAIL_USERNAME'] = 'your-email@gmail.com'
app.config['MAIL_PASSWORD'] = 'your-password'

mail = Mail(app)

# In contact() function:
msg = Message('Contact Form Submission', 
              sender='your-email@gmail.com',
              recipients=['info@constructos.com'])
msg.body = f"Name: {name}\nEmail: {email}\nMessage: {message}"
mail.send(msg)
```

### Secret Key
**Important**: Change the `SECRET_KEY` in `app.py` before deploying to production:
```python
app.secret_key = 'your-secret-key-change-this-in-production'
```

Generate a secure key:
```python
import secrets
print(secrets.token_hex(32))
```

## Production Deployment

For production deployment, consider:

1. **Use a production WSGI server** (e.g., Gunicorn, uWSGI)
2. **Set environment variables** for sensitive configuration
3. **Use a reverse proxy** (e.g., Nginx)
4. **Enable HTTPS**
5. **Set `debug=False`** in production

Example with Gunicorn:
```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:8000 app:app
```

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Dependencies

- Flask 3.0.0 - Web framework

