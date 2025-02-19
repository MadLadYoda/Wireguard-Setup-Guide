# Wireguard-Setup-Guide
This guide is to act as a simple setup guide for Wireguard server utilising a VPS through a service provider that provides a public IPv4 address. Aswell as how to connect a Windows 11 Client to your Wireguard server. Noting security hardening is not included specifically within this guide.

1. Connect to your VPS utilising SSH such as `ssh USERNAME@SERVERIP` if you are using password authentication or if you are using key based authentication `ssh -i PRIVATEKEY USERNAME@SERVERIP`
2. Once you are logged into the VPS run `sudo apt update` once this is complete proceed to run `sudo apt upgrade`
3. Once completed proceed to install Wireguard utilising `sudo apt install wireguard`
