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
git clone --recursive https://github.com/Helmy1998/fwd-proxy

   ```
## Deployment

2. Execute the following
   
   ```bash
   docker-compose build --no-cache
   docker-compose up
   ```
   Verify that all containers are up
   
   ```bash
   docker-compose ps
   ```
   
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

