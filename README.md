# Proyecto: Azure Synapse Analytics Tutorial

## Descripción
Este repositorio contiene un tutorial completo sobre cómo comenzar con Azure Synapse Analytics, incluyendo la creación de un área de trabajo, la configuración de una cuenta de almacenamiento, la carga y análisis de datos utilizando SQL sin servidor y Apache Spark. Está diseñado para ayudar a los usuarios a familiarizarse con Azure Synapse y sus herramientas principales.

El tutorial esta basado en las guias de learn de microsoft https://learn.microsoft.com/es-es/azure/synapse-analytics/get-started

## Contenidos
- **Introducción a Azure Synapse Analytics**: Conceptos básicos y requisitos previos.
- **Creación del Área de Trabajo**: Paso a paso sobre cómo crear un área de trabajo en Azure Synapse desde Azure Portal.
- **Carga de Datos de Ejemplo**: Instrucciones para cargar un conjunto de datos de ejemplo en el área de trabajo.
- **Análisis de Datos con SQL Sin Servidor**: Ejemplos de consultas SQL para explorar los datos cargados.
- **Creación y Configuración de Grupos de Apache Spark**: Cómo crear un grupo de Apache Spark sin servidor y asociarlo al proyecto.
- **Análisis de Datos con Spark**: Ejecución de análisis de datos usando notebooks y Apache Spark.

## Requisitos Previos
- **Cuenta de Azure**: Puedes crear una cuenta gratuita en [Azure](https://azure.microsoft.com/es-es/free/).
- **Acceso a Azure Portal**: Debes tener acceso al portal de Azure y permisos para crear recursos.

## Cómo Empezar
### Paso 1: Creación del Área de Trabajo de Synapse en Azure Portal
1. **Abrir Azure Portal**: Ve a [Azure Portal](https://portal.azure.com/) y escribe "Synapse" en la barra de búsqueda sin pulsar enter.  
   ![image](https://github.com/user-attachments/assets/cdb9c9f2-b850-4cec-97a5-e64ac7433a77)

2. **Seleccionar Azure Synapse Analytics**: En los resultados de búsqueda, bajo "Servicios", selecciona **Azure Synapse Analytics**.  

3. **Crear Área de Trabajo**: Selecciona **Crear** para comenzar a crear un área de trabajo.  
   ![image](https://github.com/user-attachments/assets/0a8e4c65-7b39-43e3-9419-a1b7e6c86323)


4. **Configurar la pestaña "Información básica" > Detalles del proyecto**:  
   - **Suscripción**: Selecciona la suscripción que tengas disponible.  
   - **Grupo de recursos**: Usa un grupo de recursos existente o crea uno nuevo.  
   - **Grupo de recursos administrados**: Déjalo en blanco.  
   ![image](https://github.com/user-attachments/assets/9caef305-2a3c-446b-aa48-c2897141e78c)


5. **Configurar la pestaña "Información básica" > Detalles del área de trabajo**:  
   - **Nombre del área de trabajo**: Ingresa un nombre que sea único. Por ejemplo: `myworkspaceai`.  
   - **Región**: Selecciona la misma región donde se encuentran tus aplicaciones y servicios para evitar problemas de rendimiento. Nosotros seleccionamos `North Europe`

6. **Seleccionar Data Lake Storage Gen2**:  
   - **Nombre de la cuenta**: Selecciona **Crear nuevo** y asigna un nombre único, por ejemplo, `accname99`.  Importante recordad
   - **Nombre del sistema de archivos**: Selecciona **Crear nuevo** y asigna el nombre `users`.  
   - Marca la casilla para asignarte el rol de **Colaborador de datos de Storage Blob**.  
![image](https://github.com/user-attachments/assets/56083048-ebbd-4cbb-835a-fb68f238cec0)


7. **Revisar y crear**: Selecciona **Revisar y crear** y luego **Crear**. Espera unos minutos hasta que el área de trabajo esté lista.  
   ![image](https://github.com/user-attachments/assets/ad904b55-b766-4022-9848-d83b2f227ed1)


### Paso 2: Abrir Synapse Studio
1. **Abrir Synapse Studio desde Azure Portal**: Una vez que el área de trabajo esté creada, ve a la sección "Información general" y selecciona **Abrir en el cuadro Abrir Synapse Studio**.  
![image](https://github.com/user-attachments/assets/3d2a16f1-b5f3-447e-bcdf-54f7f6b9397d)


2. **Iniciar sesión en Synapse Studio**: Inicia sesión utilizando tu suscripción de Azure.  
![image](https://github.com/user-attachments/assets/acd7db2d-0541-4274-a870-2064afadd238)


### Paso 3: Cargar Datos de Ejemplo
1. **Descargar el dataset de NYC Taxi**: Descarga el dataset "NYC Taxi - green trip" en tu equipo desde el siguiente [vínculo](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).  
   - Cambia el nombre del archivo descargado a `NYCTripSmall.parquet`.  
![image](https://github.com/user-attachments/assets/2653fea9-ea04-418a-8e67-be59b54e5bb8)


2. **Cargar el archivo en Synapse Studio**:  
   - Ve al centro **Datos** en Synapse Studio.
![image](https://github.com/user-attachments/assets/1cfc74b1-b377-41a6-bb5e-88bffa8d9229)
 
   - Selecciona **Vinculado** y luego el contenedor llamado `users (Primary)`.
![image](https://github.com/user-attachments/assets/ea31e6d7-55e2-44a2-ad7b-90ce057b67ac)

   - Selecciona **Cargar** y sube el archivo `NYCTripSmall.parquet`.
![image](https://github.com/user-attachments/assets/1338f5d5-cf74-4e22-bdcd-d03e00f9e9db)

### Paso 4: Análisis de Datos con un Grupo de SQL Sin Servidor
1. **Crear un script de SQL**: En Synapse Studio, ve al centro de **Desarrollo** y crea un nuevo script de SQL.  
2. **Ejecutar una consulta SQL**: Pega el siguiente código en el script (actualiza `contosolake` con el nombre de tu cuenta de almacenamiento y `users` con el nombre del contenedor):  
   ```sql
   SELECT
       TOP 100 *
   FROM
       OPENROWSET(
           BULK 'https://contosolake.dfs.core.windows.net/users/NYCTripSmall.parquet',
           FORMAT='PARQUET'
       ) AS [result]
   ```  
   - Selecciona **Run (Ejecutar)**.  
![image](https://github.com/user-attachments/assets/3c5e4a69-80b4-4203-9476-efc9fc1a2602)


### Paso 5: Creación de una Base de Datos de Exploración de Datos
1. **Crear una Base de Datos Independiente**: Usa la base de datos `master` para crear una nueva base de datos denominada `DataExplorationDB`:  
   ```sql
   CREATE DATABASE DataExplorationDB 
   COLLATE Latin1_General_100_BIN2_UTF8
   ```  
   *► Importante: Usa una intercalación con sufijo `_UTF8` para que el texto UTF-8 se convierta correctamente en columnas `VARCHAR`.*  

2. **Cambiar el Contexto de la Base de Datos**: Cambia el contexto de la base de datos desde `master` a `DataExplorationDB`:  
   ```sql
   USE DataExplorationDB
   ```  
![image](https://github.com/user-attachments/assets/8209ec46-dd3c-404c-a126-2b5c1cfb85ea)


3. **Crear Objetos de Utilidad**:  
   - Crea un origen de datos externo:  
   ```sql
   CREATE EXTERNAL DATA SOURCE ContosoLake
   WITH ( LOCATION = 'https://contosolake.dfs.core.windows.net')
   ```  
   *► Nota: Los orígenes de datos externos se pueden crear sin credenciales.*  
![image](https://github.com/user-attachments/assets/19836223-a317-46a0-8f74-60927635fa11)

### Paso 6: Creación de un Grupo de Apache Spark Sin Servidor
1. **Crear un Grupo de Apache Spark**:  
   - En Synapse Studio, en el lado izquierdo, selecciona **Administrar > Grupos de Apache Spark**.  
   - Selecciona **Nuevo paso**.  
   - En **Nombre del grupo de Apache Spark** escribe `Spark1`.  
   - En **Tamaño del nodo** escribe `Pequeño`.  
   - En **Número de nodos**, establece el valor mínimo en `3` y el máximo en `3`.  
   - Selecciona **Revisar y crear > Crear**.  
![image](https://github.com/user-attachments/assets/74e37407-7b2b-41ed-bc8d-d1119fa2e086)


### Paso 7: Análisis de Datos de Taxis de Nueva York con un Grupo de Spark
1. **Crear un Nuevo Notebook**: Crea un nuevo notebook en Synapse Studio y asocia el grupo de Spark `Spark1`.  
   ![image](https://github.com/user-attachments/assets/75e01912-f104-42de-877a-055385e08cdd)


2. **Cargar y Mostrar Datos**:  
   ```python
   %%pyspark
   df = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTripSmall.parquet', format='parquet')
   display(df.limit(10))
   ```  
![image](https://github.com/user-attachments/assets/cc805cf3-7172-490c-9144-a00b5ff3a4bb)

![image](https://github.com/user-attachments/assets/170f0867-7500-4c1f-a2ee-283474ba0016)

3. **Ver el Esquema de la Trama de Datos**:  
   ```python
   %%pyspark
   df.printSchema()
   ```  
  ![image](https://github.com/user-attachments/assets/5064e68f-0883-4516-b0bf-a7b3b84174a2)


4. **Cargar los Datos en una Base de Datos de Spark**:  
   ```python
   %%pyspark
   spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
   df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
   ```  
  ![image](https://github.com/user-attachments/assets/6dad4c12-1c58-401c-83ba-ecac3cad3dce)


5. **Análisis de los Datos Mediante Spark y Notebooks**:  
   ```python
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```  
   ![image](https://github.com/user-attachments/assets/3f11da96-9371-4c7d-92f0-266188f34458)

6. **Guardar Resultados de Análisis**:  
   ```python
   %%pyspark
   df = spark.sql("""
      SELECT passenger_count,
          SUM(trip_distance) as SumTripDistance,
          AVG(trip_distance) as AvgTripDistance
      FROM nyctaxi.trip
      WHERE trip_distance > 0 AND passenger_count > 0
      GROUP BY passenger_count
      ORDER BY passenger_count
   """) 
   display(df)
   df.write.saveAsTable("nyctaxi.passengercountstats")
   ```  
  ![image](https://github.com/user-attachments/assets/6e020b7e-d1da-48c9-80a8-85cb49402672)

### Conclusión
En este tutorial, hemos aprendido a:
- Crear un área de trabajo de Azure Synapse Analytics.
- Configurar una cuenta de almacenamiento y cargar datos.
- Utilizar un grupo de SQL sin servidor para analizar datos.
- Crear una base de datos de exploración de datos y objetos de utilidad.
- Configurar y utilizar un grupo de Apache Spark sin servidor.
- Realizar análisis de datos utilizando Apache Spark y notebooks.

Este es solo el comienzo. Puedes seguir explorando las herramientas disponibles en Synapse Studio, como la integración con Power BI para crear visualizaciones interactivas.

