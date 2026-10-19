
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

## Infraestructura en AWS

Para el laboratorio se utilizó una VPC en AWS con diferentes subredes para separar los componentes de la infraestructura.

El honeypot se desplegó en una instancia **Ubuntu Server**, permitiendo desde Internet únicamente el tráfico necesario para los servicios señuelo.

![Infraestructura de red en AWS](images/aws-network.png)

El tráfico se controló mediante **Network ACL y Security Groups**, permitiendo el acceso público a:

- **22/TCP** → servicio SSH señuelo de Cowrie.
- **80/TCP** → servicio web HTTP.
- **443/TCP** → servicio web HTTPS.

El acceso administrativo real al servidor se mantuvo separado del servicio SSH expuesto.

##  Servicio web señuelo

En la misma instancia se instaló **Nginx** para publicar una página web ficticia accesible desde Internet.

Se configuró resolución DNS y posteriormente se habilitó **HTTPS mediante un certificado TLS de Let's Encrypt**.

![Web señuelo](images/web-honeypot.png)

Las peticiones recibidas por Nginx quedaban registradas para poder analizarlas posteriormente mediante Wazuh.

##  Honeypot SSH con Cowrie

Para simular un servicio SSH vulnerable se instaló **Cowrie** utilizando un usuario específico y un entorno virtual de Python.

![Cowrie](images/cowrie.png)

Cowrie escuchaba internamente en el puerto **2222**, mientras que las conexiones procedentes de Internet llegaban al puerto estándar **22**.

Mediante una regla de `iptables`, el tráfico recibido en el puerto 22 se redirigía hacia Cowrie:

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-ports 2222

