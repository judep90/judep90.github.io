# ivre on truenas
## mongodb
truenas has an "app" for mongo. easy setup.

## ivre image
not sure which image to use, will build one.
```dockerfile
FROM ubuntu:24.10
RUN apt install -y python3.11
RUN pip install bottle cryptography pymongo pillow pyOpenSSL
```