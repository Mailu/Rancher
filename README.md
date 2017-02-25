# Mailu Rancher Catalog
Rancher Catalog for [**Mailu.io**](https://mailu.io/) mail server.

## How to
* Just add `https://github.com/Mailu/Rancher.git` to Rancher custom catalog in "Admin -> Settings -> Catalog".
* You need to provide a `cert.pem` and `key.pem` files in `${ROOT}/certs`. For example, in `/mailu/certs/`.


## Using the "Let's Encrypt" stack from the Rancher Catalog

The **Let's Encrypt** stack from the Rancher Catalog uses: [`janeczku/rancher-letsencrypt`](https://hub.docker.com/r/janeczku/rancher-letsencrypt/).

That stack takes care of generating TLS / SSL certificates with Let's Encrypt, updating them and adding the certificates to your Rancher Certificate "store".

You can read the full updated documentation for that stack in the GitHub repository: https://github.com/janeczku/rancher-letsencrypt.

You need to pay special attention to the configuration for certificate renewal. You can configure it with several DNS providers or do it just using a Rancher Load Balancer and redirecting a specific route (`example.com/.well-known/acme-challenge`) to the Let's Encrypt stack service: https://github.com/janeczku/rancher-letsencrypt#http.

For Mailu you also need to have those certificates in a path in the host, for example in `/mailu/certs/`.

To use the **Let's Encrypt** stack to generate the certificates and use them inside Mailu, you can do the following:

**Note**: for this example let's assume that your domain is `example.com` and that the Mailu `${ROOT}` directory is at `/mailu/`. Update it as according to your configuration.

* Create the **Let's Encrypt** stack from the Rancher Catalog [following its documentation](https://github.com/janeczku/rancher-letsencrypt).
* Enter in the **Let's Encrypt** stack and click the "upgrade" button of the `letsencrypt` service.
* Go to the "Volumes" tab, it will have a Docker named volume as `letsencrypt:/etc/letsencrypt`.
* Update that named volume to be a host volume, for example: `/etc/letsencrypt-example.com:/etc/letsencrypt`.
* Now, after creating the certificates (or renovating them) you will have your certificates as: `fullchain.pem` and `privkey.pem`.
* Those certificates will be under the path: `/etc/letsencrypt-example.com/production/certs/example.com/`.
* Now, you have your certificates in your host in that path, but you need them inside the Mailu path, i.e. in: `/mailu/certs/cert.pem` and `/mailu/certs/key.pem`.
* To achieve that, go to that directory:

```bash
cd /mailu/certs/
```

* And there, create a link (a hard link, not a symbolic link) to those files, but with the names that you need to have inside (`key.pem` and `cert.pem`):

```bash
ln /etc/letsencrypt-example.com/production/certs/example.com/privkey.pem key.pem
ln /etc/letsencrypt-example.com/production/certs/example.com/fullchain.pem cert.pem
```

* Now you can start (or re-start) your Mailu Stack.

### Using `jwilder/nginx` and `JrCs/docker-letsencrypt-nginx-proxy-companion`

* If you need jwilder/nginx support and JrCs/docker-letsencrypt-nginx-proxy-companion, you need to manually symlink cert produce by JrCs/docker-letsencrypt-nginx-proxy-companion to `cert.pem` and `key.pem` in ${ROOT}/certs of your Mailu install.
