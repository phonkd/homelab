created cronjob for it:
https://github.com/troglobit/inadyn
```bash
0 07,12 * * * docker run --rm -v "/data/ddns/inadyn.conf:/etc/inadyn.conf" -v "/data/ddns/cache:/var/cache/inadyn" troglobit/inadyn:latest -1 --cache-dir=/var/cache/inadyn > /dev/null 2>&1
```
file is in /data/ddns/inadyn.conf
