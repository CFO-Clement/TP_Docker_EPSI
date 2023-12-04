# Base image: nginx version 1.12
FROM nginx:1.12

# Copy default.conf into: /etc/nginx/conf.d/default.conf
COPY default.conf /etc/nginx/conf.d/default.conf

# HTML file for BACKEND-NOT-FOUND responses: copy into /var/www/html/backend-not-found.html
COPY backend-not-found.html /var/www/html/backend-not-found.html

# Proxy configuration: copy proxy.conf into /etc/nginx/includes/
COPY proxy.conf /etc/nginx/includes/

#Path for mac M1
COPY nginx.conf /etc/nginx/nginx.conf