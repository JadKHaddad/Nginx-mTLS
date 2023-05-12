Get yourself a root CA certificate.
```sh
step ca root certs/root.crt --ca-url <ca-url> --fingerprint <fingerprint>
```
Create the server certificate. ```server-reverse-proxy``` is the hostname of the server and will be used in ```client-nginx.conf``` to verify the server certificate.
```sh
step ca certificate "server-reverse-proxy" certs/server.crt certs/server.key --ca-url <ca-url> --root certs/root.crt   
```
Create the client certificate.
```sh
step ca certificate "client" certs/client.pem certs/client.key --ca-url <ca-url> --root certs/root.crt
```
```sh
docker network create somenetwork
```
```sh
docker build -t server -f Dockerfile.server .
```
```sh
docker run -it --rm --name server --network somenetwork server
```
```sh
docker build -t server-nginx -f Dockerfile.server-nginx .
```
```sh
docker run -it --rm --name server-nginx --network somenetwork server-nginx
```
```sh
docker build -t client-nginx -f Dockerfile.client-nginx .
```
```sh
docker run -it --rm --name client-nginx --network somenetwork client-nginx
```
```sh
docker build -t client -f Dockerfile.client .
```
```sh
docker run -it --rm --name client --network somenetwork client
```
```sh
curl http://client-nginx
```
![img](https://github.com/JadKHaddad/Nginx-mTLS/blob/main/assets/image.jpg?raw=true)