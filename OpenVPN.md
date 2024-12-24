
# Installing Packages
      apt install iptables-persistent
      apt install tcpdump 
      apt install easy-rsa openvpn
      apt install fail2ban 
      apt install tmux

* * *
# Configuring Fail2ban
     cd /etc/fail2ban/
   
     vi  jail.conf 
     fail2ban-client status
     systemctl status fail2ban
     systemctl restart fail2ban.service 
     systemctl status fail2ban

* * *
#OpenVPN configuring


     make-cadir /etc/easy-rsa
     cd /etc/easy-rsa/
     ./easyrsa init-pki
     ./easyrsa build-ca nopass
     ./easyrsa gen-dh
     ./easyrsa gen-crl 

  ./easyrsa gen-req oserver nopass
    ./easyrsa sign-req server  oserver 
    ./easyrsa gen-req client1 nopass
    ./easyrsa sign-req client client1


* * * 

# Configuring sysctl 
sed -i "$ a net.ipv6.conf.all.disable_ipv6 = 1\\
   net.ipv6.conf.default.disable_ipv6 = 1\\
     net.ipv6.conf.lo.disable_ipv6 = 1 \\
   net.ipv4.ip_forward=1\\
" /etc/sysctl.conf

* * *
# IPTABLES 
	iptables -L 
	iptables -A INPUT -i lo -j ACCEPT
	iptables -A INPUT -s 127.0.0.0/8 -j ACCEPT
	iptables -A INPUT -p tcp  -m conntrack --ctstate ESTABLISHED -j ACCEPT
	iptables -A INPUT -p udp  -m conntrack --ctstate ESTABLISHED -j ACCEPT
	iptables -L -v 
	iptables -A INPUT -p tcp  --dport 22 -m conntrack --ctstate NEW -j ACCEPT
	iptables -A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW -j ACCEPT

	iptables -t nat -A POSTROUTING -s X.X.X/16  -j MASQUERADE

	iptables -L -v

	netfilter-persistent save

* * *
# Enabling and Starting OpenVPN service 
	systemctl enable  openvpn-server@openvpn.service
	systemctl start   openvpn-server@openvpn.service
	systemctl status   openvpn-server@openvpn.service

* * * 
#Server output 
	cat /etc/openvpn/openvpn.conf

```
ca /etc/easy-rsa/pki/ca.crt
cert /etc/easy-rsa/pki/issued/oserver.crt
key /etc/easy-rsa/pki/private/oserver.key
dh /etc/easy-rsa/pki/dh.pem
proto tcp
port 443
dev tun
duplicate-cn
keepalive 10 60
server 10.20.12.0 255.255.255.0
#tun-mtu 1440
#mssfix 1400
verb 3
push "redirect-gateway def1"

```
* * * 
#Client count 

   cat ovpn
```
client

dev tun
proto tcp
remote  B.B.B.B  443
<ca>
</ca>
<cert>
</cert>
<key>
</key>
```
