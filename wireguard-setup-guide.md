# Wireguard-Setup-Guide
This guide is to act as a simple setup guide for Wireguard server utilising a VPS through a service provider that provides a public IPv4 address. Aswell as how to connect a Windows 11 Client to your Wireguard server. Noting security hardening is not included specifically within this guide. This guide is utilising Ubuntu Server 24.04 LTS.

1. Connect to your VPS utilising SSH such as ```ssh USERNAME@SERVERIP``` if you are using password authentication or if you are using key based authentication ```ssh -i PRIVATEKEY USERNAME@SERVERIP```.
2. Once you are logged into the VPS run ```sudo apt update``` once this is complete proceed to run ```sudo apt upgrade```.
3. Once completed proceed to install Wireguard utilising ```sudo apt install wireguard```.
4. Once installed enter sudo by utilising ```sudo -i``` then proceed to your Wireguard configuration folder utilising ```cd /etc/wireguard```.
5. Created your Wireguard configuration folder utilising ```nano wg0.conf```.
6. Generate a private & public key for your Wireguard server utilising the following commands ```wg genkey | sudo tee /etc/wireguard/private.key``` then ```sudo chmod go= /etc/wireguard/private.key``` and then finally generate your Public Key utilising ```sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key```.
7. Fill in the details below utilising the format provided and the data generated.
```
[Interface]
PrivateKey = SERVERPRIVATEKEY
ListenPort = SERVERPORT
Address = 10.0.0.1/24
DNS = 9.9.9.9

[Peer]
PublicKey = CLIENTPUBLICKEY
AllowedIPs = 10.0.0.1/32
```
Brief Description of settings:
- `PrivateKey`: Is the private key you generated in step 6 fopr your server.
- `ListenPort`: Choose a port you would like to use as your port for Wireguard to listen on.
- `PublicKey`: This will be your clients Public Key however due to it not being generated yet utilise your servers Public Key as a placeholder for the time being.
- `Address`: The private IP network range address you would like to utilise. 10.0.0.0 to 10.255.255.255, 172.16.0.0 to 172.31.255.255, 192.168.0.0 to 192.168.255.255.
- `DNS`: The DNS server you would like to utilise.
- `AllowedIPs` The allowed IPs that are able to connect utilising that peer configuration.

8. Once this is completed you will need to allow port forwarding on your server to do this perform the command ```sudo nano /etc/sysctl.conf``` then unhash both ```net.ipv4.ip_forward=1``` & ```net.ipv4.conf.all.forwarding=1``` do not have a hashtag at the beginning of the lines then save & exit nano.
9. Once this is done you will need to enable Masquerading for your primary interface utilising IPTables to allow your internal private network to be able to reach the outside world. To do this simple type ```ip addr``` and note down your primary network interface for this example we will use ```ens3``` then proceed to type the following command ```sudo iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE```
10. To make these changes to IPTables become persistent type the following command ```sudo iptables-save | sudo tee /etc/iptables.conf``` and it should display your recently created IPTables rule.
11. Now install Wireguard for your Windows 11 client @ https://www.wireguard.com/install/
12. Once installed open up Wireguard click the little down arrow and click ```Add Empty Tunnel``` you should now see a popup with a new Public Key, Private Key and requesting you to input the name of the file.
13. Choose a name for the file and then fill in the details below utilising the provided format. Noting you will need to copy that Public Key we created earlier on the Server into this configuration file.
```
[Interface]
PrivateKey = CLIENTPRIVATEKEY
ListenPort = CLIENTPORT
Address = 10.0.0.1/24
DNS = 9.9.9.9

[Peer]
PublicKey = SERVERPUBLICKEY
AllowedIPs = 0.0.0.0/0
Endpoint = SERVERIP:SERVERWIREGUARDPORT
```
Brief Description of settings:
- `Endpoint`: Is simply your server IPv4 Address and the port you allocated for Wireguard
- `AllowedIPs` In this instance it blocks all outbound traffic forcing it through your Wireguard tunnel.

14. Once this is done copy the Public Key generated in your Windows Wireguard Client and then save the configuration.
15. Go back to the server and type 
```sudo nano /etc/wireguard/wg0.conf``` 
and then where it says:
```
[Peer]
PublicKey = SERVERPUBLICKEY
```
Replace the server Public Key you utilised earlier as a placeholder with your Client Public Key and then save & exit nano.

16. To ensure Wireguard comes up on restarts on your server type ```sudo systemctl enable wg-quick@wg0``` and then to bring your Wireguard instance up on your server type ```sudo wg-quick up wg0```
17. To turn your Wireguard tunnel on for your client simply click activate on the client.

You should be done and have successfully created a tunnel between your home computer to your Wireguard server and allowed access to the internet through this tunnel routing through your VPS instance.




