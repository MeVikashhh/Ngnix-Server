### **1. What is NGINX?**
- **NGINX** (pronounced "Engine-X") is a high-performance web server and reverse proxy server.
- Originally developed as a web server, but now also widely used as:
  - **Reverse Proxy**
  - **Load Balancer**
  - **HTTP Cache**
  - **Forward Proxy**
- Known for handling a large number of concurrent connections with low memory usage.

---

### **2. Key Features of NGINX**
- **Event-driven architecture**: Uses an asynchronous, non-blocking model to handle many connections efficiently.
- **Reverse Proxy**: Routes client requests to backend servers and returns the server's response to the client.
- **Load Balancing**: Distributes incoming traffic across multiple backend servers to ensure no single server is overwhelmed.
- **Static Content Serving**: Efficiently serves static files such as HTML, CSS, images, and JavaScript.
- **Caching**: Can act as a caching server to store frequently requested resources and reduce load on backend servers.
- **Security**: Supports SSL/TLS for secure connections and can act as a secure reverse proxy for backend servers.
- **FastCGI Support**: Integrates with applications like PHP-FPM to process dynamic content.

---

### **3. NGINX as a Web Server**
- **High Performance**: NGINX is optimized to handle high concurrency, making it ideal for busy websites.
- **Static File Handling**: NGINX can efficiently serve static files with minimal resource overhead.

---

### **4. NGINX as a Reverse Proxy**
- **Forward Traffic**: Receives client requests and forwards them to backend servers (e.g., app servers like Node.js or Python).
- **SSL Termination**: Decrypts incoming SSL traffic and forwards it as plain HTTP to backend servers, reducing their load.
- **Load Balancing**: Supports multiple load balancing algorithms like round-robin, least connections, and IP hash.
  
   Example Configuration: 
   ```bash
   server {
       listen 80;
       server_name example.com;

       location / {
           proxy_pass http://backend_server;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       }
   }
   ```

---

### **5. NGINX as a Load Balancer**
- **Distributes traffic**: Across multiple backend servers to improve performance and reliability.
- **Load Balancing Methods**:
  - **Round Robin**: Distributes requests sequentially.
  - **Least Connections**: Sends requests to the server with the fewest connections.
  - **IP Hash**: Ensures clients are always directed to the same server based on their IP.

---

### **6. NGINX as a Caching Server**
- **Response Caching**: Caches content from upstream servers, improving performance and reducing load.
- **Cache Control**: Can be configured to cache responses for a set time, or based on HTTP headers.

---

### **7. SSL/TLS Support**
- **SSL Offloading**: NGINX can handle SSL/TLS encryption and decryption, offloading this process from backend servers.
- **HTTP/2 Support**: Improves performance over SSL/TLS connections by allowing multiplexing and header compression.

---

### **8. NGINX as a Forward Proxy**
- While more common as a reverse proxy, NGINX can also be configured as a forward proxy for client requests.

---

### **9. NGINX Configuration**
- **Configuration file**: Typically located in `/etc/nginx/nginx.conf` or under `/etc/nginx/sites-available/`.
- **Modules**: NGINX uses a modular architecture where modules must be compiled into the binary.
  
   Example of basic server block:
   ```bash
   server {
       listen 80;
       server_name yourdomain.com;

       location / {
           root /var/www/html;
           index index.html index.htm;
       }
   }
   ```

---

### **10. NGINX vs. Apache**
- **Performance**: NGINX is generally faster than Apache in handling high-concurrency scenarios.
- **Memory Usage**: NGINX uses an event-driven model, which consumes less memory compared to Apache’s process-driven model.
- **Configuration**: NGINX is simpler for static content but may require more complex setups for dynamic content.

---

### **11. NGINX Advantages**
- **Scalability**: NGINX can handle thousands of simultaneous connections with ease.
- **Low Resource Consumption**: Uses less CPU and memory compared to other web servers.
- **Flexibility**: Can act as a web server, reverse proxy, load balancer, caching server, and more.
- **Security**: Offers SSL termination, rate limiting, and security filtering.

---

### **12. NGINX Limitations**
- **Complexity**: Some advanced configurations (e.g., handling dynamic content with PHP) can be more complex than other servers.
- **No Built-in .htaccess Support**: Unlike Apache, NGINX doesn’t support `.htaccess` files for directory-specific configurations.

---

NGINX is highly versatile, efficient, and widely used for modern web applications and high-traffic sites.

We need to Know about Docker, Ubuntu & NodeJS


Basic Code of Ngnix.conf

events{

}

http{

    server{
        listen 80;
        server_name _;

        location / {
            return 200 "Hello form Ngnix"
        }
    }

}


This is the basic code of Ngnix.conf file

Commands:-

sudo apt install nginx
nginx -v

nginx run on port 80

touch nginx.conf

events{                                                                                                                                                                                                          

}                                                                                                                                                                                                                                                                                                                                                                                                                                 http {

        server{

                listen 80;                                                                                                                                                                                                       server_name _;                                                                                                                                                                                                                                                                                                                                                                                                                    location / {
                        return 200 "Hi";
                }

        }

}  

sudo nginx -s reload

sudo nginx -t  ==> to check configurations is correct or not

Serve Static Code
events {
}

http {
    include /etc/nginx/mime.types;

    server {
        listen 80;
        server_name _;

        root /etc/nginx/website;
    }
}


Check the logs for any errors or issues during the renewal process
sudo cat /var/log/letsencrypt/letsencrypt.log 
if there is not any error then restart the NGINX .
sudo service nginx restart


# Node.js Deployment

> Steps to deploy a Node.js app to DigitalOcean using PM2, NGINX as a reverse proxy and an SSL from LetsEncrypt

## 1. Create Free AWS Account
Create free AWS Account at https://aws.amazon.com/

## 2. Create and Lauch an EC2 instance and SSH into machine
I would be creating a t2.medium ubuntu machine for this demo.

## 3. Install Node and NPM
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs

node --version
```

## 4. Clone your project from Github
```
git clone https://github.com/piyushgargdev-01/short-url-nodejs
```

## 5. Install dependencies and test app
```
sudo npm i pm2 -g
pm2 start index

# Other pm2 commands
pm2 show app
pm2 status
pm2 restart app
pm2 stop app
pm2 logs (Show log stream)
pm2 flush (Clear logs)

# To make sure app starts when reboot
pm2 startup ubuntu
```

## 6. Setup Firewall
```
sudo ufw enable
sudo ufw status
sudo ufw allow ssh (Port 22)
sudo ufw allow http (Port 80)
sudo ufw allow https (Port 443)
```

## 7. Install NGINX and configure
```
sudo apt install nginx

sudo nano /etc/nginx/sites-available/default
```
Add the following to the location part of the server block
```
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://localhost:8001; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
```
```
# Check NGINX config
sudo nginx -t

# Restart NGINX
sudo nginx -s reload
```

## 8. Add SSL with LetsEncrypt
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Only valid for 90 days, test the renewal process with
certbot renew --dry-run
```

