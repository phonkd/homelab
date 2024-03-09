***

## Putting everything behind pfsense

1. docs tbd

## DNS BIIIG Problem

- i wanted to expose only my nextcloud publicly so i created a dns record for nextcloud and did a port forward to my router. Its path looks like this: ![[Pasted image 20240306002055.png]]
	- For this i exposed and forwarded port 443 directly to my traefik
	- You cant access other apps because the dns record doesnt exist

>[!error] What if the client makes an entry in the host file?
>If a client enters a desired hostname with my public ip as counterpart he can **access any service** behind traefik, since traefik just looks for the host header and doesnt care how the DNS name was resolved.
>After testing this and confirming it i knew something had to be done.

>[!success] Solution: **ip-whitelist** traefik middleware
>This middleware upon accessing checks if your ip is in the allowed ranges list and gives you a forbidden if not.
>To add this middleware edit labels of the service (e.g jellyfin):
>```bash
>...
>labels:
>	      - traefik.enable=true
>	      - traefik.http.routers.jf.rule=Host(`jf.docker.phonkd.net`)
>	      - traefik.http.routers.jf.entryPoints=https$
>	      - traefik.http.routers.jf.tls=true
>	      - traefik.http.routers.jf.tls.certresolver=cloudflare
>	      - traefik.http.services.jf.loadBalancer.server.port=8096
>	      - traefik.http.routers.jf.middlewares=ip-whitelist@docker
>	      - traefik.http.middlewares.ip-whitelist.ipwhitelist.sourcerange=127.0.0.1/32, 192.168.1.0/24, 92.106.79.2
>```
>The **relevent part** is in the last 2 lines.
>>[!tip] Allow your public ip because when accessing from lan sometimes it still uses your public ip.
>
>**updated flow:**
>![[Pasted image 20240306003204.png]]



