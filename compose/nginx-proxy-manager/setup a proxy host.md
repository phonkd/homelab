***


>[!error] Problem
>When multiple deployments with webUI's exist, they both want port 80.
>One way to solve this is to assign another port which will have to be manually entered whenever accessing the website.
>Or with nginx-proxy-manager you could let it handle that.
>By creating a host, the source subdomain will decide to which endpoint you are directed.




## howto create an (ingress?)

1. Create a dns record for a subdomain (CNAME): ![](Pasted%20image%2020231129173214.png)
2. Change the port of the service to e.g 8888 ![](Pasted%20image%2020231129173238.png)
3. Enter the earlyer created `CNAME` in Domain name
4. enter the endpoint ip in hostname and its port in forwar port 
5. ![](Pasted%20image%2020231129173437.png)
6. test

>[!tip] To use hostnames in the `Forward Hostname /IP*` field, adjusting the dns server might be neccesary:
>See [docker-compose](svcs/portainer/nginx-proxy-manager/docker-compose.yml) ![](Pasted%20image%2020231129194159.png)

## adding a path e.g `/admin`

1. edit your desired proxy host
2. under advanced enter `location = /{return 301 $scheme://$http_host/thepathyouwant;}` ![](Pasted%20image%2020231129173834.png)
3. save & test