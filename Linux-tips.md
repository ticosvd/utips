### Checking ports

nc -zv IP_address PORTS

######### Certificates show #########	
KEYS : 
openssl rsa -noout -text -check -in nginx.key

CERT: 
  openssl x509 --noout -text -in nginx.crt 



########## curl with time ##############
#
curl  -v  http://XXXX  -w "\nSize_download : %{speed_download} \n Time: %{time_total} \n Local Port : %{local_port} \n"



### Ubuntu 20.10 install Pulse ##############

https://community.pulsesecure.net/t5/Pulse-Desktop-Clients/pulseUi-doesn-t-work-in-ubuntu-20-04/td-p/42721

wget http://webdev.web3.technion.ac.il/docs/cis/public/ssl-vpn/ps-pulse-ubuntu-debian.deb


################ GENERATE CERTS and NGINX configuration

GENERATE KEY and certificate for CA:

openssl genrsa -des3 -out ca.key 4096

openssl req -new -x509 -days 365 -key ca.key -out ca.crt

#
GENERATE USER KEY and Cert Request:

openssl genrsa -des3 -out user.key 4096

REGENERATE KEY WIHTOUT PASSWORD:
openssl rsa -in server.key -out server.key



openssl req -new -key user.key -out user.csr

#
SIGNED THE CSR:
openssl x509 -req -days 365 -in user.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out user.crt
#
server {
    listen 443 ssl;
    server_name example.com;

    ssl_protocols TLSv1.1 TLSv1.2;
    # letsencrypt certificate
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # client certificate
    ssl_client_certificate /etc/nginx/client_certs/ca.crt;
    ssl_verify_client  on;


###################

show arp dump in Linux:
cat /proc/net/arp


##############

extract multiple files from tar with directory : 
for f in *.tgz; do tar zxvf "$f"  --one-top-level; done

 for i in {1..20}; do   touch -a -m -t  202201171005.09 tgs$i.txt ; done

##########################
The linux kernel has a feature called AnyIP which allows you to answer for a contiguous block of IPv4 or IPv6 addresses via your linux loopback interface for very little cost in DRAM/CPU.

For instance, assume I want my linux machine to answer for any address in 10.7.0.0/16:

    On the linux system add a local route: ip -4 route add local 10.7.0.0/16 dev lo
    Ask your network engineers to advertise a route for 10.7.0.0/16 pointing to the eth0 address of the machine you did this with.

##########
Copy files with wildcard to ../1 :
ls -la | grep "1111\|2222\|3333\|4444" | cut -d " " -f10- | xargs cp -t ../1/

#####################
Create the file with set timestamp:
 touch -a -m -t 203801181205.09 tgs.txt
 
 ######################
 Delete files if timestamps > 1day :
   
 find /path/to/directory/ -mindepth 1 -mtime +1 -delete
 Testing find /path/to/directory/ -mindepth 1 -mtime +1 -print
 or find /path/to/directory/ -mindepth 1 -mmin +1440 -print
  find /var/opt/radware/license/logs   -type f -iname "flexnet*"   -mmin +1440   -delete
 
################
Daemon accounts-daemon makes HIGH CPU Loading:

sudo systemctl stop accounts-daemon.service 
 
########################
Changing configuration of proxy in Firefox

cd cd ~/.mozilla/firefox/*Default*

cat prefs.js | grep proxy
user_pref("network.proxy.socks", "localhost");
user_pref("network.proxy.socks_port", 10001);
user_pref("network.proxy.type", 1);

Another options : 
cd ~/.mozilla/firefox/*Default*; cat  prefs.js

cd ~/.mozilla/firefox/*Default*; sed -i s/.*network.proxy.type.*/'user_pref("network.proxy.type", 1);'/ prefs.js

But it doesn't work currently :(


#####################################
Utils for testing:

slowhttptest
hping3 


##################
Changing crt.file with headers:
sed -i 's/^-----BEGIN CERTIFICATE-----/&\n/ ;  s/-----END CERTIFICATE-----/\n&/' tt3.crt
####

###################
fzf 

Fuzzy finding
##########
