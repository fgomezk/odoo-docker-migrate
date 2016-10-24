FROM ubuntu:14.04

MAINTAINER Felipe Gómez <fgomezk@gmail.com>

RUN apt-get update \ # Actualizamos repositorio
 && apt-get -y upgrade \ # Actualizamos el sistema
 && apt-get -y install openssh-server \  # Instalamos openssh-server
                       python-dateutil \ # Instalamos las dependencias para Odoo
                       python-decorator \
                       python-docutils \
                       python-feedparser \
                       python-gdata \
                       python-gevent \
                       python-imaging \
                       python-jinja2 \
                       python-ldap \
                       python-libxslt1 \
                       python-lxml \
                       python-mako \
                       python-mock \
                       python-openid \
                       python-passlib \
                       python-psutil \
                       python-psycopg2 \
                       python-pybabel \
                       python-pychart \
                       python-pydot \
                       python-pyparsing \
                       python-pypdf \
                       python-reportlab \
                       python-requests \
                       python-simplejson \
                       python-tz \
                       python-unittest2 \
                       python-vatnumber \
                       python-vobject \
                       python-werkzeug \
                       python-xlwt \
                       python-yaml \
                       python-cups \
                       python-geoip \
#                       git \ # Instala git
                       python-unidecode \ # Necesario para la módulo AEAT de localización española
                       python-pip \ # Instalamos python-pip para modulos de TPV y asterisk
#                       mercurial \ #Instalamos mercurial para dependencias de los módulos de asterisk

 && sudo pip install pyserial \ # Instalamos pyserial, pyusb, qrcode para TPV y PosBox
                     pyusb \
                     qrcode \
#                     phonenumbers \ # Bajamos phonenumbers y py-asterisk para asterisk
#                     py-asterisk \
 
# && sudo hg clone https://bitbucket.org/anybox/web_action_request \ # Bajamos web_action_request, web_socketio para asterisk
# && sudo hg clone https://bitbucket.org/anybox/web_socketio \
 # wkhtmltopdf
 && curl -o wkhtmltox.deb -SL http://download.gna.org/wkhtmltopdf/0.12/0.12.2.1/wkhtmltox-0.12.2.1_linux-trusty-amd64.deb \
 && dpkg --force-depends -i wkhtmltox.deb \
 && apt-get -y install -f --no-install-recommends \ 
 && rm -rf /var/lib/apt/lists/* wkhtmltox.deb \

 && sudo adduser --system --home=/opt/odoo --group odoo \ # Creamos el usuario de stma odoo con carpeta home en /opt/odoo

USER odoo # Probar si vale poner sólo ésta línea o hay que usar el comando adduser

VOLUME /opt/odoo:/opt/odoo
# RUN cd /opt/odoo \ # Descargamos odoo de git dentro de /opt/odoo
# && sudo su - odoo -s /bin/bash -c "git clone https://www.github.com/odoo/odoo --depth 1 --branch 8.0 --single-branch ." \
# && mkdir /opt/odoo/extras \ # Creamos la carpeta extras
# && cd /opt/odoo/extras \ # Bajamos localizacion española
# && sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/l10n-spain.git --depth 1 --branch 8.0 --single-branch" \
    # Bajamos partner-contact
# && sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/partner-contact.git --depth 1 --branch 8.0 --single-branch" \
    # Bajamos account-payment
# && sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/account-payment.git --depth 1 --branch 8.0 --single-branch" \
    # Bajamos account-financial-tools
# && sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/account-financial-tools.git --depth 1 --branch 8.0 --single-branch" \
    # Bajamos bank-payment
# && sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/bank-payment.git --depth 1 --branch 8.0 --single-branch" \
    # Bajamos connector-telephony
# && sudo su - odoo -s /bin/bash -c "git clone https://github.com/OCA/connector-telephony.git --depth 1 --branch 8.0 --single-branch" \

# Creamos fichero de configuracion /etc/odoo-server.conf  y le ponemos los permisos
ADD odoo-server.conf /etc/odoo-server.conf
RUN sudo chown odoo: /etc/odoo-server.conf \
 && sudo chmod 640 /etc/odoo-server.conf \
 && sudo mkdir /var/log/odoo \
 && sudo chown odoo:root /var/log/odoo
 
# Copiamos el fichero de odoo-server de opensourcerer y le ponemos los permisos
ADD http://www.theopensourcerer.com/wp-content/uploads/2014/09/odoo-server /etc/init.d/odoo-server
RUN sudo chmod 755 /etc/init.d/odoo-server \
 && sudo chown root: /etc/init.d/odoo-server \
 && sudo update-rc.d odoo-server defaults # Actualizamos el update-rc para que se ejecute al iniciar

#Abrimos los puertos 8069 y 8072
EXPOSE 8069 8072
