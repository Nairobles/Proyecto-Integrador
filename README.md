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





Luego nos ubicamos dentro del contenedor correspondiente al servidor de Hive y ejecutamos desdea allí los scripts necesarios:
```
sudo docker exec -it hive-server bash
hive
```
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/1c752bdf-5f57-4d0d-a6cc-b845446bef20)

Este proceso de creación las tablas debe poder ejecutarse desde un shell script.

Para ejecutar un script de Hive, requiere el comando:
```
  hive -f <script.hql>
```
![image](https://github.com/Nairobles/Proyecto-Integrador/assets/155001844/e89e2444-5211-4266-867f-ed67bfb10b47)


## 3) Formatos de Almacenamiento

Las tablas creadas en el punto 2 a partir de archivos en formato csv, deben ser almacenadas en formato Parquet + Snappy.
Tener en cuenta además de aplicar particiones para alguna de las tablas.

## 4) SQL

La mejora en la velocidad de consulta que puede proporcionar un índice tiene el costo del procesamiento adicional para crear el índice y el espacio en disco para almacenar las referencias del índice.
Se recomienda que los índices se basen en las columnas que utiliza en las condiciones de filtrado. El índice en la tabla puede degradar su rendimiento en caso de que no los esté utilizando.
Crear índices en alguna de las tablas cargadas y probar los resultados:

```
CREATE INDEX index_name
 ON TABLE base_table_name (col_name, ...)
 AS index_type
 [WITH DEFERRED REBUILD]
 [IDXPROPERTIES (property_name=property_value, ...)]
 [IN TABLE index_table_name]
 [ [ ROW FORMAT ...] STORED AS ...
 | STORED BY ... ]
 [LOCATION hdfs_path]
 [TBLPROPERTIES (...)]
 [COMMENT "index comment"];
```

Ejemplo:

```
hive> CREATE INDEX index_students ON TABLE students(id) 
 > AS 'org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler' 
 > WITH DEFERRED REBUILD ;
```

ALTER INDEX index_name ON table_name [PARTITION partition_spec] REBUILD;

Ejemplo:
```
hive> ALTER INDEX index_students ON students REBUILD; 
```

DROP INDEX [IF EXISTS] index_name ON table_name;
```
hive> DROP INDEX IF EXISTS index_students ON students; 
```

## 5) No-SQL

Se puede utilizar el entorno docker-compose-v3.yml

#### 1) HBase:

Instrucciones:
```
	1- sudo docker exec -it hbase-master hbase shell

		create 'personal','personal_data'
		list 'personal'
		put 'personal',1,'personal_data:name','Juan'
		put 'personal',1,'personal_data:city','Córdoba'
		put 'personal',1,'personal_data:age','25'
		put 'personal',2,'personal_data:name','Franco'
		put 'personal',2,'personal_data:city','Lima'
		put 'personal',2,'personal_data:age','32'
		put 'personal',3,'personal_data:name','Ivan'
		put 'personal',3,'personal_data:age','34'
		put 'personal',4,'personal_data:name','Eliecer'
		put 'personal',4,'personal_data:city','Caracas'
		get 'personal','4'

	2-En el namenode del cluster:

		hdfs dfs -put personal.csv /hbase/data/personal.csv

	3-sudo docker exec -it hbase-master bash
		
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.separator=',' -Dimporttsv.columns=HBASE_ROW_KEY,personal_data:name,personal_data:city,personal_data:age personal hdfs://namenode:9000/hbase/data/personal.csv
		hbase shell
		scan 'personal'
		create 'album','label','image'
		put 'album','label1','label:size','10'
		put 'album','label1','label:color','255:255:255'
		put 'album','label1','label:text','Family album'
		put 'album','label1','image:name','holiday'
		put 'album','label1','image:source','/tmp/pic1.jpg'
		get 'album','label1'
```		

#### 2) MongoDB

Instrucciones:
```
	1) 	sudo docker cp iris.csv mongodb:/data/iris.csv
		  sudo docker cp iris.json mongodb:/data/iris.json

	2)  sudo docker exec -it mongodb bash

	3) 	mongoimport /data/iris.csv --type csv --headerline -d dataprueba -c iris_csv
		  mongoimport --db dataprueba --collection iris_json --file /data/iris.json --jsonArray

	4) mongosh
		use dataprueba
		show collections
		db.iris_csv.find()
		db.iris_json.find()
	
	5) 	mongoexport --db dataprueba --collection iris_csv --fields sepal_length,sepal_width,petal_length,petal_width,species --type=csv --out /data/iris_export.csv
		mongoexport --db dataprueba --collection iris_json --fields sepal_length,sepal_width,petal_length,petal_width,species --type=json --out /data/iris_export.json
				
	6) 	Descargar desde https://search.maven.org/search?q=g:org.mongodb.mongo-hadoop los jar:
		https://search.maven.org/search?q=a:mongo-hadoop-hive
		https://search.maven.org/search?q=a:mongo-hadoop-spark
		
		sudo docker cp mongo-hadoop-hive-2.0.2.jar hive-server:/opt/hive/lib/mongo-hadoop-hive-2.0.2.jar
		sudo docker cp mongo-hadoop-core-2.0.2.jar hive-server:/opt/hive/lib/mongo-hadoop-core-2.0.2.jar
		sudo docker cp mongo-hadoop-spark-2.0.2.jar hive-server:/opt/hive/lib/mongo-hadoop-spark-2.0.2.jar
		sudo docker cp mongo-java-driver-3.12.11.jar hive-server:/opt/hive/lib/mongo-java-driver-3.12.11.jar
		
	7) 	sudo docker cp iris.hql hive-server:/opt/iris.hql
		sudo docker exec -it hive-server bash

	8) 	hiveserver2
		chmod 777 iris.hql
		hive -f iris.hql
```	
	
### 3) Neo4J
	
	Ejemplo de búsqueda del camino más corto:
		https://neo4j.com/docs/graph-data-science/current/algorithms/dijkstra-source-target/

```	
		CREATE (a:Location {name: 'A'}),
			   (b:Location {name: 'B'}),
			   (c:Location {name: 'C'}),
			   (d:Location {name: 'D'}),
			   (e:Location {name: 'E'}),
			   (f:Location {name: 'F'}),
			   (a)-[:ROAD {cost: 50}]->(b),
			   (b)-[:ROAD {cost: 50}]->(a),
			   (a)-[:ROAD {cost: 50}]->(c),
			   (c)-[:ROAD {cost: 50}]->(a),
			   (a)-[:ROAD {cost: 100}]->(d),
			   (d)-[:ROAD {cost: 100}]->(a),
			   (b)-[:ROAD {cost: 40}]->(d),
			   (d)-[:ROAD {cost: 40}]->(b),
			   (c)-[:ROAD {cost: 40}]->(d),
			   (d)-[:ROAD {cost: 40}]->(c),
			   (c)-[:ROAD {cost: 80}]->(e),
			   (e)-[:ROAD {cost: 80}]->(c),
			   (d)-[:ROAD {cost: 30}]->(e),
			   (e)-[:ROAD {cost: 30}]->(d),
			   (d)-[:ROAD {cost: 80}]->(f),
			   (f)-[:ROAD {cost: 80}]->(d),
			   (e)-[:ROAD {cost: 40}]->(f),
			   (f)-[:ROAD {cost: 40}]->(e);
			   
		CALL gds.graph.project(
			'miGrafo',
			'Location',
			'ROAD',
			{
				relationshipProperties: 'cost'
			}
		)

		MATCH (l:Location) RETURN l
					
		MATCH (source:Location {name: 'A'}), (target:Location {name: 'E'})
		CALL gds.shortestPath.dijkstra.write.estimate('miGrafo', {
			sourceNode: source,
			targetNode: target,
			relationshipWeightProperty: 'cost',
			writeRelationshipType: 'PATH'
		})
		YIELD nodeCount, relationshipCount, bytesMin, bytesMax, requiredMemory
		RETURN nodeCount, relationshipCount, bytesMin, bytesMax, requiredMemory

		MATCH (source:Location {name: 'A'}), (target:Location {name: 'E'})
		CALL gds.shortestPath.dijkstra.stream('miGrafo', {
			sourceNode: source,
			targetNode: target,
			relationshipWeightProperty: 'cost'
		})
		YIELD index, sourceNode, targetNode, totalCost, nodeIds, costs, path
		RETURN
			index,
			gds.util.asNode(sourceNode).name AS sourceNodeName,
			gds.util.asNode(targetNode).name AS targetNodeName,
			totalCost,
			[nodeId IN nodeIds | gds.util.asNode(nodeId).name] AS nodeNames,
			costs,
			nodes(path) as path
		ORDER BY index
```	

  Ejemplo de logística:
    https://neo4j.com/docs/graph-data-science/current/alpha-algorithms/minimum-weight-spanning-tree/

```	
		MATCH (n:Location {name: 'A'})
		CALL gds.alpha.spanningTree.minimum.write('miGrafo', {
		  startNodeId: id(n),
		  relationshipWeightProperty: 'cost',
		  writeProperty: 'MINST',
		  weightWriteProperty: 'writeCost'
		})
		YIELD preProcessingMillis, computeMillis, writeMillis, effectiveNodeCount
		RETURN preProcessingMillis, computeMillis, writeMillis, effectiveNodeCount;		

		MATCH path = (n:Location {name: 'A'})-[:MINST*]-()
		WITH relationships(path) AS rels
		UNWIND rels AS rel
		WITH DISTINCT rel AS rel
		RETURN startNode(rel).name AS source, endNode(rel).name AS destination, rel.writeCost AS cost
		
		MATCH (n) DETACH DELETE n

		sudo docker cp producto.csv neo4j:/var/lib/neo4j/import/producto.csv
		sudo docker cp tipo_producto.csv neo4j:/var/lib/neo4j/import/tipo_producto.csv
		sudo docker cp cliente.csv neo4j:/var/lib/neo4j/import/cliente.csv
		sudo docker cp venta.csv neo4j:/var/lib/neo4j/import/venta.csv
		
		Ver Archivo "ejemploNeo4J.txt"		
```	

#### 4) Zeppelin

		HDFS:
		En la máquina anfitrión probar WebHDFS:
			curl "http://<IP_Anfitrion>:9870/webhdfs/v1/?op=LISTSTATUS"
		En el interpreter:
			En la parte de "file"
				Variable hdfs.url = http://<IP_Anfitrion>:9870/webhdfs/v1/
		En nuevo notebook / nueva nota:
			%file
			ls /

		Neo4J:
		En el interpreter
			En la parte de "neo4J"
				Variables 
					neo4J.url = http://<IP_Anfitrion>:7687
					neo4j.auth.user	= neo4j
					neo4j.auth.password	= zeppelin

## 6) Spark

Se pueden utilizar los entornos docker-compose-v4.yml y docker-compose-kafka.yml

### 1) Spark y Scala:

Ubicarse en la línea de comandos del Spark master y comenzar PySpark.
```
  docker exec -it spark-master bash
  /spark/bin/pyspark --master spark://spark-master:7077
```

Cargar raw-flight-data.csv desde HDFS.
```
	from pyspark.sql.types import *

	flightSchema = StructType([
	StructField("DayofMonth", IntegerType(), False),
	StructField("DayOfWeek", IntegerType(), False),
	StructField("Carrier", StringType(), False),
	StructField("OriginAirportID", IntegerType(), False),
	StructField("DestAirportID", IntegerType(), False),
	StructField("DepDelay", IntegerType(), False),
	StructField("ArrDelay", IntegerType(), False),
	]);

	flights = spark.read.csv('hdfs://namenode:9000/data/flights/raw-flight-data.csv', schema=flightSchema, header=True)
  
  	flights.show()
	  +----------+---------+-------+---------------+-------------+--------+--------+
|DayofMonth|DayOfWeek|Carrier|OriginAirportID|DestAirportID|DepDelay|ArrDelay|
+----------+---------+-------+---------------+-------------+--------+--------+
|        19|        5|     DL|          11433|        13303|      -3|       1|
|        19|        5|     DL|          14869|        12478|       0|      -8|
|        19|        5|     DL|          14057|        14869|      -4|     -15|
|        19|        5|     DL|          15016|        11433|      28|      24|
|        19|        5|     DL|          11193|        12892|      -6|     -11|
|        19|        5|     DL|          10397|        15016|      -1|     -19|
|        19|        5|     DL|          15016|        10397|       0|      -1|
|        19|        5|     DL|          10397|        14869|      15|      24|
|        19|        5|     DL|          10397|        10423|      33|      34|
|        19|        5|     DL|          11278|        10397|     323|     322|
|        19|        5|     DL|          14107|        13487|      -7|     -13|
|        19|        5|     DL|          11433|        11298|      22|      41|
|        19|        5|     DL|          11298|        11433|      40|      20|
|        19|        5|     DL|          11433|        12892|      -2|      -7|
|        19|        5|     DL|          10397|        12451|      71|      75|
|        19|        5|     DL|          12451|        10397|      75|      57|
|        19|        5|     DL|          12953|        10397|      -1|      10|
|        19|        5|     DL|          11433|        12953|      -3|     -10|
|        19|        5|     DL|          10397|        14771|      31|      38|
|        19|        5|     DL|          13204|        10397|       8|      25|
+----------+---------+-------+---------------+-------------+--------+--------+
only showing top 20 rows
  	flights.describe()
```

Ubicarse en la línea de comandos del Spark master y comenzar Scala.
```
  docker exec -it spark-master bash
  spark/bin/spark-shell --master spark://spark-master:7077
```

Cargar raw-flight-data.csv desde HDFS.
```
	case class flightSchema(DayofMonth:String, DayOfWeek:String, Carrier:String, OriginAirportID:String, DestAirportID:String, DepDelay:String, ArrDelay:String)
	val flights = spark.read.format("csv").option("sep", ",").option("header", "true").load("hdfs://namenode:9000/data/flights/raw-flight-data.csv").as[flightSchema]

  	flights.show()

+----------+---------+-------+---------------+-------------+--------+--------+
|DayofMonth|DayOfWeek|Carrier|OriginAirportID|DestAirportID|DepDelay|ArrDelay|
+----------+---------+-------+---------------+-------------+--------+--------+
|        19|        5|     DL|          11433|        13303|      -3|       1|
|        19|        5|     DL|          14869|        12478|       0|      -8|
|        19|        5|     DL|          14057|        14869|      -4|     -15|
|        19|        5|     DL|          15016|        11433|      28|      24|
|        19|        5|     DL|          11193|        12892|      -6|     -11|
|        19|        5|     DL|          10397|        15016|      -1|     -19|
|        19|        5|     DL|          15016|        10397|       0|      -1|
|        19|        5|     DL|          10397|        14869|      15|      24|
|        19|        5|     DL|          10397|        10423|      33|      34|
|        19|        5|     DL|          11278|        10397|     323|     322|
|        19|        5|     DL|          14107|        13487|      -7|     -13|
|        19|        5|     DL|          11433|        11298|      22|      41|
|        19|        5|     DL|          11298|        11433|      40|      20|
|        19|        5|     DL|          11433|        12892|      -2|      -7|
|        19|        5|     DL|          10397|        12451|      71|      75|
|        19|        5|     DL|          12451|        10397|      75|      57|
|        19|        5|     DL|          12953|        10397|      -1|      10|
|        19|        5|     DL|          11433|        12953|      -3|     -10|
|        19|        5|     DL|          10397|        14771|      31|      38|
|        19|        5|     DL|          13204|        10397|       8|      25|
+----------+---------+-------+---------------+-------------+--------+--------+
only showing top 20 rows
```

#### 2) Kafka		
```		
			sudo docker-compose up -d
			sudo docker exec -it kafka_container bash
			cd /opt/kafka/bin
			sh kafka-topics.sh --create --bootstrap-server kafka:9092 --replication-factor 1 --partitions 100 --topic demo
			sh kafka-topics.sh --list --bootstrap-server kafka:9092
			sh kafka-topics.sh --describe --bootstrap-server kafka:9092 --topic demo 
			sh kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic demo --from-beginning
			sh kafka-console-producer.sh --broker-list localhost:9092 --topic demo
				Escribir desde la consola del productor "Esto es una Prueba 1" y enviar.
				
			Acceder a <IP_Anfitrion>:9000	
	
			Desde Scala:
			val df = spark.readStream
					.format("kafka")
					.option("kafka.bootstrap.servers", "192.168.1.100:9092")
					.option("subscribe", "json_topic")
					.option("startingOffsets", "earliest") // From starting
					.load()

			df.printSchema()
			
			Más ejemplos:
				https://github.com/dbusteed/kafka-spark-streaming-example
						
			Otra forma de ejecutar:
			docker-compose exec kafka kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic TenMinPsgCnts --from-beginning
```	

#### 3) Comparativa Dataset y Dataframe en Scala:

```	
    sudo docker cp pruebaPySpark.py spark-master:pruebaPySpark.py
    sudo docker cp pruebaScala.scala spark-master:pruebaScala.scala

		sudo docker exec -it spark-master bash
		
		/spark/bin/spark-submit --master spark://spark-master:7077 pruebaPySpark.py
		/spark/bin/spark-shell --master spark://spark-master:7077 -i pruebaScala.scala
		
		/spark/bin/pyspark --master spark://spark-master:7077
		/spark/bin/spark-shell --master spark://spark-master:7077
```	

#### 4) ETL con Spark

A partir de la tabla venta generada en Parqet, realizar el proceso de filtrado de valores outliers utilizando Spark.

## 7) Carga incremental con Spark 

Ahora resta evaluar qué sucede cuando en los sistemas fuente, se genere más dato, es decir, siguiendo los datos de esta práctica, qué pasa cuando se carguen más ventas. Se debería tomar las novedades e ingestar en el modelo existente cada día, de modo que la tabla venta, irá creciendo en cantidad de registro de manera diaria.
Para este fin, se provee un script en spark que realiza la generación de nuevas ventas, de manera aleatoria, para poder crear una situación, donde se cuenta con novedades para la tabla de venta. El script "Paso06_GeneracionVentasNuevasPorDia.py" utiliza los datasets provistos en la carpeta "Datasets\data_nvo" para generar las novedades de forma automática. Revisar la variable "fecha_nvo" que contiene la fecha para la que se quiere generar información, como tenemos datos hasta el año 2020, la fecha de ejemplo tomada es '2021-01-01'.
Es necesiario entonces generar, un script tal que tome las novedades en csv, y las cargue al modelo.

```	
	sudo docker exec -it spark-master /spark/bin/spark-submit --master spark://spark-master:7077 /home/Paso06_GeneracionVentasNuevasPorDia.py
```	
Supongamos que tenemos nuestro script, y ahora se quiere programar su ejecución:
```	
	/spark/bin/spark-submit --master spark://spark-master:7077 Paso06_IncrementalVentas.py
```	

Con crontab, para que ejecute cada día a las 5 AM:
```
	$ crontab -e
	5	0	*	*	*	/home/CargaIncremental.sh

	$ crontab -l
```	

## 8) Herramientas de orquestación de flujos de datos

https://github.com/sercasti/datalaketools
