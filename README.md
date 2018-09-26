# Inspec
## For docker security

InSpec es un marco de tiempo de ejecución de código abierto y un lenguaje de reglas utilizado para especificar requisitos de cumplimiento, seguridad y política para probar cualquier nodo en su infraestructura.
Protección de Docker EE y mejores prácticas de seguridad
Docker se centra en tres áreas clave de la seguridad de los contenedores: acceso seguro , de contenido seguro , y la plataforma segura . Esto resulta en tener características de aislamiento y contención no solo integradas en Docker EE sino también habilitadas de fábrica

# 1-. Configuración Host 


### 1.5 Docker daemon se ejecuta con privilegios ‘root’. Por lo tanto, es necesario auditar sus actividades y su uso.
Auditar:
auditctl -l | grep /usr/bin/docker

S:
Agregue la línea de la siguiente línea en  /etc/audit/audit.rules file:
-w /usr/bin/docker -k docker

reinicie el servicio
service auditd restart


1.6 
/var/lib/docker es un directorio que contiene toda la información acerca de los contenedores. Debe ser auditado.
Auditar:
/var/lib/docker 
auditctl -l | grep /var/lib/docker

S:
añadir en  /var/lib/docker 

-w /var/lib/docker -k docker

reiniciar el servicio
service auditd restart

1.7 /etc/docker es un directorio que lleva a cabo diversos certificados y claves utilizadas para la comunicación TLS entre Docker daemon y Docker client
Auditar:
auditctl -l | grep /etc/docker

añadir en --->  /etc/audit/audit.rules 
-w /etc/docker -k docker

reiniciar
service auditd restart

1.8
docker.service puede estar presente si los parámetros demonio se han cambiado por un administrador. Contiene varios parámetros para docker daemon
Auditar:
encontrar el archivo
systemctl show -p FragmentPath docker.service
si existe, entonces:
auditctl -l | grep docker.service

S:
Añadir en  /etc/audit/audit.rules:
-w /usr/lib/systemd/system/docker.service -k docker

reiniciar el servicio
service auditd restart

1.9  docker.socket lleva a cabo diversos parámetros en el zócalo de demonio de estibador debe ser auditado
Auditar:
encontrar el archivo
systemctl show -p FragmentPath docker.socket

si el archivo no existe, no aplicar esta recomendacion y si si existe 
auditctl -l | grep docker.socket

S:
añadir en /etc/audit/audit.rules 
-w /usr/lib/systemd/system/docker.socket -k docker

reiniciar el servicio
service auditd restart

1.10
/etc/default/docker contiene varios parámetros para docker daemon
Auditar:
auditctl -l | grep /etc/default/docker

S:
añadir en /etc/audit/audit.rules :
-w /etc/default/docker -k docker

reiniciar el servicio:
service auditd restart

1.11
/etc/docker/daemon.json contiene varios parámetros para docker daemon
Auditar:
auditctl -l | grep /etc/docker/daemon.json

S: añadir en  /etc/audit/audit.rules:
-w /etc/docker/daemon.json -k docker

reiniciar el servicio:
service auditd restart

1.12  /usr/bin/docker-containerd se basa en containerd y Runc para desovar contenedores.
Auditar:
auditctl -l | grep /usr/bin/docker-containerd
S:
añadir en  /etc/audit/audit.rules :
-w /usr/bin/docker-containerd -k docker
reiniciar el servicio
service auditd restart

1.13
/usr/bin/docker-runc  se basa en containerd y Runc para desovar contenedores.
Auditar:

auditctl -l | grep /usr/bin/docker-runc

Añadir en  /etc/audit/audit.rules :
-w /usr/bin/docker-runc -k docker

reiniciar el servicio
service auditd restart


