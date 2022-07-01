This role tested on Ubuntu 20.04.

1) Change inventories, group_vars, host_vars values to your own 
2) Start playbook
ansible-playbook -i ./inventories/hosts ./strongswan.yml

3) Copy /etc/ipsec.d/cacerts/ca-cert.pem from remote host to local host /etc/ipsec.d/cacerts/ca-cert.pem

It is necessary that the key is located in this path /etc/ipsec.d/cacerts/ca-cert.pem on local mashine. 
Otherwise, the vpn client may not accept it.

Use this key and login/password for vpn client.
Auth method EAP

P.S.
If you need to recreate all certs, delete /etc/ipsec.d/private/ca-key.pem on remote host
and ansible role automaticaly will regenerate all necessary keys at the next start of role.

OR

run ansible-playbook with variable generate_certs=true

ansible-playbook -i ./inventories/hosts ./strongswan.yml -e "generate_certs=true"

P.S.S.
When you add or edit user credentials, ipsec.service doesn't restart. It rereads the config. Works well in 90 percent
cases. You can write some symbols in username or password and strongswan may crash when try to read it.
In this case you need to change credentials and restart ipsec.service, run ansible-playbook with ipsec_restart=true

ansible-playbook -i ./inventories/hosts ./strongswan.yml -e "ipsec_restart=true"