
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

## Objetivos

- Diseñar la infraestructura del laboratorio en AWS.
- Desplegar servicios señuelo accesibles desde Internet.
- Separar el acceso SSH administrativo del servicio SSH de Cowrie.
- Comunicar AWS con el entorno local mediante WireGuard.
- Centralizar los eventos generados por el honeypot en Wazuh.
- Preparar la infraestructura para una posterior fase de análisis.

## Arquitectura

![Arquitectura del laboratorio](images/arquitectura.png)


El laboratorio se divide en dos entornos principales:

**AWS**
- Instancia Ubuntu Server.
- Cowrie como servicio SSH señuelo.
- Nginx como servidor web.
- Agente Wazuh.

**Entorno local**
- Wazuh Manager.
- Wazuh Indexer.
- Wazuh Dashboard.

Ambos entornos se comunican mediante una VPN **WireGuard**, permitiendo enviar los eventos del honeypot hacia Wazuh sin exponer el servidor de monitorización directamente a Internet.

## Tecnologías utilizadas

`AWS` `Ubuntu Server` `Cowrie` `Nginx` `Wazuh` `WireGuard` `DNS` `HTTPS/TLS` `iptables` `systemd`

