---
title: "Blocking Russian IPs"
date: Thu, 06 Mar 2025 23:42:30 GMT
draft: false
type: posts
---
# Blocking Russian IPs





You might run some websites behind Nginx on Debian 12 and you might find yourself wanting to block requests from Russian IPs. It’s trivial. Here’s how to do it: # apt install libnginx-mod-http-geoip # echo "geoip_country /usr/share/GeoIP/GeoIP.dat;" &gt; /etc/nginx/conf.d/geoip.conf # cat &lt;&lt; EOF &gt; /etc/nginx/cc-block.conf if ($geoip_country_code3 = "RUS") { rewrite

You might run some websites behind Nginx on Debian 12 and you might find yourself wanting to block requests from Russian IPs. It’s trivial. Here’s how to do it:

```
# apt install libnginx-mod-http-geoip
# echo "geoip_country /usr/share/GeoIP/GeoIP.dat;" > /etc/nginx/conf.d/geoip.conf
# cat << EOF > /etc/nginx/cc-block.conf
if ($geoip_country_code3 = "RUS") {
    rewrite / /block-russia;
}

location /block-russia {
    default_type text/plain;
    return 200 'Russian IPs are blocked here, due to your unjust and unprovoked War against Ukraine.';
}
EOF
```

Now for each of the vhosts in /etc/conf.d/sites-enabled/ that you want to block access for, just add “include cc-block.conf;” inside the server block. Then of course, do an “nginx -t” to test your config and a “service nginx reload” to load the changes.

You can test what your website looks like from different IPs using [https://geotargetly.com/geo-browse](https://geotargetly.com/geo-browse)

#### [Source](https://www.grepular.com/Blocking_Russian_IPs)

