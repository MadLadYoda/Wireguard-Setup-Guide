# Wireguard-Setup-Guide
This guide is to act as a simple setup guide for Wireguard server utilising a VPS through a service provider that provides a public IPv4 address. Aswell as how to connect a Windows 11 Client to your Wireguard server. Noting security hardening is not included specifically within this guide.

1. Connect to your VPS utilising SSH such as `ssh USERNAME@SERVERIP` if you are using password authentication or if you are using key based authentication `ssh -i PRIVATEKEY USERNAME@SERVERIP`.
2. Once you are logged into the VPS run `sudo apt update` once this is complete proceed to run `sudo apt upgrade`.
3. Once completed proceed to install Wireguard utilising `sudo apt install wireguard`.
4. Once installed enter sudo by utilising `sudo -i` then proceed to your Wireguard configuration folder utilising `cd /etc/wireguard`.
5. Created your Wireguard configuration folder utilising `nano wg0.conf`.
6. Generate a private & public key for your Wireguard server utilising the following commands `wg genkey | sudo tee /etc/wireguard/private.key` then `sudo chmod go= /etc/wireguard/private.key` and then finally generate your Public Key utilising `sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key`.
7. Fill in the details below utilising the format provided and the data generated.
```
[Interface]
PrivateKey = SERVERPRIVATEKEY
ListenPort = SERVERPORT
Address = 10.0.0.1/24
DNS = 9.9.9.9

[Peer]
PublicKey = CLIENTPUBLICKEY
AllowedIPs = 10.0.0.1/24
```
Brief Description of settings:
`PrivateKey`: Is the private key you generated in step 6.
`ListenPort`: Choose a port you would like to use as your port for Wireguard to listen on.
`PublicKey`: This will be your clients Public Key however due to it not being generated yet utilise your servers Public Key as a placeholder for the time being.
