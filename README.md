# WireGuard installer

![Lint](https://github.com/angristan/wireguard-install/workflows/Lint/badge.svg)
![visitors](https://visitor-badge.glitch.me/badge?page_id=angristan.wireguard-install)

**This project is a bash script that aims to setup a [WireGuard](https://www.wireguard.com/) VPN on a Linux server, as easily as possible!**

WireGuard is a point-to-point VPN that can be used in different ways. Here, we mean a VPN as in: the client will forward all its traffic trough an encrypted tunnel to the server.
The server will apply NAT to the client's traffic so it will appear as if the client is browsing the web with the server's IP.

The script supports both IPv4 and IPv6. Please check the [issues](https://github.com/angristan/wireguard-install/issues) for ongoing development, bugs and planned features!

WireGuard does not fit your environment? Check out [openvpn-install](https://github.com/angristan/openvpn-install).

## Requirements

Supported distributions:

- Ubuntu >= 16.04
- Debian 10
- Fedora
- CentOS
- Arch Linux

## Usage

Download and execute the script. Answer the questions asked by the script and it will take care of the rest.

```bash
curl -O https://raw.githubusercontent.com/angristan/wireguard-install/master/wireguard-install.sh
chmod +x wireguard-install.sh
./wireguard-install.sh
```

It will install WireGuard (kernel module and tools) on the server, configure it, create a systemd service and a client configuration file.

Run the script again to add or remove clients!

## Listenport 53

Free up the port 53. Local DNS resolver listens on it. Follow this guide:

https://medium.com/@niktrix/getting-rid-of-systemd-resolved-consuming-port-53-605f0234f32f
https://www.reddit.com/r/WireGuard/comments/ilz0qm/cant_get_wg_to_work_on_port_53/

## Make wireguard listen on multiple ports

Use this as an example:

`iptables -t nat -I PREROUTING -i eth0 -d <yourIP/32> -p udp -m multiport --dports 53,80,4444  -j REDIRECT --to-ports 15351`
Change the interface to ens3 if needed, add allowed IP list in CIDR format.

To make this rule permanent, run 

```
sudo netfilter-persistent save
sudo netfilter-persistent reload
```

## Providers

I recommend these cheap cloud providers for your VPN server:

- [Vultr](https://goo.gl/Xyd1Sc): Worldwide locations, IPv6 support, starting at \$3.50/month
- [Hetzner](https://hetzner.cloud/?ref=ywtlvZsjgeDq): Germany and Finland, IPv6, 20 TB of traffic, starting at €3/month
- [Digital Ocean](https://goo.gl/qXrNLK): Worldwide locations, IPv6 support, starting at \$5/month
- [PulseHeberg](https://goo.gl/76yqW5): France, unlimited bandwidth, starting at €3/month
