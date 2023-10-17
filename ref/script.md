#!/bin/bash

# Instala el servidor DHCP
sudo apt-get update
sudo apt-get install isc-dhcp-server

# Configura el servidor DHCP primario en la interfaz enp0s3
cat <<EOF | sudo tee /etc/dhcp/dhcpd.conf
authoritative;
subnet 172.26.0.0 netmask 255.255.0.0 {
  range 172.26.0.100 172.26.255.200;
  option routers 172.26.0.1;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
}

failover peer "dhcp-failover" {
  primary;
  address 192.168.0.10;
  port 647;
  peer address 192.168.0.11;
  peer port 847;
  max-response-delay 60;
  max-unacked-updates 10;
}
EOF

# Configura la interfaz de red para el servidor DHCP primario
cat <<EOF | sudo tee /etc/default/isc-dhcp-server
INTERFACES="enp0s3"
EOF

# Reinicia el servicio DHCP para aplicar la configuración
sudo service isc-dhcp-server restart
