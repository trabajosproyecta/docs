# Levantar Nueva Aplicación con Nginx y Cloudflare

A menudo queremos publicar nuestras nuevas aplicaciones para poder sacar el máximo provecho de lo que hacemos y que se vea reflejado nuestro trabajo.

Para poder realizarlo, debemos cumplir una serie de requisitos:
- Tener una aplicación desarrollada
- Tener acceso a un servidor con un webserver
- Tener acceso a un dominio
- Tener acceso a la configuración de los DNS referentes al dominio

Para este caso específico, usaremos un servidor con **Nginx** de webserver y [Cloudflare](https://www.cloudflare.com/) como administrador de configuraciones de DNS. Levantaremos una aplicación expuesta en el puerto 5000 y accederemos a ella desde http://subdominio.trabajosproyecta.cl

Con todo esto solo basta seguir los siguientes pasos
1. **Subir aplicación al servidor**
    Lo primero que debemos hacer es conectarnos al servidor para poder descargar los archivos y preparar nuestra aplicación en modo de producción.
    Para esto iremos a la terminal y nos conectaremos via **ssh**. 
    ```bash
    $ ssh <user>@<server>
    ```
    Aca reemplazamos **&lt;user&gt;** por nuestro usuario y **&lt;server&gt;** por la IP o dominio dirijido a la IP de nuestro servidor.
    Luego tenemos que descargar la aplicación. Dado que usamos github, basta clonarla usando `git clone` en el directorio deseado, para luego entrar a el y usar los comandos de instalación necesarios (en caso de que no usemos docker por ejemplo).
2. **Iniciar Aplicación**
    Teniendo ya los archivos necesarios y la instalación completada, podemos levantar nuestra aplicación. 
    > Es muy importante que se declaren todas las variables de entorno necesarias para su correcta ejecución, y que ejecutemos los comandos con flags indicados para producción.
    
    A modo de ejemplo, usaré docker para dejar corriendo la aplicación. Esto es con
    ```bash
    docker run -d -p 5000:3000 --name <name> <image>
    ```
    En este caso estamos corriendo la imagen **&lt;image&gt;** asignandole el nombre **&lt;name&gt;** para posteriores referencias. Cabe destacar que esta imagen usa el puerto **3000** internamente, pero que para el exterior es accesible en el puerto **5000** (este es el que nos importa).
    Para probar, podemos acceder a la IP del servidor con el puerto: http://192.168.0.1:5000 *(acá reemplazamos 192.168.0.1 por la IP de nuestro servidor)*
3. **Configurar Nginx**
    Ahora debemos habilitar un servidor virtual dentro de Nginx. Para esto accedemos a sus configuraciones
    ```bash
    $ cd /etc/nginx
    ```
    En esta carpeta encontramos una serie de archivos relacionados a la configuracione de Nginx, pero para ahora los unicos importantes son **sites-available** y **sites-enabled**. Como sus nombres lo indican, en el primero están las configuraciones de todos los sitios disponibles, y en el segundo de todos los sitios habilitados.
    Como buena práctica, lo que haremos es crear un archivo en *sites-available* con el mismo nombre del subdominio que usa y luego crearemos un *[Symbolic Link](https://en.wikipedia.org/wiki/Symbolic_link)* en *sites-enabled*.
    Primero creamos el archivo
    ```bash
    $ sudo touch ./sites-available/subdominio
    ```
    Luego lo abrimos con nuestro editor de preferencia, como `sudo nano ./sites-available/subdominio` y escribimos lo siguiente:
    ```nginx
    server {
        listen 80;
        server_name subdominio.trabajosproyecta.cl;
    
        location / {
            proxy_pass http://localhost:5000;
        }
    }
    ```
    Con esto estamos diciendo algo similar a:
    > Escucha todo lo que venga al puerto 80 del servidor. Si es que el llamado viene como subdominio.trabajosproyecta.cl dirije todo hacia el puero 5000 local.
    
    Ahora debemos crear el link simbolico:
    ```bash
    $ cd sites-enabled
    $ ln -s ../sites-available/subdominio subdominio
    ```
    Esto es entrar a la carpeta (o el link sumbolico no va a funcionar correctamente) y luego crear un link simbolico suave donde apuntaremos a **../sites-available/subdominio** con el nombre de **subdominio**.
    Finalmente reiniciamos el servidor para que se consideren los cambios realizados
    ```bash
    $ sudo service nginx restart
    ```
4. **Configurar DNS**
    Para esto [entraremos a nuestra cuenta de Cloudflare](https://www.cloudflare.com/a/login), e iremos a la opción **DNS** ubicada en el menu superior (tercer elemento del menu).
    ![dnsConfig](https://github.com/trabajosproyecta/docs/blob/master/devops/dnsConfig.png?raw=true)
    En este momento, se debe ver el formulario que aparece en el imágen.
    Para esto, completamos el campo de name con **subdominio** y el campo de IPv4 address con la **IP del servidor**
    > También podemos usar **@** en caso de que la ip sea la misma que el servidor principal. Para ver cual es, basta bajar en la misma página donde aparecen todos las configuraciones, y buscar la que tenga de nombre el dominio, en este caso **trabajosproyecta.cl**

    
Luego de esto, si accedemos a subdominio.trabajosproyecta.cl se verá el mismo contenido que en http://192.168.0.1:5000 que habiamos probado antes.
