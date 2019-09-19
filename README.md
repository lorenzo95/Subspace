# Subspace - A simple WireGuard VPN server GUI

![Screenshot](https://raw.githubusercontent.com/subspacecloud/subspace/master/screenshot1.png?cachebust=8923409243)

## Screenshots

[Screenshot 1](https://raw.githubusercontent.com/subspacecloud/subspace/master/screenshot1.png)

[Screenshot 2](https://raw.githubusercontent.com/subspacecloud/subspace/master/screenshot2.png)

[Screenshot 3](https://raw.githubusercontent.com/subspacecloud/subspace/master/screenshot3.png)

[Screenshot 4](https://raw.githubusercontent.com/subspacecloud/subspace/master/screenshot4.png)

## Features

* **WireGuard VPN Protocol**
  * The most modern and fastest VPN protocol.
* **Single Sign-On (SSO) with SAML**
  * Support for SAML providers like G Suite and Okta.
* **Add Devices**
  * Connect from Mac OS X, Windows, Linux, Android, or iOS.
* **Remove Devices**
  * Removes client key and disconnects client.
* **Auto-generated Configs**
  * Each client gets a unique downloadable config file.
  * Generates a QR code for easy importing on iOS and Android.

## Run Subspace on Portal Cloud

Portal Cloud is a hosting service that enables anyone to run open source cloud applications.

[Sign up for Portal Cloud](https://portal.cloud/) and get $15 free credit with code **Portal15**.

# Run on Raspberry Pi   
   
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
    --env SUBSPACE_HTTP_HOST=75.155.76.169 \   
    --env SUBSPACE_LETSENCRYPT=false \   
    --env SUBSPACE_HTTP_INSECURE=true \   
    --env NAMESERVER="9.9.9.9" \   
    netsecured/subspace:armv6   

