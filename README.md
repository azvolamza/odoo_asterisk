# Odoo Asterisk
## Project description
This project is Asterisk Management system and also Asterisk development platform based on [Odoo](http://odoo.com).

Currently implemented features:
* **Asterisk .conf files editor** with Asterisk syntax highlight. Configuration files are stored in Odoo database and delivered to Asterisk server via Asterisk internal HTTP server.
* **Asterisk WEB console** with colors.
* **Stasis apps server** implementing different business logic, e.g. set callerid name from Odoo contacts.
* **Call QoS reporting** on every CDR.
* **CEL (Channel event logging)** on every call.
* **Call Recording** with automatic download and cleanup (embedded pbx ready).

## Project Components
### odoo-addons
This folder contains Odoo modules. You should update your odoo.conf and append path to this folder.
```
[options]
addons_path =  /usr/lib/python2.7/dist-packages/odoo/addons,/opt/odoo_asterisk/odoo-addons
```

Don't forget to activate *developer mode* and update apps list so that Odoo knows about these modules!

Also this folder contains requirements for Odoo, you can install them from pip:

```sh
pip install -r requirements.txt
```
#### PostgreSQL preparations
Before installing Odoo Asterisk addon modules you have to create PostgreSQL role for Asterisk (set your own password!):

```sql
template1=# create user asterisk with password 'asterisk';
CREATE ROLE
template1=#
```

### agent
This folder contains Asterisk agent that should be deployed on your Asterisk server.
Currently the agent gives a WEB Asterisk CLI in Odoo.

You should create a python environment using requirements.txt and run the agent.

Clone this repo on your Asterisk server, create python virtual environment, go to *agent* folder and run
```
virtualenv env
source env/bin/activate
pip install -r requirements.txt
python cli_server.py
```
After that go to Odoo and set Asterisk CLI URL	in server's settings to (for example if your asterisk server is on 192.168.1.1):
```
ws://192.168.1.1:8010/websocket
```
*Save*, go to Console tab and click *Edit* to activate Asterisk CLI. Click *Discard* to leave the console.

### services
This folder contains additional helper applications:
* stasis_apps.py - Asterisk Stasis Application implementing different services, e.g. setting callerid name from Odoo contacts.
* ami_broker.py - AMI client for asterisk used for example to catch Hangup event to download call recording.

You should create a virtualenvironment using this folder's requirements.txt and run both scripts.
See conf.py for configuration settings
