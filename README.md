# Pila-LEMP

# Indice

1. [Indice.](#Indice)
2. [Introducción.](#introducción)
3. [Creacion de servidores.](CREACION_DE_SERVIDORES) 
    * [Balanceador.](#Balanceador)
    * [Servidor NFS.](#NFSserver)
    * [Servidores web.](#Serverweb1y2)
    * [Servidor BBDD.](#GGBBserver)



# Introduccion

En esta práctica, se implementa una pila LEMP con la siguiente estructura:

Capa 1: Exposición a la red pública. Se utiliza una máquina con un balanceador de carga Nginx.

Capa 2: Backend.

Dos máquinas con un servidor web Nginx cada una.
Una máquina que aloja un servidor NFS y el motor PHP-FPM.

Capa 3: Datos. Se emplea una base de datos MariaDB.

Es importante destacar que las capas 2 y 3 no estarán accesibles desde la red pública. Los servidores web utilizarán una carpeta compartida proporcionada por NFS desde el servidor NFS. Además, emplearán el motor PHP-FPM instalado en la misma máquina que el servidor NFS.

# CREACION DE SERVIDORES.

# Balanceador

Para la configuración del balanceador, utilizaremos un proceso de aprovisionamiento que permitirá ahorrar tiempo y esfuerzo. Este proceso incluirá la instalación de Nginx, su inicio y la eliminación de archivos innecesarios.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/21e4b353-ee85-45dc-b099-d725e3e08f36)

Una vez iniciada la máquina, accederemos al directorio **/etc/nginx/sites-enabled** y crearemos un archivo llamado "balanceador". En este archivo, añadiremos el siguiente código:
 ```
upstream servidoresweb {
    server X.X.X.X;(direccion IP de servidor web)
    server X.X.X.X;(direccion IP de servidor web)
}
	
server {
    listen      80;
    server_name balanceador;

    location / {
	    proxy_redirect      off;
	    proxy_set_header    X-Real-IP \$remote_addr;
	    proxy_set_header    X-Forwarded-For \$proxy_add_x_forwarded_for;
        proxy_set_header    Host \$http_host;
        proxy_pass          http://servidoresweb;
	}
}
 ```



 ![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/aeef5f16-752f-4374-ab20-a8aaad649c2d)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/60783a9a-d5b0-40cc-a84c-8cb1e0a6a096)

# NFSserver
Para este servidor, llevaremos a cabo el siguiente aprovisionamiento para reducir el trabajo:

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/1cf0e0ef-7b1a-4db5-8e09-f11e8b61a6f8)

Una vez tengamos el servidor activo, crearemos el siguiente directorio.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/1e694aba-bfbc-45a0-a136-35a582db403e)

Después, accederemos al archivo /etc/exports y añadiremos las siguientes dos líneas para permitir compartir la carpeta creada anteriormente.
```
/var/nfs/compartir     X.X.X.X(rw,sync,no_root_squash,no_subtree_check)
/var/nfs/compartir     X.X.X.X(rw,sync,no_root_squash,no_subtree_check)
```

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/ca810b77-a0de-4bdd-acc7-160a9d2d8ffe)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/661465d3-7243-477f-915b-ef4ce438b058)

Una vez editado el archivo, nos moveremos a /var/nfs/compartir y descargaremos el siguiente archivo de WordPress.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/cae03f31-db06-42b4-8429-d8c9bcdf25fb)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/899e103b-ad73-4cbe-bc55-44de82686296)

Ahora habilitaremos los puertos de los servidores para evitar problemas con NFS.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/1f49b9ca-16f1-4b7f-a9ba-30726aba92a0)

Seguidamente, en la carpeta en la que descargamos WordPress, editaremos el archivo **wp-config-sample** con los siguientes datos que utilizaremos para acceder a la base de datos.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/3acb4b39-d3c0-4ee3-beca-c2c1e95a93c8)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/3506fdf9-e803-4e4d-ac19-89d4e31f0331)

 Ahora le asignaremos sus permisos y realizaremos las modificaciones correspondientes.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/5714b586-9492-4882-9906-aecd8884717c)

# Serverweb1y2

Para los servidores web, utilizaremos el siguiente aprovisionamiento:

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/38a5c8ce-9bcb-4e31-a03d-71ac8ffe51a5)


Para montar el directorio anteriormente creado en el NFS, utilizaremos el siguiente comando:

```
sudo mount X.X.X.X:/var/nfs/compartida /var/nfs/compartida
```

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/6ab6c670-11e2-4b78-9f62-2f4c1aa144d6)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/09d3842d-643b-4fac-a828-79db78b1bebe)

Tras realizar el paso anterior, iremos al directorio **/etc/nginx/sites-available** y crearemos una copia del archivo "default".

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/53cd97d1-90c5-4fa5-9483-39da614b8e94)

La cual ahora se editará modificando las siguientes líneas.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/0086b1c5-b3aa-443b-8df7-89a24ad4233e)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/71556a29-a794-475c-b605-f5e4a8bdd20e)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/5cc4b2cc-bd50-4829-8974-e4f8cf54b7dc)

Una vez editado el archivo, crearemos un enlace simbólico a otro directorio con el siguiente comando:
```
sudo ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled.
```

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/9819f352-0d93-4374-9a24-58b97f8640b4)

-----Nota-----
Si luego queremos conectarnos de forma remota al servidor de datos, debemos instalar mysql-client. Para esto, utilizaremos:
```
sudo apt install mysql-client
sudo mysql -h X.X.X.X -u mi_usuario -p
```

# GGBBserver

Este será el aprovisionamiento que se le dará al servidor de datos.


![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/483e447f-dfcd-496b-b4ac-61d0fcf3f851)

Una vez arranquemos la máquina, iremos al directorio **/etc/mysql** y editaremos el archivo "50-server", cambiando en la línea "bind-address" la dirección IP que está por defecto por la de nuestro servidor de datos.

Una vez realizado ese cambio, crearemos la base de datos, el usuario y se le darán los permisos correspondientes para el posterior acceso.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/021cb5cd-7a1d-4ffd-9c6c-1552bd353881)



Para la comprobación, iremos a nuestro navegador en modo incógnito y escribiremos "localhost:9000" en mi caso, y deberá mostrarse una página como la siguiente.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/330bffc6-08ca-470c-ad80-0b15bbd78e72)

Con esto, WordPress estaría instalado y solo faltaría continuar con su instalación desde la página web.



