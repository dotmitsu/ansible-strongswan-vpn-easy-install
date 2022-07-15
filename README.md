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
<p>
3) UFW doesn't delete added rules. For example: If you used ssh port 22, and after that you changed it 
to another (for example 20022), the rule with 22 port will remain. You need to remove it manually.
This is done in order not to delete other rules on the server.
</p>

<h1><b>Clients setup</b></h1>
<details>
<summary><b>1. Ubuntu or other Linux</b></summary>

<h2>Ubuntu or other Linux</h2>
Copy <code>/etc/ipsec.d/cacerts/ca-cert.pem</code> from remote host to local host 
<code>/etc/ipsec.d/cacerts/ca-cert.pem</code>

It is necessary that the key is located in this path /etc/ipsec.d/cacerts/ca-cert.pem on local mashine. 
Otherwise, the vpn client may not accept it.<br>
Use any vpn client you want.

Auth method EAP<br>
Use ca-cert.pem and login/password for vpn client.
</details>

<details>
<summary><b>2. Windows</b></summary>
<h2>Windows</h2>
Open <code>Manage Computer Certificates</code>.<br>
Add ca-cert.pem to <code>Trusted Root Certification Authorities</code>.

By steps:<br>

1)
<img alt="Windows_1" src="README_src/Windows/Windows_01.png" width="600">
<br><br>

2)
<img alt="Windows_2" src="README_src/Windows/Windows_02.png" width="600">
<br><br>

3)
<img alt="Windows_3" src="README_src/Windows/Windows_03.png" width="600">
<br><br>

4)
Choose All Files(&ast;.&ast;) and select ca-cert.pem

<img alt="Windows_4" src="README_src/Windows/Windows_04.png" width="600">
<br><br>

5)
<img alt="Windows_5" src="README_src/Windows/Windows_05.png" width="600">
<br><br>

6)
<img alt="Windows_6" src="README_src/Windows/Windows_06.png" width="600">
<br><br>

7)
<img alt="Windows_7" src="README_src/Windows/Windows_07.png" width="600"><br>

After that you can create VPN connection in Windows Settings. 
VPN Type: IKEv2, Authenticate by Login/Password. (Tested on Windows 10, 11)
<br>
</details>

<details>
<summary><b>3. iOS</b></summary>
<h2>iOS</h2>

1) Download ca-cert.pem using Safari (it is important use Safari browser). Then go to
Settings and open "Profile Downloaded" and choose "Install".


<img alt="iOS_01" src="README_src/iOS/iOS_01.png" width="300"/>
<br><br>
<img alt="iOS_01_1" src="README_src/iOS/iOS_01_1.png" width="300"/>
<br><br>
<img alt="iOS_01_2" src="README_src/iOS/iOS_01_2.png" width="300"/>
<br><br>

2) After that go to Settings <code>General -> VPN & Device Management -> VPN -> Add VPN Configuration</code><br>

<img alt="iOS_02" src="README_src/iOS/iOS_02.png" width="300"/>
<br><br>
<img alt="iOS_03" src="README_src/iOS/iOS_03.png" width="300"/>
<br><br>
<img alt="iOS_04" src="README_src/iOS/iOS_04.png" width="300"/>
<br><br>
<img alt="iOS_05" src="README_src/iOS/iOS_05.png" width="300"/>
<br><br>

3) Fill in the fields.

<img alt="iOS_06" src="README_src/iOS/iOS_06.png" width="300"/>
<br>
Type: IKEv2<br>
Server: your <code>server address</code><br>
Remote ID: your <code>server address</code><br>
User Authentication: <code>Username</code><br>
Username: your <code>login</code><br>
Password: your <code>password</code><br>
</details>

<details>
<summary><b>4. Android</b></summary>
<h2>Android</h2>

You have 2 ways:<br>

1) Use official application from Play Market <code>strongSwan VPN Client</code>
2) Use Android settings and create VPN Connection.

<h3>The 1 way:</h3>

1) Install the application <code>strongSwan VPN Client</code>

<img alt="Android_1" src="README_src/Android/the_1_way/Android_01.png" width="300">
<br><br>

2)
<img alt="Android_2" src="README_src/Android/the_1_way/Android_02.png" width="300">
<br><br>

3)
<img alt="Android_3" src="README_src/Android/the_1_way/Android_03.png" width="300">
<br><br>

4)
<img alt="Android_4" src="README_src/Android/the_1_way/Android_04.png" width="300">
<br><br>

5) Tap to Import certificate and choose ca-cert.pem file.

<img alt="Android_5" src="README_src/Android/the_1_way/Android_05.png" width="300">
<br><br>

6) Go back to main screen and choose "ADD VPN PROFILE". Fill in the fields, uncheck <code>Select CA certificate</code>
and choose imported certificate. 

<img alt="Android_6" src="README_src/Android/the_1_way/Android_06.png" width="300">
<br><br>

7) Sometimes the imported certificate is not displayed. In this case, go back
and open this menu again.

<img alt="Android_7" src="README_src/Android/the_1_way/Android_07.png" width="300">
<br><br>

8) You can add VPN shortcut to Android top menu.

<img alt="Android_8" src="README_src/Android/the_1_way/Android_08.png" width="300">
<br>

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
</details>