FROM ubuntu:14.04

MAINTAINER Felipe Gómez <fgomezk@gmail.com>

#Actualizamos repositorio
RUN apt-get update

#Actualizamos el sistema
RUN apt-get -y upgrade

#Instalamos openssh-server
RUN apt-get -y install openssh-server 

#Creamos el usuario de sistema odoo con la carpeta home en /opt/odoo
RUN sudo adduser --system --home=/opt/odoo --group odoo

#Instalamos las dependencias para Odoo
RUN sudo apt-get install -y python-dateutil python-decorator python-docutils python-feedparser python-gdata python-gevent python-imaging python-jinja2 python-ldap python-libxslt1 python-lxml python-mako python-mock python-openid python-passlib python-psutil python-psycopg2 python-pybabel python-pychart python-pydot python-pyparsing python-pypdf python-reportlab python-requests python-simplejson python-tz python-unittest2 python-vatnumber python-vobject python-werkzeug python-xlwt python-yaml wkhtmltopdf

#Instala git
RUN sudo apt-get -y install git

#Instalamos python-unidecode para AEAT de localizacion española
RUN sudo apt-get -y install python-unidecode

#Instalamos python-pip para modulos de TPV y asterisk
RUN sudo apt-get -y install python-pip

#Instalamos pyserial, pyusb, qrcode para TPV y PosBox
RUN sudo pip install pyserial
RUN sudo pip install pyusb
RUN sudo pip install qrcode

#Bajamos phonenumbers, py-asterisk para asterisk
RUN sudo pip install phonenumbers
RUN sudo pip install py-asterisk

#Instalamos mercurial
RUN sudo apt-get -y install mercurial

#Bajamos web_action_request, web_socketio
RUN sudo hg clone https://bitbucket.org/anybox/web_action_request
RUN sudo hg clone https://bitbucket.org/anybox/web_socketio

#Descargamos odoo de git dentro de /opt/odoo
RUN cd /opt/odoo
RUN sudo su - odoo -s /bin/bash -c "git clone https://www.github.com/odoo/odoo --depth 1 --branch 8.0 --single-branch ."

#Creamos la carpeta extras
RUN mkdir /opt/odoo/extras
RUN cd /opt/odoo/extras

#Bajamos localizacion española
RUN sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/l10n-spain.git --depth 1 --branch 8.0 --single-branch"

#Bajamos partner-contact
RUN sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/partner-contact.git --depth 1 --branch 8.0 --single-branch"

#Bajamos account-payment
RUN sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/account-payment.git --depth 1 --branch 8.0 --single-branch"

#Bajamos account-financial-tools
RUN sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/account-financial-tools.git --depth 1 --branch 8.0 --single-branch"

#Bajamos bank-payment
RUN sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/bank-payment.git --depth 1 --branch 8.0 --single-branch"

#Bajamos connector-telephony
RUN sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/connector-telephony.git --depth 1 --branch 8.0 --single-branch"

#Creamos fichero de configuracion /etc/odoo-server.conf  y le ponemos los permisos
RUN sudo echo "[options]" >  /etc/odoo-server.conf
RUN sudo echo "; This is the password that allows database operations:" >>  /etc/odoo-server.conf
RUN sudo echo "; admin_passwd = admin" >>  /etc/odoo-server.conf
RUN sudo echo "db_host = False" >  /etc/odoo-server.conf
RUN sudo echo "db_port = False" >  /etc/odoo-server.conf
RUN sudo echo "db_user = odoo" >  /etc/odoo-server.conf
RUN sudo echo "db_password = N50d00" >  /etc/odoo-server.conf
RUN sudo echo "#addons_path = /opt/odoo/addons,/opt/odoo/extra/l10n-spain,/opt/odoo/extra/partner-contact,/opt/odoo/extra/account-payment,/opt/odoo/extra/account-financial-tools,/opt/odoo/extra/bank-payment,/opt/odoo/extra/connector-telephony,/opt/odoo/extra/web_action_request,/opt/odoo/extra/web_socketio,/opt/odoo/extra/web_longpolling" >  /etc/odoo-server.conf
RUN sudo echo "addons_path = /opt/odoo/addons,/opt/odoo/extra/l10n-spain,/opt/odoo/extra/partner-contact,/opt/odoo/extra/account-payment,/opt/odoo/extra/account-financial-tools,/opt/odoo/extra/bank-payment" >  /etc/odoo-server.conf
RUN sudo echo "logfile = /var/log/odoo/odoo-server.log" >  /etc/odoo-server.conf

RUN sudo chown odoo: /etc/odoo-server.conf
RUN sudo chmod 640 /etc/odoo-server.conf

#RUN sudo sed -i s/"db_password = .*"/"db_password = N50d00"/g /etc/odoo-server.conf 
#RUN sudo sed -i s/"db_user = .*"/"db_user = odoo"/g /etc/odoo-server.conf

#Copiamos el fichero de odoo-server de opensourcerer y le ponemos los permisos
ADD http://www.theopensourcerer.com/wp-content/uploads/2014/09/odoo-server /etc/init.d/odoo-server
RUN sudo chmod 755 /etc/init.d/odoo-server
RUN sudo chown root: /etc/init.d/odoo-server

RUN sudo mkdir /var/log/odoo
RUN sudo chown odoo:root /var/log/odoo

#Actualizamos el update-rc para que se ejecute al iniciar
RUN sudo update-rc.d odoo-server defaults

#Abrimos los puertos 8069 y 8072
EXPOSE 8069 8072
