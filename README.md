# Big Data
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/456ba8fc-ec3d-4f67-8a95-0ccfc2545c67)


El objetivo principal de este trabajo es simular un ambiente de trabajo donde se nos solicita, desde un área de innovación, construir un Producto Viable Minimo(MVP) de un ambiente de Big Data. 
El MVP es una metodología de desarrollo de productos o servicios que se centra en crear un producto con el mínimo conjunto de características necesarias para validar su viabilidad en el mercado y obtener retroalimentación de los usuarios lo antes posible. La idea detrás de este procedimiento es lanzar algo lo suficientemente funcional como para que los usuarios puedan interactuar con él y proporcionar comentarios y datos de uso, pero sin gastar recursos excesivos en el desarrollo de características complejas que pueden no ser necesarias o relevantes para los usuarios.

Para lograr este objetivo debemos trasladar archivos CSV (que anteriormente se utilizaban en un Datawarehouse en MySQL) a un entorno de **Hadoop**.

Se debe tener en cuenta que la gerencia de Infraestructura no está totalmente convencida de utilizar esta tecnología, por lo tanto no ha asignado presupuesto alguno para esta iniciativa. Es por ello que todo el MVP se deberá implementar utilizando **Docker** de forma tal que se pueda hacer una demo al sector de infraestructura mostrando las ventajas de utilizar tecnologías de Big Data.

Nos vamos a encontrar trabajando con un entorno Docker con Hadoop(HDFS) e implementando las siquientes herramientas:
* *Spark*: Apache Spark es un framework de computación en clúster de código abierto que proporciona un **potente motor de procesamiento de datos y análisis en memoria**. Se utiliza para realizar análisis de datos de gran escala, procesamiento de datos en tiempo real, procesamiento de lotes, aprendizaje automático y más. Spark es conocido por su velocidad y capacidad de procesamiento distribuido.

* *Hive*: Apache Hive es una infraestructura de **almacenamiento** y procesamiento de datos construida sobre Hadoop que facilita el análisis de grandes conjuntos de datos utilizando un lenguaje **similar a SQL** llamado HiveQL. Hive permite a los usuarios consultar, analizar y procesar datos almacenados en Hadoop **HDFS (Sistema de Archivos Distribuido de Hadoop)** de manera eficiente y escalable.

* *HBase*: Apache HBase es una **base de datos NoSQL distribuida y escalable** diseñada para proporcionar acceso aleatorio y en tiempo real a grandes volúmenes de datos no estructurados o semi-estructurados. HBase se ejecuta sobre Hadoop HDFS y es adecuado para aplicaciones que requieren una alta disponibilidad y rendimiento para operaciones de lectura y escritura.

* *MongoDB*: MongoDB es una base de datos NoSQL de código abierto y **orientada a documentos** que se utiliza para almacenar datos en formato JSON-like (BSON). MongoDB es conocido por su escalabilidad, flexibilidad y capacidad para manejar datos semi-estructurados o no estructurados. Es ampliamente utilizado en aplicaciones web, móviles y de Internet de las Cosas (IoT).

* *Neo4J*: Neo4j es una **base de datos de grafos** de código abierto diseñada para almacenar y consultar datos altamente conectados, como redes sociales, recomendaciones de productos, sistemas de recomendación y análisis de redes. Neo4j utiliza un modelo de datos de grafo que consiste en nodos, relaciones y propiedades para representar y consultar relaciones complejas entre entidades.

* *Zeppelin*: Apache Zeppelin es un cuaderno de código abierto basado en web que **permite a los usuarios crear y compartir documentos interactivos que contienen código, visualizaciones y narrativas.** Zeppelin admite varios lenguajes de programación, incluidos Scala, Python, SQL y R, lo que lo hace útil para la **exploración de datos, el análisis de datos y la creación de informes interactivos**.

* *Kafka*: Apache Kafka es una **plataforma de streaming** de código abierto que se utiliza para la ingestión, el almacenamiento y el procesamiento de datos en tiempo real a gran escala. Kafka es conocido por su **alta velocidad, durabilidad y capacidad de escala horizontal.** Se utiliza en una amplia variedad de aplicaciones, como la ingesta de registros, el procesamiento de eventos en tiempo real, la integración de sistemas y la replicación de datos.

Si no estas interiorizado en el mundo de la Big Data, te invito a leer el siguiente articulo:
https://www.powerdata.es/big-data

## ¡Comencemos! ##

1)El primer paso será dar inicio a nuestra maquina virtual y posicionarnos en la consola de Putty donde ejecutaremos los siguientes comandos:

Clonar el repositorio:
```
git clone https://github.com/Nairobles/Proyecto-Integrador.git
```
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/62d00604-c9de-4ddf-aea2-498e793d3ff2)

Nos ubicamos en el directorio 'Proyecto-Integrador':
```
cd Proyecto-Integrador
```
Y ejecutamos el siguiente comando que permite dar inicio y ejecución de los servicios en contenedores de Docker:

```
sudo docker-compose -f docker-compose-vX.yml up -d
```
*Nota: En este caso reemplazaremos la 'X' de 'vX' por el valor '1' para dar inicio a la practica pero van a ir adquiriendo nuevos valores: 2, 3 y 4 ya que los mismos hacen referencia a distintos entornos.*
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/7377c84c-6515-4711-a444-11dd9000c146)

## Copia de archivos
2) En este punto nos vamos a encontrar trabajando con un sistema de archivos distribuido diseñado específicamente para manejar grandes volúmenes de datos. Recibe el nombre de **HDFS** y es el componente de almacenamiento distribuido central de Hadoop.

Como mencioné en el paso anterior, vamos a utilizar el entorno 'docker-compose-v1.yml' para dar inicio:

```
sudo docker-compose -f docker-compose-v1.yml up -d
```
Vamos a acceder a una sesión interactiva dentro del contenedor Docker "namenode", específicamente ejecutando el shell Bash dentro de él.
```
  sudo docker exec -it namenode bash
```
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/f9b3fc27-01a0-43f9-8cb6-68762f96c6af)

Volvemos al directorio 'home'
```
  cd home
```

El comando "mkdir Datasets"(mkdir= make directory) se utiliza para crear un nuevo directorio (carpeta) llamado "Datasets" en el directorio actual.
```
mkdir Datasets
```
Salimos de la sesión interactiva de shell
```
exit
```
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/d2708426-4bd8-45e2-ad2b-8a30abd72a3e)

Y finalmente copiamos los archivos ubicados en la carpeta Datasets dentro del contenedor "namenode":

Podemos copiarlos uno por uno, de la carpeta Datasets, con el siguiente comando:
```
  sudo docker cp <path><archivo> namenode:/home/Datasets/<archivo>
```
Aqui los comandos:
```
sudo docker cp Datasets/canaldeventa/CanalDeVenta.csv namenode:/home/Datasets/CanalDeVenta.csv
sudo docker cp Datasets/calendario/Calendario.csv namenode:/home/Datasets/Calendario.csv
sudo docker cp Datasets/cliente/Cliente.csv namenode:/home/Datasets/Cliente.csv
sudo docker cp Datasets/compra/Compra.csv namenode:/home/Datasets/Compra.csv
sudo docker cp Datasets/empleado/Empleado.csv namenode:/home/Datasets/Empleado.csv
sudo docker cp Datasets/gasto/Gasto.csv namenode:/home/Datasets/Gasto.csv
sudo docker cp Datasets/producto/Producto.csv namenode:/home/Datasets/Producto.csv
sudo docker cp Datasets/proveedor/Proveedor.csv namenode:/home/Datasets/Proveedor.csv
sudo docker cp Datasets/sucursal/Sucursal.csv namenode:/home/Datasets/Sucursal.csv
sudo docker cp Datasets/tipodegasto/TiposDeGasto.csv namenode:/home/Datasets/TiposDeGasto.csv
sudo docker cp Datasets/venta/Venta.csv namenode:/home/Datasets/Venta.csv
sudo docker cp Datasets/data_nvo/Cliente.csv namenode:/home/Datasets/data_nvo
sudo docker cp Datasets/data_nvo/Empleado.csv namenode:/home/Datasets/data_nvo
sudo docker cp Datasets/data_nvo/Producto.csv namenode:/home/Datasets/data_nvo
```

Nos ubicamos en el contenedor "namenode":

```
sudo docker exec -it namenode bash
```

Creamos un directorio en HDFS llamado "/data":

```
hdfs dfs -mkdir -p /data
```

Finalmente copiamos los archivos csv provistos a HDFS:
```
  hdfs dfs -put /home/Datasets/* /data
```
Para asegurarnos que los archivos se han copiado correctamente en el HDFS, ingresamos a la interfaz de hadoop a través nuestro navegador:

Ingresamos la IP de nuestra máquina virtual y luego el puerto :9870 :
 
 ![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/d2482ea0-fa14-4e05-8722-7152d2a14c14)

Luego ingresamos a la solapa "Utilities" y seleccionamos la opción "Browse the fyle system":

![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/06b848a1-d160-4c44-b124-d11b34c06c41)

Ingresamos a "data":

![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/707eb590-caea-4e39-811a-a21c516b7cfa)

Y nos encontramos con todos los datos copiados:

![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/0eb5a14e-c021-41c8-95cf-5bd1bd614374)


## Creación y población de tablas

3)Para realizar este paso vamos a utilizar la herramienta **Hive** y el entorno docker-compose-v2.yml.

La creación y población de tablas se va a dar a partir de los CSV ingestados en HDFS.

Primero nos ubicamos en el entorno correspondiente:
```
sudo docker-compose -f docker-compose-v2.yml up -d
```
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/7d42ef75-d3a0-4d71-8c6c-57b0b4e40b84)


Para crear las tablas vamos a copiar el script del archivo Paso02.hql en /home del contenedor hive-server:
```
docker cp Paso02.hql hive-server:/home/
```

![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/7fd21c22-c8a6-48f3-9c18-216fa3c8af6a)

Y luego nos ubicamos en el contenedor hive-server e iniciamos una sesión interactiva para ejecutar un shell Bash dentro de él:
```
sudo docker exec -it hive-server bash
```

Ahora ejecutamos el script de Hive almacenado en un archivo Paso02.hql:
```
hive –f /home/Paso02.hql
```
*Nota: Para ejecutar un script de Hive se requiere el comando "hive -f <script.hql>" ya que "-f" es la opción que se utiliza para especificar que se va a ejecutar un script almacenado en un archivo*

![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/7a6f5979-3f05-474d-882d-ae37a7cd2939)

Para chequear la información ingresamos a hive desde el shell Bash:
```
hive
```
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/7eddd18d-8b44-467c-80f5-0cff84ebb21c)

Consultamos que exita la base de datos:
```
hive > SHOW DATABASES;
```
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/648b8a51-c34d-40b8-a9e6-c8becccc0ed2)

Ahora consultamos las tablas usando la base de datos 'integrador':
```
hive > use integrador;
hive > show TABLES;
```
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/4b9d2cdc-8e77-4bca-847b-2f334a77e2af)

*Nota: el "hive >" esta escrito para entender que nos encontramos dentro de este Shell bash, no se lo debe escribir*
