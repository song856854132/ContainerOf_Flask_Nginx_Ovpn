# Deploy Web Services(VPN, website, firewall) by Docker-compose
###### tags: `web develope`
Requirement : 
* docker
* docker-compose
* git

## Git clone from remote:
```shell=
$git clone https://github.com/song856854132/ContainerOf_Flask_Nginx_Ovpn.git
```
## Setup steps :
* For the first time running, setup openvpn ca & pki  
```shell=
$docker-compose run --rm openvpn ovpn_genconfig -u udp://IP address
$docker-compose run --rm openvpn ovpn_initpki
$sudo chown -R $(whoami): ./openvpn-data
```
* Then compose up all container by "docker-compose" command
```shell=
$sudo docker-compose up -d
```
## For client certificate
* with a passphrase (recommended)
```shell=
$docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME
```
* without a passphrase (not recommended)
```shell=
$docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME nopass
```
## Produce .ovpn file
```shell=
$docker-compose run --rm openvpn ovpn_getclient $CLIENTNAME > $CLIENTNAME.ovpn
```
## For Revoke a client certificate
* Keep the corresponding crt, key and req files.
```shell=
$docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME
```
* Remove the corresponding crt, key and req files.
```shell=
$docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME remove
```
## Result
use docker ps to show the running container
```shell=
$sudo docker ps
[sudo] password for $(whoami): 
CONTAINER ID   IMAGE                            COMMAND                  CREATED      STATUS      PORTS                                      NAMES
924a05cc8d3b   container_flasknginxovpn_nginx   "/docker-entrypoint.…"   6 days ago   Up 6 days   0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   template_nginx
aa123664803c   container_flasknginxovpn_flask   "uwsgi app.ini"          6 days ago   Up 6 days   8080/tcp                                   template_flask
77fe464fc4c5   kylemanna/openvpn                "ovpn_run"               6 days ago   Up 6 days   0.0.0.0:1194->1194/udp                     openvpn
7868b9fb5540   postgres                         "docker-entrypoint.s…"   6 days ago   Up 6 days 
```

```shell=
$tree
./
├── docker-compose.yml
├── flask
│   ├── app
│   │   ├── config
│   │   │   └── config.py
│   │   ├── __init__.py
│   │   └── models
│   │       └── users.py
│   ├── app.ini
│   ├── Dockerfile
│   ├── main.py
│   └── requirements.txt
├── flask-template
│   ├── img
│   │   └── flask-developer-roadmap.png
│   ├── README.md
│   ├── template3-docker-compose-flask-nginx-postgres
│   ├── template-flask-blueprint-with-factory
│   │   └── flask
│   │       ├── app
│   │       │   ├── config
│   │       │   │   └── config.py
│   │       │   ├── __init__.py
│   │       │   └── view
│   │       │       └── auth.py
│   │       ├── main.py
│   │       └── requirements.txt
│   ├── template-flask-factory-Application
│   │   └── flask
│   │       ├── app
│   │       │   ├── config
│   │       │   │   └── config.py
│   │       │   └── __init__.py
│   │       ├── main.py
│   │       └── requirements.txt
│   ├── template-flask-i18n
│   │   └── flask
│   │       ├── app
│   │       │   ├── babel.cfg
│   │       │   ├── config
│   │       │   │   └── config.py
│   │       │   ├── __init__.py
│   │       │   ├── templates
│   │       │   │   └── index.html
│   │       │   ├── translations
│   │       │   │   └── zh
│   │       │   │       └── LC_MESSAGES
│   │       │   │           └── messages.po
│   │       │   └── view
│   │       │       └── index.py
│   │       ├── main.py
│   │       └── requirements.txt
│   ├── template-flask-jwt
│   │   ├── app
│   │   │   ├── config
│   │   │   │   └── config.py
│   │   │   ├── __init__.py
│   │   │   ├── model
│   │   │   │   └── users.py
│   │   │   ├── templates
│   │   │   │   └── index.html
│   │   │   └── view
│   │   │       └── v1
│   │   │           ├── auth.py
│   │   │           └── __init__.py
│   │   ├── main.py
│   │   └── requirements.txt
│   ├── template-flask-login
│   │   ├── app
│   │   │   ├── config
│   │   │   │   ├── config.py
│   │   │   │   └── test.db
│   │   │   ├── __init__.py
│   │   │   ├── model
│   │   │   │   └── user.py
│   │   │   ├── templates
│   │   │   │   ├── login.html
│   │   │   │   └── signup.html
│   │   │   └── view
│   │   │       ├── abort_msg.py
│   │   │       └── auth.py
│   │   ├── main.py
│   │   └── requirements.txt
│   └── template-flask-sqlalchemy
│       ├── hashtag.db
│       ├── hash_tag.py
│       ├── main.py
│       ├── n1.db
│       ├── n1_queries.py
│       └── requirements.txt
├── nginx
│   ├── Dockerfile
│   ├── nginx.conf
│   ├── ssl.csr
│   └── ssl.key
├── openvpn-data
│   └── conf
│       ├── ccd
│       ├── crl.pem
│       ├── openvpn.conf
│       ├── ovpn_env.sh
│       └── pki
│           ├── ca.crt
│           ├── certs_by_serial
│           │   ├── 4CBD20A1BE70AEDF675F3D1FC0AC2B72.pem
│           │   └── 68FACA2C7F700A34649F562BFE52BB71.pem
│           ├── crl.pem
│           ├── dh.pem
│           ├── index.txt
│           ├── index.txt.attr
│           ├── index.txt.attr.old
│           ├── index.txt.old
│           ├── issued
│           │   ├── 140.117.12.84.crt
│           │   └── user00.crt
│           ├── openssl-easyrsa.cnf
│           ├── private
│           │   ├── 140.117.12.84.key
│           │   ├── ca.key
│           │   └── user00.key
│           ├── renewed
│           │   ├── certs_by_serial
│           │   ├── private_by_serial
│           │   └── reqs_by_serial
│           ├── reqs
│           │   ├── 140.117.12.84.req
│           │   └── user00.req
│           ├── revoked
│           │   ├── certs_by_serial
│           │   ├── private_by_serial
│           │   └── reqs_by_serial
│           ├── safessl-easyrsa.cnf
│           ├── serial
│           ├── serial.old
│           ├── ta.key
│           └── user00.ovpn
├── postgres-data [error opening dir]
└── README_vpnsetup.md
```
