# Rancher-Mailu
Rancher Catalog for Mailu.io mail server.

#How-to ?
* Just add `https://github.com/Mailu/Rancher.git` to Rancher custom catalog.
* If you need jwilder/nginx support and JrCs/docker-letsencrypt-nginx-proxy-companion, you need to manually symlink cert produce by JrCs/docker-letsencrypt-nginx-proxy-companion to priv.key and cert.pem in ${ROOT}/certs of your Mailu install.
