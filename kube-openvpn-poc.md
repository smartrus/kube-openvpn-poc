# kube-openvpn POC

```
git clone https://github.com/pieterlange/kube-openvpn.git

cd kube-openvpn
sudo docker run --user=$(id -u) -e OVPN_SERVER_URL=tcp://vpn.my.fqdn:1194 -v $PWD:/etc/openvpn:z -ti ptlange/openvpn ovpn_initpki
sudo docker run --user=$(id -u) -e EASYRSA_CRL_DAYS=180 -v $PWD:/etc/openvpn:z -ti ptlange/openvpn easyrsa gen-crl

kubectl cluster-info dump | grep cidr
kubectl create namespace openvpn
sudo ./kube/deploy.sh openvpn tcp://vpn.my.fqdn:1194 10.233.0.0/18 10.233.64.0/18

kubectl get services --all-namespaces

# Generate VPN client credentials for CLIENTNAME without password protection; leave 'nopass' out to enter password
sudo docker run --user=$(id -u) -v $PWD:/etc/openvpn:z -ti ptlange/openvpn easyrsa build-client-full CLIENTNAME nopass
sudo docker run --user=$(id -u) -e OVPN_SERVER_URL=tcp://vpn.my.fqdn:1194 -v $PWD:/etc/openvpn:z --rm ptlange/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn
```
