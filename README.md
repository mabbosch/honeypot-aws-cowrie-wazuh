# honeypot-aws-cowrie-wazuh
Despliegue de un honeypot en AWS con Cowrie y Nginx, conectado mediante WireGuard a un servidor Wazuh local para la centralización de eventos.
# Honeypot en AWS con Cowrie y Wazuh

Proyecto de ciberseguridad centrado en el despliegue de un honeypot
en AWS para exponer servicios señuelo a Internet y registrar la
actividad recibida.

El laboratorio combina un servidor Ubuntu en AWS con **Cowrie** como
servicio SSH señuelo y **Nginx** como servicio web. Los eventos se
centralizan en un servidor **Wazuh** situado en el entorno local.

La comunicación entre AWS y el entorno local se realiza mediante
**WireGuard**, evitando exponer directamente el servidor Wazuh a
Internet.

##Objetivos

- Diseñar la infraestructura del laboratorio en AWS.
- Desplegar servicios señuelo accesibles desde Internet.
- Separar el acceso SSH administrativo del servicio SSH de Cowrie.
- Comunicar AWS con el entorno local mediante WireGuard.
- Centralizar los eventos generados por el honeypot en Wazuh.
- Preparar la infraestructura para una posterior fase de análisis.

## Arquitectura

[AQUÍ PONDREMOS EL DIAGRAMA]

La infraestructura está dividida entre AWS y el entorno local.

En AWS se encuentra el honeypot, formado por un servidor Ubuntu con
Cowrie y Nginx. El agente Wazuh instalado en el honeypot envía los
eventos al Wazuh Manager situado en una máquina virtual local.

La comunicación entre ambos entornos se realiza mediante una VPN
WireGuard.

## Tecnologías utilizadas

- AWS
- Ubuntu Server
- Cowrie
- Nginx
- Wazuh
- WireGuard
- DNS
- HTTPS / TLS
- iptables
- systemd
