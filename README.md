# x-ui-automation
Installs and configures [FranzKafkaYu/x-ui](https://github.com/FranzKafkaYu/x-ui) with 15 CDN and non CDN connections.

Tested with FranzKafkaYu/x-ui version `0.3.4.0`.

## Requirements
To use this script you need:
- A server with Ubuntu 18.04 or later.
- One subdomain with Proxy off for panel login and non CDN connections.
- One subdomain with Proxy on for CDN connections.

## Parameters
```
-u|--username       User to use with x-ui panel installation (optional, default = admin).
-p|--password       Password to use with x-ui panel installation (optional, default = admin).
-o|--port           Port to use with x-ui panel installation (optional, default = 54321).
-w|--webpath        A random 4 letter phrase to customize x-ui default login URL.
-e|--email          Email to use when getting SSL with Let's encrypt.
-d|--domain-direct  Main (sub)domain on Cloudflare with proxy CDN OFF.
-c|--domain-cdn     Secondary (sub)domain on Cloudflare with proxy CDN ON.
-t|--timezone       Timezone to use with x-ui panel installation (optional, default = Asia/Tehran).
--pre               One word name of this server for organizational porpuse (no spaces).
                        This word will be added to the begining of the connection name.
```

## Steps
- Get a new VPS at any VPS provider like [Vultr](https://www.vultr.com/?ref=7127449) or [DigitalOcean](https://www.digitalocean.com/?refcode=e6ae46244d85).
- Get a domain form a domain registerar like [NameCheap](https://namecheap.com). We'll use `domain.com` in this example.
- Signup for [Cloudflare](https://cloudflare.com).
- Add your domain to Cloudflare.
- Change your domain's DNS to Cloudflare DNS.
- Add a subdomain in Cloudflare with proxy off. We'll use `srv1d.domain.com` in this example.
- Add a second subdomain in Cloudflare with proxy on. We'll use `srv1c.domain.com` in this example.
- Wait until you can ping the first subdomain (the one that has proxy turned off). In this example we need to ping `srv1d.domain.com`.
    ```
    ping srv1d.domain.com
    ```
- Copy the command below to a text editor like Notepad and change the parameters.
    ```
    bash <(curl -Ls https://raw.githubusercontent.com/leomoon/x-ui-automation/master/xui) --username admin --password admin --port 46321 --email email@domain.com --webpath panel --domain-direct srv1d.domain.com --domain-cdn srv1c.domain.com --pre srv1
    ```
- SSH to your VPS server.
- Paste the modified command and press enter.

### The command above will:
- Update and install required packages.
- Issue certificate for both subdomains using acme.
- Install and configure x-ui with defined user, pass, port, x-ui custom login path, and timezone.
- Create 16 most optimized connections to bypass censorship.

## Connection types
- vmess-ws-cdn-2082
- vmess-ws-tls-cdn-2083
- vmess-ws-tls-randomPort
- vless-ws-cdn-2086
- vless-ws-tls-cdn-2087
- vless-ws-tls-randomPort
- vless-tcp-tls-randomPort
- vless-grpc-tls-cdn-2053
- vless-grpc-tls-randomport
- trojan-ws-tls-cdn-2096
- trojan-ws-tls-randomPort
- trojan-tcp-tls-randomPort
- trojan-grpc-tls-cdn-8443
- trojan-grpc-tls-randomport

## Can I run this script again?
Yes, this will uninstall x-ui, install a fresh new x-ui and create new connections (with new UUIDs).
