# Rasberry-pi-5-lab
In this Project i show how i configured my pihole DNS - Adblocker using my rasberry pi 5 to selfhost the services using docker compose container.

#   pihole:
    image: pihole/pihole:latest
    container_name: pihole
    restart: unless-stopped
    network_mode: host
    environment:
      - WEBPASSWORD=********
      - TZ=Europe/London
      - DNSMASQ_LISTENING=all
    volumes:
      - /mnt/nvme/pihole/etc:/etc/pihole
      - /mnt/nvme/pihole/dnsmasq:/etc/dnsmasq.d


 Pihole Web-ui

<img width="1910" height="916" alt="Screenshot From 2026-03-23 17-39-36" src="https://github.com/user-attachments/assets/d26227cf-4a7d-44ad-bc38-a8d1a9fb94bb" />







