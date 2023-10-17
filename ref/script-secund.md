#!/bin/bash

# Instala el servidor DHCP
sudo apt-get update
sudo apt-get install isc-dhcp-server

# Configura el servidor DHCP secundario en la interfaz enp0s8
cat <<EOF | sudo tee /etc/dhcp/dhcpd.conf
authoritative;
subnet 192.168.0.0 netmask 255.255.255.0 {
  range 192.168.0.100 192.168.0.200;
  option routers 192.168.0.1;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
}

failover peer "dhcp-failover" {
  secondary;
  address 192.168.0.11;
  port 847;
  peer address 192.168.0.10;
  peer port 647;
  max-response-delay 60;
  max-unacked-updates 10;
}
EOF

# Configura la interfaz de red para el servidor DHCP secundario
cat <<EOF | sudo tee /etc/default/isc-dhcp-server
INTERFACES="enp0s8"
EOF

# Reinicia el servicio DHCP para aplicar la configuración
sudo service isc-dhcp-server restart

