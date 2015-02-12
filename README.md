# odoo

Odoo Base instalado en /opt/odoo
  Con el usuario odoo de sistema y carpeta home en /opt/odoo
  Se descarga automáticamente con git
  Crea automáticamente el fichero de configuración /etc/odoo-server.conf
   El usuario de la base de datos es odoo con password odoo
   El fichero de log se crea en /usr/var/log/odoo/odoo.log
   Se crea el fichero /etc/init.d/odoo-server con las opciones start/restart/stop
   El servidor se inicia automáticamente
   
Con los siguientes repositorios en /opt/odoo/extra:

 l10n_es: Localización española
  account-financial-tools: Herramientas financieras
  partner-contact: Contactos

 bank-payment: para poder hacer los cobros SEPA

 connector-telephony: conector centralitas asterisk
  web_action_request
  web_longpolling
  wb_socket_io

