# EWF RPC node install script

## Quickstart

1. Install operating system and prepare dedicated host for RPC node

    At least we recommend to run AWS EC2 instance with minimum resources like `t3.medium` or gcloud `e2-medium`\
    Basic required instance specification:
      - OS: Ubuntu Server 18.04
      - CPU: 2
      - RAM: 4GB
      - Hard drive: minimum 60GB at this moment

    Network security recommendations:
      - Incoming SSH connection only from specific IPS or via VPN.
      - Incoming HTTP/HTTPS connections via ports 80/443 (Can be limitated to specific locations depending on your needs)
      - Open outgoing connection to the internet. Node should be regulary updated and have to be connected to the blockchain network.

2. Connect to the host and download the installation script:
    
    `wget -O install-rpc.bash https://raw.githubusercontent.com/dathbezumniy/ewc-rpc-node/main/install-rpc-ubuntu-server-18.04-ewc.bash`

3. Run installation script with selected option:
      
    - In this configuration we receive ready to use RPC with connection provided via HTTP on port 80:\
      `sudo bash install-rpc.bash install-http`

    - In this configuration we receive RPC with self-signed certificate which is not allowed by most browsers:\
     `sudo bash install-rpc.bash install-https` Afer the installation user is obligated to provide his own certificates.

4. I have added additional instructions to the script in order to download db-backup.\
    If for some reason script does not download and sets everything up automatically,
    please refer here for manual https://energyweb.atlassian.net/wiki/spaces/EWF/pages/1013153839/How+to+use+ready-to-go+chain+backup or ping me on discord.

5. Now we have to wait till validator will be fully synced

    In order to check progress you can use `docker-compose logs -f`

## HTTPS Certificate Informations

In situation when we chose HTTPS option our script will generate simple self-signed certificate which should be changed.

Certificates are dynamically binded to nginx container based on path inside `docker-stack/.env` file. We have two variables `NGINX_CERT` and `NGINX_KEY` where we can provide path to new certificates.

Members can use any trusted ssl certificates. For our composition does not matter if any certbot will provide files or another service - they just must be readable for nginx.

After certificate change nginx container restart is required, so please run below command from `docker-stack` directory:

`docker-compose restart web`

Additionally verify if nginx started correctly and we do not have any errors related to that certificates in logs

`docker-compose logs --tail 50 web`
