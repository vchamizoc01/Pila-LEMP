# Pila-LEMP



# Introduccion
En esta practica se realiza una pila LEMP con la siguiente estructura:

Capa 1: Expuesta a red pública. Una máquina con balanceador de carga Nginx.
Capa 2: BackEnd. 
    Dos máquinas con un servidor web nginx cada una.
    Una máquina con un servidor NFS y motor PHP-FPM
Capa 3: Datos. Base de datos MariaDB

Las capas 2 y 3 no estarán expuestas a red pública. Los servidores web utilizarán carpeta compartida por NFS desde el serverNFS y además utilizarán el motor PHP-FPM instalado es una misma máquina.

#Balanceador
Para la configuracion del balanceador usaremos un aprovisionamiento que permitira ahorrar tiempo y trabajo, con el cual instalaremos nginx, lo iniciaremos y ademas eliminaremos un archivo que no usaremos.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/21e4b353-ee85-45dc-b099-d725e3e08f36)

una vez iniciemos la maquina accederemos al directorio **/etc/nginx/sites-enable**
y crearemos un fichero llamado balanceador, en el cual añadiremos el siguiente codigo:
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
Para este servidor realizaremos el siguiente aprovisionamiento para reducir trabajo

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/1cf0e0ef-7b1a-4db5-8e09-f11e8b61a6f8)

una vez tengamos el servidor activo crearemos el siguente directorio

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/1e694aba-bfbc-45a0-a136-35a582db403e)

para despues acceder al fichero **etc/exports** y añadir las siguientes dos linias y que nos permita compartir la carpeta creada anterior mente.
```
/var/nfs/compartir     X.X.X.X(rw,sync,no_root_squash,no_subtree_check)
/var/nfs/compartir     X.X.X.X(rw,sync,no_root_squash,no_subtree_check)
```

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/ca810b77-a0de-4bdd-acc7-160a9d2d8ffe)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/661465d3-7243-477f-915b-ef4ce438b058)

una vez editado el fichero nos moveremos a **/var/nfs/compartir** y descargaremos el siguiente archivo wordpress

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/cae03f31-db06-42b4-8429-d8c9bcdf25fb)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/899e103b-ad73-4cbe-bc55-44de82686296)

ahora habilitaremos los puertos de los servidores para que no haya problemas con NFS

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/1f49b9ca-16f1-4b7f-a9ba-30726aba92a0)

seguidamente en la carpeta en que descargamos wordpress editaremos el fichero wp-config-sample con los siguentes datos que utilizaremos para acceder a la base de datos.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/3acb4b39-d3c0-4ee3-beca-c2c1e95a93c8)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/3506fdf9-e803-4e4d-ac19-89d4e31f0331)

 Ahora le asignaran sus permisos y modificaciones correspondientes.

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/5714b586-9492-4882-9906-aecd8884717c)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/38a5c8ce-9bcb-4e31-a03d-71ac8ffe51a5)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/6ab6c670-11e2-4b78-9f62-2f4c1aa144d6)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/09d3842d-643b-4fac-a828-79db78b1bebe)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/53cd97d1-90c5-4fa5-9483-39da614b8e94)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/0086b1c5-b3aa-443b-8df7-89a24ad4233e)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/71556a29-a794-475c-b605-f5e4a8bdd20e)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/5cc4b2cc-bd50-4829-8974-e4f8cf54b7dc)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/43a5dbe6-7e7e-4c27-9873-5319e4588070)



![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/b18082ee-c70d-42c7-9d20-54057eb7dac3)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/483e447f-dfcd-496b-b4ac-61d0fcf3f851)


![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/021cb5cd-7a1d-4ffd-9c6c-1552bd353881)

![image](https://github.com/vchamizoc01/Pila-LEMP/assets/73099273/08e0da62-08e9-451f-9bde-ad3b0aa6f8ac)
