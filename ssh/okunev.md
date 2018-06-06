```
apt-get install -y uml-utilities bridge-utils ifupdown

cat >/etc/network/interaces.d/sshvpn0 <<EOF
auto tap8
iface tap8 inet manual
	pre-up tunctl -u xaionaro -t $IFACE
	post-down tunctl -d $IFACE
	up ifconfig $IFACE 0.0.0.0 up
	down ifconfig $IFACE down

auto sshvpn0
iface sshvpn0 inet static
	address 172.31.248.3
	netmask 255.255.255.240
	bridge-stp off
	bridge-ports tap8
	mtu 1300
EOF

cat >~/.ssh/config <<EOF
Host vpn.telemed.help
	Tunnel=Ethernet
	TunnelDevice=8:8
EOF

ifup tap8
ifup sshvpn0
trezor-agent -c vpn.telemed.help …
```
