## Estimación de recursos y costos

Realizaremos una estimación para poner en producción la app PetClinic, teniendo en cuenta lo siguiente:
* Debe estar disponible siempre (24/7).
* Debe soportar 10000 usuarios concurrentes.
* Los usuarios finales están en Alemania.

Antes de hacer una estimación de recursos y costos para llevar a la aplicación PetClinic a producción con Azure, miremos qué tipo de aplicación tenemos. La aplicación PetClinic es una aplicación web de Java, usando Spring Boot. Esta aplicación usa el patrón de arquitectura MVC (por lo tanto, es monolítica), así que no tendremos qué preocuparnos por separar un backend de un frontend. Con respecto a la base de datos, tenemos una base de datos en memoria (también está integrada a la aplicación web), pero en la documentación se muestra cómo separar (usando Docker) la base de datos, ya sea con MySQL o PostgreSQL. Para este caso estimaremos una base de datos PostgreSQL.

Teniendo en cuenta lo anterior, los dos artefactos que necesitaríamos serían los siguientes:
* Aplicación web de Java, con Spring Boot.
* Base de datos de PostgreSQL.

Para calcular los costos de los recursos usaremos la herramienta [**Pricing Calculator**](https://azure.microsoft.com/es-es/pricing/calculator/) de Azure.
Vamos a usar la contenerización para hacer que la cobertura y el crecimiento de la disponibilidad de la aplicación sea escalable de forma eficiente. Como la aplicación está pensada para Alemania, podemos elegir entre dos regiones: Germany North y **Germany West Central**. Elegiremos Germany West Central porque es mucho más económica.

Los recursos a usar serían los siguientes:

* **Azure Kubernete Services (AKS):** Hemos estimado que para una app de Java con Spring Boot que no tiene muchos controladores y funcionalidades (como PetClinic), el consumo de RAM de cada usuario por unidad de tiempo realizando operaciones podría ser de aproximadamente 1MB. Esto quiere decir que si tenemos 10000 usuarios concurrentes, tendríamos que soportar 10000MB, que corresponden a 10GB de RAM. No hay máquinas virtuales (nodos) que tengan exactamente 10GB de RAM, así que tendríamos que usar una de 16GB (dejaríamos libres 6GB de RAM, sin embargo hay que tener en cuenta que el Sistema Operativo consume de esta RAM también). Para este caso tendríamos que optar por una máquina virtual **A2m v2**, que contienen una CPU de 2 núcleos, 16GB de RAM y almacenamiento de 20GB (para almacenar los contenedores), que tiene un costo de 0,124 US$ por hora. Usando el nivel estándar y el método de pago por uso, tendríamos un costo mensual (sumado el costo del clúster) de **163,52 US$**.
* **Azure Container Registry (ACR):** Para almacenar y gestionar las imágenes de los contenedores de la aplicación y la base de datos, necesitaríamos este servicio. Con 1 registro por mes, con nivel estándar y la región indicada anteriormente, este servicio costaría **20,00 US$** al mes.
* **Load Balancer:** Se necesita para distribuir las peticiones en las diferentes aplicaciones (pods) en las máquinas virtuales (nodos). Teniendo 5 reglas y 1000GB de datos procesados, este servicio tendría un costo mensual de **23,25 US$**.
* **Azure Database for PostgreSQL:** Se requiere 1 servidor para la base de datos, de uso general, con 2 núcleos virtuales (0,2084 US$ por hora). Con 5GB de almacenamiento inicial y 100GB para realizar backups, este servicio costaría **164,72 US$** al mes.

En total tendríamos que destinar **371,49 US$** dólares al mes para mantener la aplicación en producción.
