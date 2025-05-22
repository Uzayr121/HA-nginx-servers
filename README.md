High availability web server with NGINX and SSL

# Project Objective

**Networking Assignment v2** aims to build a highly available web setup using multiple backend servers, an NGINX reverse proxy, and SSL encryption.

Key goals:

- Set up domain and DNS with two subdomains: one for the app (`app.yourdomain.com`) and one for a status page (`status.yourdomain.com`).
- Deploy two backend VMs with simple web apps and a third VM as an NGINX reverse proxy/load balancer.
- Secure the app with SSL (using Let’s Encrypt) and enforce HTTPS.
- Create a status page showing backend health and active server.
- Optional: Make the reverse proxy highly available with failover.

**Outcomes:** Learn reverse proxy setup, SSL implementation, health checks, and cloud resilience.  

# Servers

We began by creating 5 ec2 instances, 2 will be our backend servers and 2 will be our reverse proxy load balancers(one main and one backup) and one will be the status page, and using user data we configure nginx with this script 
- user data script
```#!/bin/bash
sudo yum update -y
sudo yum install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
```

- for the security group we allow http, https, ssh 
- for the 2nd web server go to /usr/share/nginx/html/index.html and change the file contents so the web pages are different


# Reverse Proxy and Load balancer configuration

- in the main and backend we need to edit the /etc/nginx/nginx.conf
- for the load balancer we create an upstream block and add private IP's of our backend servers
``` upstream 'name of server' {
    server **private IP 1**;
    server **private IP 2**;
}
```
this will distribute traffic across the 2 backend servers

- Then we configure the reverse proxy section, for this we add
``` location / {
    proxy_pass http://'name of server'(name we use for upstream block);
}
```
- we do this in both reverse proxy servers
