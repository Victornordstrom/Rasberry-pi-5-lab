# Rasberry-pi-5-lab
In this Project i show how i configured my homelab using my rasberry pi 5 to selfhost services using docker compose containers.


## 🐳 Docker Services

<details>
  
<summary>📄 View Full <code>docker-compose.yml</code></summary>
```yaml
services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: unless-stopped
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data

  pihole:
    image: pihole/pihole:latest
    container_name: pihole
    restart: unless-stopped
    network_mode: host
    environment:
      - WEBPASSWORD=changeme
      - TZ=Europe/London
      - DNSMASQ_LISTENING=all
    volumes:
      - /mnt/nvme/pihole/etc:/etc/pihole
      - /mnt/nvme/pihole/dnsmasq:/etc/dnsmasq.d

  nginx-proxy-manager:
    image: jc21/nginx-proxy-manager:latest
    container_name: nginx-proxy-manager
    restart: unless-stopped
    ports:
      - "81:81"
      - "8443:443"
    volumes:
      - /mnt/nvme/npm/data:/data
      - /mnt/nvme/npm/letsencrypt:/etc/letsencrypt

  uptime-kuma:
    image: louislam/uptime-kuma:latest
    container_name: uptime-kuma
    restart: unless-stopped
    ports:
      - "3001:3001"
    volumes:
      - /mnt/nvme/uptime-kuma:/app/data

  nextcloud:
    image: nextcloud:28.0.14-apache
    container_name: nextcloud
    restart: unless-stopped
    ports:
      - "8080:80"
    environment:
      - MYSQL_HOST=mariadb
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=changeme
    volumes:
      - /mnt/nvme/nextcloud:/var/www/html/data
    depends_on:
      - mariadb

  mariadb:
    image: mariadb:latest
    container_name: mariadb
    restart: unless-stopped
    environment:
      - MYSQL_ROOT_PASSWORD=changeme
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=changeme
    volumes:
      - /mnt/nvme/databases/mariadb:/var/lib/mysql

  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      - N8N_SECURE_COOKIE=false
    volumes:
      - /mnt/nvme/n8n:/home/node/.n8n

  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    ports:
      - "8081:80"
    environment:
      - DOMAIN=https://192.168.1.10
    volumes:
      - /mnt/nvme/vaultwarden:/data

  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    restart: unless-stopped
    ports:
      - "8096:8096"
    volumes:
      - /mnt/nvme/jellyfin:/config
      - /mnt/nvme/jellyfin/media:/media

volumes:
  portainer_data:
```

</details>
