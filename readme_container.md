## Descargar imagen y ejecutar la aplicación contenerizada
Si se prefiere evitar una instalación manual como la anterior, y que sea agnóstica al Sistema Operativo, puede obtarse por usar la imagen generada de la aplicación y crear su respectivo contenedor.

### Requisitos
* Docker (Docker Desktop, o Docker Engine en su defecto) ([descargar última versión](https://docs.docker.com/desktop/))

### Pasos a seguir

1. **Verificar la instalación de Docker**: Necesitarás tener Docker instalado en tu equipo (Docker Desktop o mínimamente Docker Engine para poder realizar los pasos desde la terminal). Verifica cómo instalar Docker Desktop (incluye el Docker Engine) o Docker Engine en la [documentación oficial](https://docs.docker.com/desktop/) Puedes verificar la instalación rápidamente con el comando `docker --version` en una terminal de comandos (en Windows `CMD`, `Símbolo del sistema`, `Windows Terminal` o `Command Prompt`). Si el mensaje que visualizas muestra la versión de Docker, quiere decir que al menos Docker Engine está instalado correctamente.

2. **Instalar la última imagen generada:** En la sección o página de [Paquetes](https://github.com/AdrianPerdomoE/hello-webapp-230824114908/pkgs/container/petclinic_adrian_alejandro), se puede ver la última versión de imagen generada (y las versiones anteriores). Se recomienda siempre usar la última versión, entonces para ello, copia el comando que aparece en el primer recuadro de la página, donde dice **Install from the command line**. El comando que aparece, es de este tipo: 
    ```sh
    docker pull ghcr.io/adrianperdomoe/petclinic_adrian_alejandro:v<version_number>
    ```
    Donde `<version_number>` corresponde al número de la versión de la imagen generada. Se reitera que lo más recomendable es instalar siempre la última versión disponible. Al ejecutar este comando en una terminal de comandos, se debe instalar en Docker (la máquina virtual de Linux creada en el sistema al instalar Docker) la imagen correspondiente a la aplicación. Para confirmar que se instaló correctamente, ejecuta el comando `docker images`, que hará que se muestre un listado de las imágenes que están alojadas en tu equipo (máquina virtual Linux). Si se muestra la imagen con REPOSITORY como `pymeya_web_app_adrian_alejandro:v<version_number>` quiere decir que la imagen se instaló correctamente. Es importante que copies el **IMAGE ID**, ya que será necesario para el siguiente paso.

3. **Crear el contenedor a partir de la imagen:** Hay dos formas de crear el contenedor a partir de la imagen que ya está instala. La primera es por medio de la interfaz gráfica de Docker Desktop, y la segunda es que se indicará aquí, que es por medio de la terminal de comandos. Deberás ejecutar el siguiente comando para crear el contenedor:
    ```sh
    docker run -d --name <container_name> -p <port>:8080 <image_id>
    ```
    Donde `<container_name>` es un nombre cualquiera que le puedes dar al contenedor a crear y ejecutar. Si omites la opción `--name <container_name>` se asignará un nombre aleatorio al contenedor. `<port>` es el puerto donde quieres que esté escuchando la aplicación contenerizada (ten en cuenta usar un puerto que esté disponible en tu equipo), e `<image_id>` es el **IMAGE ID** que viste y debiste copiar en el paso anterior para que Docker sepa a partir de qué imagen va a crear el contenedor. La opción `-d` es para que el comando de creación y ejecución del contenedor se ejecute en segundo plano, y no quede ligado a la consola. Si quitas esta opción, deberás usar otra consola para realizar las demás tareas, ya que si finalizas esa consola o el proceso, el contenedor finalizará su ejecución. Si todo se ejecuta correctamente, debería mostrar un código, al ejecutarlo en segundo plano, o en el otro caso se mostraría las operaciones respectivas que se estén ejecutando en el ciclo de vida de la ejecución de la aplicación contenerizada.

4. **Verificar que la aplicación contenerizada funciona correctamente:** Para verificar que la aplicación está funcionando correctamente, usa un navegador web y visita la ruta `http://localhost:<port>` o `http://127.0.0.1:<port>`. Recuerda que `<port>` es el puerto que especificaste al crear el contenedor en el paso anterior. Si se muestra la interfaz de PymeYa, ¡listo, ya puedes usar tu instancia de la aplicación PymeYa!

5. **Otros comandos:** Puedes hacer uso de otros comandos cuando los requieras:
  * **Ver contenedores en ejecución**
    ```sh
    docker ps
    ```
  * **Ver todos los contenedores creados**
    ```sh
    docker ps -a
    ```
  * **Finalizar la ejecución de un contenedor**
    ```sh
    docker stop <container_name>
    ```
    Donde `<container_name>` es el nombre del contenedor.
  * **Ejecutar contenedor ya creado**
    ```sh
    docker start <container_name>
    ```
    Donde `<container_name>` es el nombre del contenedor.
  * **Eliminar contenedor**
    ```sh
    docker remove <container_name>
    ```
    Donde `<container_name>` es el nombre del contenedor.
  * **Eliminar imagen**
    ```sh
    docker rmi <image_id>
    ```
    Donde `<image_id>` es el ID de la imagen.
