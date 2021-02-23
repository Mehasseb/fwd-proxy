# forward-proxy-icap-docker 

A forward proxy is an Internet-facing proxy used to retrieve data from a wide range of sources (in most cases anywhere on the Internet).
A forward proxy provides proxy services to a client or a group of clients. ... But then when the forward proxy receives the response, it recognizes it as a response to the request that went through earlier. And so it in turn sends that response to the client that made the request.

 The completed pieces of this project so far are:
- Being Docker based.
- Support basic authentication static username/password.
- Support SSL pumping. (Broken)
- Support ICAP integration for the related file types





### Preparing environment:

```bash
sudo apt-get update && sudo apt-get install curl git -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo usermod -aG docker $( whoami )
```

You will have to logout and relogin from your machine before deploying the solution

## Preparing source code

1. Clone repository

```bash
git clone --recursive https://github.com/k8-proxy/fwd-proxy

   ```
## Deployment

. Execute the following
   
   ```bash
   docker-compose build --no-cache
   docker-compose up
   ```
   Verify that all containers are up
   
   ```bash
   docker-compose ps
   ```
- To enable user authentication
execute the following commands
```bash
   sudo apt-get install apache2-utils
   htpasswd -c <path of squid users file on squid directory> <username>
   "you will be asked to enter the password for the user"
   chown squid <path of squid users files on squid directory>
  
   Add the following to squid.conf file
   
   auth_param basic program /usr/lib64/squid/basic_ncsa_auth <ath of squid users file on squid directory>
   auth_param basic children 5
   auth_param basic realm Proxy Authentication Required
   auth_param basic credentialsttl 2 hours
   auth_param basic casesensitive off
 ```
   for more details check the following link  https://kifarunix.com/how-to-setup-squid-proxy-basic-authentication-with-username-and-password/ 
   
   -To enable SSL pumping 
   Go to Squid folder and execute the following Commands
   
   ```bash
  openssl req -new -newkey rsa:2048 -days <certificate validity period in days> -nodes -x509 -keyout squidCA.pem -out squidCA.pem
  
"You will be prompted to fill in the fields of the self-signed SSL certificate"

openssl x509 -in squidCA.pem -outform DER -out squid.der
  ```
  for more details you can check https://support.kaspersky.com/KWTS/6.0/en-US/166244.htm

   ## Troubleshooting

- Check if docker service is active
  
  ```bash
  systemctl status docker
  ```

- Check if containers are up and running (not Restarting...)
  
  ```bash
  docker-compose ps
  ```

- If squidis not started correctly
  ```bash
  docker-compose up -d --force-recreate
  ```
  Now Get your machine IP Address and Open your web browser
  Go to setting>proxy>manualproxy
  Add IP Adress of your machine and port 3128 then pree ok and restart your browser
  ![auth](https://user-images.githubusercontent.com/75560486/108697290-57057e80-750b-11eb-8d68-ad7756c685b5.PNG)
  You will be asked to enter a username and password use helmy as a username and 123 as password once you pass authentication you will be able to use the web browser

