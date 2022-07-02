<h1><b>Server setup</b></h1>
This role tested on Ubuntu 20.04.

1) Change inventories, group_vars, host_vars values to your own 
2) Start playbook<br>
<p>
<code>ansible-playbook -i ./inventories/hosts ./strongswan.yml</code>
</p>
<br>
<b>Recommendations</b><br>
<p>
1) If you need to recreate all certs, delete /etc/ipsec.d/private/ca-key.pem on remote host
and ansible role automaticaly will regenerate all necessary keys at the next start of role.

OR

run ansible-playbook with variable generate_certs=true

<code>ansible-playbook -i ./inventories/hosts ./strongswan.yml -e "generate_certs=true"</code>
</p>
<p>
2) When you add or edit user credentials, ipsec.service doesn't restart. It rereads the config. Works well in 90 percent
cases. You can write some symbols in username or password and strongswan may crash when try to read it.
In this case you need to change credentials and restart ipsec.service, run ansible-playbook with ipsec_restart=true

<code>ansible-playbook -i ./inventories/hosts ./strongswan.yml -e "ipsec_restart=true"</code>
</p>

<h1><b>Clients setup</b></h1>

<h2><b>Ubuntu or other Linux</b></h2>
Copy <code>/etc/ipsec.d/cacerts/ca-cert.pem</code> from remote host to local host 
<code>/etc/ipsec.d/cacerts/ca-cert.pem</code>

It is necessary that the key is located in this path /etc/ipsec.d/cacerts/ca-cert.pem on local mashine. 
Otherwise, the vpn client may not accept it.
Use any vpn client you want.

Auth method EAP<br>
Use ca-cert.pem and login/password for vpn client.

<h2><b>Windows</b></h2>
Open <code>Manage Computer Certificates</code>.<br>
Add ca-cert.pem to <code>Trusted Root Certification Authorities</code>.

By steps:<br>

1)
<img alt="Windows_1" src="../../README_src/strongswan/Windows/Windows_01.png" width="600">

2)
<img alt="Windows_2" src="../../README_src/strongswan/Windows/Windows_02.png" width="600">

3)
<img alt="Windows_3" src="../../README_src/strongswan/Windows/Windows_03.png" width="600">

4)
Choose All Files(&ast;.&ast;)

<img alt="Windows_4" src="../../README_src/strongswan/Windows/Windows_04.png" width="600">

5)
<img alt="Windows_5" src="../../README_src/strongswan/Windows/Windows_05.png" width="600">

6)
<img alt="Windows_6" src="../../README_src/strongswan/Windows/Windows_06.png" width="600">

7)
<img alt="Windows_7" src="../../README_src/strongswan/Windows/Windows_07.png" width="600">

After that you can create VPN connection in Windows Settings. 
VPN Type: IKEv2, Authenticate by Login/Password. (Tested on Windows 11)

<h2><b>Android</b></h2>
You have 2 ways:
1) Use official application from Play Market <code>strongSwan VPN Client</code>
2) Use Android settings and create VPN Connection.

<h3>The 1 way:</h3>
1) Install the application <code>strongSwan VPN Client</code>

<img alt="Android_1" src="../../README_src/strongswan/Android/the_1_way/Android_01.png" width="300">

2)
<img alt="Android_2" src="../../README_src/strongswan/Android/the_1_way/Android_02.png" width="300">

3)
<img alt="Android_3" src="../../README_src/strongswan/Android/the_1_way/Android_03.png" width="300">

4)
<img alt="Android_4" src="../../README_src/strongswan/Android/the_1_way/Android_04.png" width="300">

5) Tap to Import certificate and choose ca-cert.pem file.

<img alt="Android_5" src="../../README_src/strongswan/Android/the_1_way/Android_05.png" width="300">

6) Uncheck <code>Select CA certificate</code> and choose imported certificate. 

<img alt="Android_6" src="../../README_src/strongswan/Android/the_1_way/Android_06.png" width="300">

7) Sometimes the imported certificate is not displayed. In this case, go back
and open this menu again.

<img alt="Android_7" src="../../README_src/strongswan/Android/the_1_way/Android_07.png" width="300">

8) You can add VPN shortcut to Android top menu.

<img alt="Android_8" src="../../README_src/strongswan/Android/the_1_way/Android_08.png" width="300">

<h3>The 2 way:</h3>

1) Go to Android Settings, then <br>
<code>Security -> Encription & Credentials -> Install a certificate -> CA certificate</code><br>
and install ca-cert.pem
2) Go to Network -> VPN and create VPN conecction profile.<br>
Type: IKEv2/IPSec MSCHAPv2<br>
Server address: your <code>server address</code><br>
IPSec CA certificate - choose your imported certificate<br>
IPSec identifier: your <code>login</code><br>
Username: your <code>login</code><br>
Password: your <code>password</code><br>

<h2><b>iOS</b></h2>

1) Download ca-cert.pem using Safari (it is important use Safari browser).
System will write that you can import certificate. Do it.
2) After that go to Settings <code>General -> VPN -> Add VPN Configuration</code>
Type: IKEv2
Server: your <code>server address</code><br>
Remote ID: your <code>server address</code><br>
Local ID: your <code>login</code><br>
User Authentication: <code>Username</code>
Username: your <code>login</code><br>
Password: your <code>password</code><br>