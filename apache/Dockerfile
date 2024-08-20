FROM ubuntu:latest
RUN apt update && apt install nginx -y
COPY default.conf /etc/nginx/sites-enabled/default
CMD ["nginx", "-g", "daemon off;"]
