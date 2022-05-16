1) ipsec.secrets file is encrypted by ansible-vault

ipsec.secrets structure is:

: RSA "server-key.pem"<br/>
vpn_username1 : EAP "password"<br/>
vpn_username2 : EAP "password"<br/>
vpn_username3 : EAP "password"

Delete ipsec.secrets and create your ipsec.secrets file in files directory 

2) Change inventories, group_vars, host_vars values to your own 
3) Replace remote_user = dotmitsu to your user in ansible.cfg
4) Start playbook
ansible-playbook -i ./inventories/hosts ./strongswan.yml

5) Copy /etc/ipsec.d/cacerts/ca-cert.pem from remote host to local host /etc/ipsec.d/cacerts/ca-cert.pem


It is necessary that the key is located in this path /etc/ipsec.d/cacerts/ca-cert.pem on local mashine. 
Otherwise, the vpn client may not accept it.

Use this key and login/password from ipsec.secrets file for vpn client.
Auth method EAP

P.S.
If you need to regenerate all certs, delete /etc/ipsec.d/private/ca-key.pem on remote host
and ansible role automaticaly will regenerate all necessary keys at the next start of role.

OR

start ansible-playbook with variable generate_certs=true

ansible-playbook -i ./inventories/hosts ./strongswan.yml -e "generate_certs=true"