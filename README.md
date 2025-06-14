# Python API Server

This is a simple Flask-based API server that can handle HTTP requests.

## Setup Instructions

1. Install Python 3.x if not already installed:
```bash
sudo apt update
sudo apt install python3 python3-pip
```

2. Create a virtual environment (recommended):
```bash
python3 -m venv venv
source venv/bin/activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

## Running the Server

### Development Mode
To run in development mode:
```bash
python app.py
```

### Production Mode
For production, use Gunicorn:
```bash
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

## API Endpoints

1. GET `/api/hello`
   - Returns a hello message
   - Example response: `{"message": "Hello, World!"}`

2. POST `/api/echo`
   - Echoes back the JSON data sent in the request
   - Example request body: `{"key": "value"}`
   - Example response: `{"received_data": {"key": "value"}}`

## Running as a Service

To run the application as a systemd service:

1. Create a service file:
```bash
sudo nano /etc/systemd/system/api-server.service
```

2. Add the following content:
```ini
[Unit]
Description=Python API Server
After=network.target

[Service]
User=your_username
WorkingDirectory=/path/to/your/app
Environment="PATH=/path/to/your/app/venv/bin"
ExecStart=/path/to/your/app/venv/bin/gunicorn -w 4 -b 0.0.0.0:5000 app:app

[Install]
WantedBy=multi-user.target
```

3. Enable and start the service:
```bash
sudo systemctl enable api-server
sudo systemctl start api-server
```

4. Check status:
```bash
sudo systemctl status api-server
```

## Using Nginx as a Reverse Proxy (Optional)

For better security and performance, you can set up Nginx as a reverse proxy:

1. Install Nginx:
```bash
sudo apt install nginx
```

2. Create a new Nginx configuration:
```bash
sudo nano /etc/nginx/sites-available/api-server
```

3. Add the following configuration:
```nginx
server {
    listen 80;
    server_name your_domain.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

4. Enable the site and restart Nginx:
```bash
sudo ln -s /etc/nginx/sites-available/api-server /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
``` #
