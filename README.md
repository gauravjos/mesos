# mesos

#####Mesos-DNS

Scripts for setting up

sudo mkdir /etc/mesos-dns
sudo vi /etc/mesos-dns/config.json
config.json

#### replace zk:/10.16.67.152 with IP of your mesos master
 {
  "zk": "zk://10.16.67.152:2181/mesos",
  "refreshSeconds": 60,
  "ttl": 60,
  "domain": "mesos",
  "port": 53,
  "resolvers": ["172.27.172.10","172.27.172.12"],
  "timeout": 5,
  "email": "root.mesos-dns.mesos"
 }

sudo docker run -d --net=host -v "/etc/mesos-dns/config.json:/config.json" -v "/var/mesos-dns/logs:/tmp" mesosphere/mesos-dns:0.5.2 /usr/bin/mesos-dns -v=2 -config=/config.json


sudo sed -i "1s/^/nameserver $(hostname -i)\n/" /etc/resolv.conf
sudo sed -i "1s/^/prepend domain-name-servers $(hostname -i);\n/" /etc/dhcp/dhclient.conf

testing

sudo docker run --net=host tutum/dnsutils dig google.com
sudo docker run --net=host tutum/dnsutils dig master.mesos
