1. Intalmos los repositorios de nuestro debian y hacemos "apt update" y "apt upgrade"
2. Instalamos "isc-dhcp-server"
3. Deshabilitamos el network-manager "apt purgue network-manager"
4. "sudo ip addr flush dev enp0s3" para poner la ip estática
5. ponemos la siguiente configuracion en /etc/network/interfaces
![img1](images/)
