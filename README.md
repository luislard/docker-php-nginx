# How to set the enviroment

1. Clone this repo
2. cd to folder
3. Clone symfony proyect into symfony folder.
4. Modify your configuration in *.env* file. Remove `.dist` on *.env.dist* file and configure it as your needs.
5. Build containers with:

6. Build Containers

```bash
docker-compose build
```

7. Start your containers

```bash
docker-compose up -d
```

# How to set the app

1. Create or copy a key pair to be used for *JWT* generation.

```bash
cp /path/to/your/private.pem ./symfony/var/jwt/private.pem
cp /path/to/your/public.pem ./symfony/var/jwt/public.pem
```


If you need to create a key pair:
Generate the SSH keys :

``` bash
$ mkdir -p config/jwt # For Symfony3+, no need of the -p option
$ openssl genrsa -out config/jwt/private.pem -aes256 4096
$ openssl rsa -pubout -in config/jwt/private.pem -out config/jwt/public.pem
```

In case first ```openssl``` command forces you to input password use following to get the private key decrypted
``` bash
$ openssl rsa -in config/jwt/private.pem -out config/jwt/private2.pem
$ mv config/jwt/private.pem config/jwt/private.pem-back
$ mv config/jwt/private2.pem config/jwt/private.pem
```

2. Install Dependencies

```bash
docker-compose exec -u $(whoami) php bash
# you should be inside the ocntainer logged with your user
composer install
```

3. Create user

```bash
# Inside the container run 
php bin/console fos:user:create adminuser mail@example.com adminuser
```

4. Fix Symfony permissions (*Linux*)

```bash
# Asuming you are in Linux and Inside the Symfony folder in your host
sudo setfacl -dR -m u:www-data:rwX -m u:$(whoami):rwX var
sudo setfacl -R -m u:www-data:rwX -m u:$(whoami):rwX var
```