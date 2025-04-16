# LLM Brain Web Application

This is a web application for visualizing how an LLM thinks using the Goodfire API.

## Deployment Instructions for www.nahatakarn.com/llmbrain

### Option 1: Deploy with a WSGI Server (Recommended)

1. Install the required dependencies:
   ```
   pip install -r requirements.txt
   ```

2. Install a WSGI server like Gunicorn:
   ```
   pip install gunicorn
   ```

3. Run the application with Gunicorn:
   ```
   cd /path/to/nahatakarn.com/llmbrain
   gunicorn --bind 0.0.0.0:5000 wsgi:app
   ```

4. Configure your web server (Nginx or Apache) to proxy requests to the WSGI server.

### Nginx Configuration Example

Add the following to your Nginx configuration:

```nginx
location /llmbrain {
    proxy_pass http://localhost:5000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}

location /llmbrain/static {
    alias /path/to/nahatakarn.com/llmbrain/static;
}
```

### Option 2: Deploy with Apache + mod_wsgi

1. Install mod_wsgi for Apache:
   ```
   pip install mod_wsgi
   ```

2. Add the following to your Apache configuration:
   ```apache
   <VirtualHost *:80>
       ServerName nahatakarn.com
       
       WSGIDaemonProcess llmbrain python-home=/path/to/your/virtualenv python-path=/path/to/nahatakarn.com/llmbrain
       WSGIProcessGroup llmbrain
       WSGIScriptAlias /llmbrain /path/to/nahatakarn.com/llmbrain/wsgi.py
       
       <Directory /path/to/nahatakarn.com/llmbrain>
           Require all granted
       </Directory>
       
       Alias /llmbrain/static /path/to/nahatakarn.com/llmbrain/static
       <Directory /path/to/nahatakarn.com/llmbrain/static>
           Require all granted
       </Directory>
   </VirtualHost>
   ```

### Important Notes

1. Make sure the API key for Goodfire is properly set.
2. Ensure that your server has outbound internet access to connect to the Goodfire API.
3. For security reasons, don't run the application in debug mode in production.

## Local Development

To run the application locally:

```
cd /path/to/nahatakarn.com/llmbrain
python app.py
```

The application will be available at http://localhost:5000/. 