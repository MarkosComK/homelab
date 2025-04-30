FROM alpine:3.18

RUN apk add --no-cache \
    nginx \
    bash \
    vim \
    curl \
    wget \
    htop

RUN mkdir -p /run/nginx

# Copy custom nginx configuration
#COPY config/nginx.conf /etc/nginx/nginx.conf

# Create web root directory
RUN mkdir -p /usr/share/nginx/html

# Copy website files
COPY html/ /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

# Set nginx as entrypoint to ensure it runs as PID 1
ENTRYPOINT ["nginx", "-g", "daemon off;"]
