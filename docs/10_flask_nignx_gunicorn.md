
---
layout: default
title: Deploy Flask
nav_order: 10
---

# How to Deploy Your Flask App Using Gunicorn and Nginx on a Linux Server

Are you ready to take your Flask application from development to production? Deploying your Flask app using Gunicorn and Nginx on a Linux server is a robust way to serve your web application efficiently. This tutorial will walk you through the steps needed to get your Flask app up and running on a Linux server.

## Prerequisites

Before we start, ensure you have the following:

1. A Flask application.
2. A Linux server (Ubuntu is used in this tutorial).
3. Basic knowledge of terminal commands.

## Step 1: Set Up Your Flask Application

First, make sure your Flask app is ready. Here’s a simple example of a `app.py` file:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello, World!"

if __name__ == "__main__":
    app.run()
```

## Step 2: Install Required Software

We need to install Python, pip, virtualenv, Gunicorn, and Nginx on the server. Open your terminal and run:

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv nginx
```

## Step 3: Create a Virtual Environment and Install Flask

Set up a virtual environment for your Flask app to manage dependencies:

```bash
mkdir ~/myflaskapp
cd ~/myflaskapp
python3 -m venv venv
source venv/bin/activate
pip install flask gunicorn
```

## Step 4: Test Your Flask Application with Gunicorn

Run your Flask application with Gunicorn to ensure everything is set up correctly:

```bash
gunicorn --bind 0.0.0.0:8000 app:app
```

If everything is working, you should be able to access your app at `http://<your-server-ip>:8000`.

## Step 5: Create a Systemd Service for Gunicorn

To manage Gunicorn, we’ll create a systemd service file. This will allow us to start, stop, and enable Gunicorn to run at boot.

Create the service file:

```bash
sudo nano /etc/systemd/system/myflaskapp.service
```

Add the following content:

```ini
[Unit]
Description=Gunicorn instance to serve myflaskapp
After=network.target

[Service]
User=yourusername
Group=www-data
WorkingDirectory=/home/yourusername/myflaskapp
Environment="PATH=/home/yourusername/myflaskapp/venv/bin"
ExecStart=/home/yourusername/myflaskapp/venv/bin/gunicorn --workers 3 --bind unix:/home/yourusername/myflaskapp/myflaskapp.sock -m 007 app:app

[Install]
WantedBy=multi-user.target
```

Make sure to replace `yourusername` with your actual username.

## Step 6: Configure Nginx

Next, we need to set up Nginx as a reverse proxy to forward requests to Gunicorn.

Create a new Nginx configuration file:

```bash
sudo nano /etc/nginx/sites-available/myflaskapp
```

Add the following content:

```nginx
server {
    listen 80;
    server_name your_domain_or_IP;

    location / {
        include proxy_params;
        proxy_pass http://unix:/home/yourusername/myflaskapp/myflaskapp.sock;
    }

    location /static {
        alias /home/yourusername/myflaskapp/static;
    }
}
```

Enable the Nginx configuration by creating a symbolic link:

```bash
sudo ln -s /etc/nginx/sites-available/myflaskapp /etc/nginx/sites-enabled
```

Test the Nginx configuration:

```bash
sudo nginx -t
```

Restart Nginx to apply the changes:

```bash
sudo systemctl restart nginx
```

## Step 7: Start and Enable Gunicorn Service

Start the Gunicorn service and enable it to start on boot:

```bash
sudo systemctl start myflaskapp
sudo systemctl enable myflaskapp
```

## Step 8: Access Your Flask App

Now, open your web browser and navigate to your server's domain or IP address. You should see your Flask app running!

## Troubleshooting

If you encounter any issues, here are a few tips:

- **Check Gunicorn Service Logs:** Look for errors in the Gunicorn service logs:

  ```bash
  sudo journalctl -u myflaskapp.service -f
  ```

- **Review Nginx Logs:** Check Nginx error logs for issues:

  ```bash
  sudo tail -f /var/log/nginx/error.log
  ```

- **File Permissions:** Ensure that the user running Gunicorn has the appropriate permissions to access your application files.

## Conclusion

Congratulations! You have successfully deployed your Flask app using Gunicorn and Nginx on a Linux server. This setup provides a scalable and efficient way to serve your web applications. Happy coding!
