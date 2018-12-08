# Nginx

sudo apt-get install nginx
sudo ufw app list
sudo ufw allow 'Nginx Full'

## SSL

cd /etc/nginx
mkdir ssl
cd ssl/ 
openssl dhparam -out dhparam.pem 4096

touch /etc/nginx/conf.d/ssl.conf

### ssl.conf

```
# I've used the configuration below for all my nginx instances and gotten an A+ on the Qualys SSL Test
# (https://www.ssllabs.com/ssltest/index.html). It satisfies requirements for PCI Compliance and
# FIPS. Includes OCSP Stapling (http://en.wikipedia.org/wiki/OCSP_stapling) and HTTP Strict Transport
# Security (http://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security).
# - Not vulnerable to the Heartbleed attack.
# - Not vulnerable to the OpenSSL CCS vulnerability (CVE-2014-0224) with OpenSSL v1.0.1i 6 Aug 2014 & Nginx 1.6.0
# - SSL Handshake takes <80ms on most modern server hardware

## Send header to tell the browser to prefer https to http traffic
#add_header Strict-Transport-Security max-age=31536000;

## Use TLS instead of SSL - Compatibility issues with some Java clients
## and older versions of of IE, however, more secure.
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

## Use more secure and less CPU tasking ciphers compared to nginx defaults
ssl_ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS;

## Improves TTFB by using a smaller SSL buffer than the nginx default
ssl_buffer_size 8k;

## Specifies that server ciphers should be preferred over client ciphers
ssl_prefer_server_ciphers on;

## Enables all nginx worker processes share SSL session information
ssl_session_cache shared:SSL:30m;

## Increases the amount of time SSL session information in the cache is valid
ssl_session_timeout 30m;

## Specifies a file with DH parameters for EDH ciphers
## Run "openssl dhparam -out /path/to/ssl/dhparam.pem 4096" in
## terminal to generate it
ssl_dhparam /etc/nginx/ssl/dhparam.pem;

## Enables OCSP stapling
ssl_stapling on;
resolver 8.8.8.8 8.8.4.4;
ssl_stapling_verify on;
```

-> disable all ssl configs in `nginx.conf`

# Let's encrypt
### Debian

```
vi /etc/apt/sources.list << echo 'deb http://ftp.debian.org/debian jessie-backports main'
sudo apt-get install certbot -t jessie-backports
```

### Ubuntu

```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
sudo certbot --nginx -d example.com -d www.example.com # optional
sudo certbot certonly
```

## Basic auth

```
sudo htpasswd -c /etc/nginx/.htpasswd si

```


## Monitoring

https://www.martin-helmich.de/de/blog/monitoring-nginx.html


listen {
  port = 4040
  address = "0.0.0.0"
}


namespace "smartdieselcheck" {
  format = "$remote_addr - $remote_user [$time_local] \"$request\" $status $body_bytes_sent \"$http_referer\" \"$http_user_agent\""
  source_files = [
    "/var/log/nginx/smart-diesel-check.access.log", "/var/log/nginx/smartdieselcheck.access.log"
  ]
  labels {
    app = "smartdieselcheck"
    environment = "production"
  }
}
```
```
[Unit]
Description=NGINX metrics exporter for Prometheus
After=network-online.target
 
[Service]
ExecStart=/usr/local/bin/prometheus-nginxlog-exporter -config-file /etc/prometheus-nginxlog-exporter.hcl
Restart=always
ProtectSystem=full
CapabilityBoundingSet=

[Install]
WantedBy=multi-user.target
```