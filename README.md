# Guía para instalación y cliente de Mosquitto en Python

## Instalación de Mosquitto
Dependiendo del sistema operativo que se tenga, se puede instalar Mosquitto de diferentes maneras. En el caso de Ubuntu, se puede instalar con el siguiente comando:

```bash
sudo apt-get update
sudo apt-get install mosquitto mosquitto-clients
```

Para otras distribuciones de Linux, se puede consultar la [documentación oficial](https://mosquitto.org/download/).

## Configuracion de Mosquitto
Para configurar Mosquitto, se debe editar el archivo de configuración que se encuentra en `/etc/mosquitto/mosquitto.conf`. En este archivo se pueden configurar diferentes aspectos del broker, como el puerto, el usuario y contraseña, entre otros.

Para configurar el puerto, se debe agregar una línea con el siguiente formato:

```
listener <numero de puerto>
```

El número de puerto establecido para el proyecto es el 16911.

Debemos tabmién configurar mosquitto para que acepte conexiones de clientes anónimos. Para ello, debemos agregar las siguientes líneas al archivo de configuración:

```
allow_anonymous true
```

## Cliente de Mosquitto en Python
Para instalar el cliente de Mosquitto en Python, debemos primero crear un entorno virtual con el siguiente comando:

```bash
python3 -m venv env
```

Con el entorno virtual creado, debemos activarlo con el siguiente comando:

```bash
source env/bin/activate
```

Instalamos el cliente de Mosquitto con el siguiente comando:

```bash
pip install paho-mqtt
```

Con el entorno virtual activado, podemos correr los códigos de Python que se encuentran en la carpeta `src`.

## Ejecución del cliente de Mosquitto en Python
Para ejecutar el suscriptor, se debe correr el siguiente comando:

```bash
python src/client.py
```


## Montar el servidor de Mosquitto como servicio de systemd
Para configurar el script en un entorno virtual y ejecutarlo como un servicio, puedes seguir estos pasos adicionales:

#### 1\. Crear el entorno virtual

Primero, crea un entorno virtual en el mismo directorio donde está el script, por ejemplo, `/opt/mqtt_client`.

```bash
cd /opt/mqtt_client
python3 -m venv venv
```

#### 2\. Instalar las dependencias en el entorno virtual

Activa el entorno virtual y luego instala las librerías necesarias, como `paho-mqtt`.

```bash
source venv/bin/activate
pip install paho-mqtt
deactivate
```

#### 3\. Modificar el archivo de servicio para usar el entorno virtual

Edita el archivo del servicio para que ejecute el script dentro del entorno virtual (src/mqtt_client.service) para adecuarlo al entorno donde se encuentra el script.

#### 4\. Recargar el demonio de systemd y reiniciar el servicio

Después de hacer estos cambios, recarga `systemd` y reinicia el servicio:

```bash
sudo systemctl daemon-reload
sudo systemctl restart mqtt_client.service
```

#### 5\. Habilitar el servicio en el arranque

Para que el servicio inicie automáticamente al arrancar el sistema:

```bash
sudo systemctl enable mqtt_client.service
```

#### 6\. Verificar el servicio y los logs

Comprueba que el servicio esté funcionando correctamente y consulta los logs si es necesario:

```bash
sudo systemctl status mqtt_client.service
sudo journalctl -u mqtt_client.service -f
```

#### 7\. Adaptar la ruta de los archivos donde se escriben los logs
Modifica client.py para que los logs se escriban en una ruta adecuada para el sistema. Por ejemplo, puedes usar algún directorio de logs del sistema, como `/var/log/mqtt_client.log`.

Con esta configuración, el servicio se ejecutará dentro del entorno virtual y estará disponible cada vez que inicie el sistema.


## Referencias
- [Documentación oficial de Mosquitto](https://mosquitto.org/documentation/)
- [Documentación oficial de Paho-MQTT](https://www.eclipse.org/paho/index.php?page=clients/python/docs/index.php)
- [Documentación oficial de Python](https://docs.python.org/3/)