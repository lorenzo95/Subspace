# Subspace - NOT THE OFFICIAL REPOSITORY !!!

   
   
Runs on Raspberry Pi - Do NOT expose this to the internet without reverse proxy, etc!   
   
## Remove the local DNS resolver. Dnsmasq will run inside the container   
systemctl disable systemd-resolved && reboot   
   
## Set DNS server.   
echo nameserver 1.1.1.1 >/etc/resolv.conf
   
## Load modules.   
modprobe wireguard   
modprobe iptable_nat   
modprobe ip6table_nat   
   
## Enable IP forwarding   
sysctl -w net.ipv4.ip_forward=1   
sysctl -w net.ipv6.conf.all.forwarding=1   
   
mkdir /data   
docker run -d \   
    --name subspace \   
    --network host \   
    --restart always \   
    --cap-add NET_ADMIN \   
    --volume /usr/bin/wg:/usr/bin/wg \   
    --volume /data:/data \   
    --env SUBSPACE_HTTP_HOST=IP/url of wireguard host in the config \   
    --env SUBSPACE_LETSENCRYPT=false \   
    --env SUBSPACE_HTTP_INSECURE=true \   
    --env NAMESERVER="9.9.9.9" \   
    netsecured/subspace:armv6   

