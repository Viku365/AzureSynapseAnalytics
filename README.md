# Proyecto: Azure Synapse Analytics Tutorial

## Descripción
Este repositorio contiene un tutorial completo sobre cómo comenzar con Azure Synapse Analytics, incluyendo la creación de un área de trabajo, la configuración de una cuenta de almacenamiento, la carga y análisis de datos utilizando SQL sin servidor y Apache Spark. Está diseñado para ayudar a los usuarios a familiarizarse con Azure Synapse y sus herramientas principales.

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
   *► Insertar captura de pantalla mostrando la barra de búsqueda y la palabra "Synapse".*

2. **Seleccionar Azure Synapse Analytics**: En los resultados de búsqueda, bajo "Servicios", selecciona **Azure Synapse Analytics**.  
   *► Insertar captura de pantalla mostrando los resultados de la búsqueda.*

3. **Crear Área de Trabajo**: Selecciona **Crear** para comenzar a crear un área de trabajo.  
   *► Insertar captura de pantalla de la opción "Crear".*

4. **Configurar la pestaña "Información básica" > Detalles del proyecto**:  
   - **Suscripción**: Selecciona la suscripción que tengas disponible.  
   - **Grupo de recursos**: Usa un grupo de recursos existente o crea uno nuevo.  
   - **Grupo de recursos administrados**: Déjalo en blanco.  
   *► Insertar captura de pantalla mostrando la configuración de los detalles del proyecto.*

5. **Configurar la pestaña "Información básica" > Detalles del área de trabajo**:  
   - **Nombre del área de trabajo**: Ingresa un nombre que sea único. Por ejemplo: `myworkspace`.  
   - **Región**: Selecciona la misma región donde se encuentran tus aplicaciones y servicios para evitar problemas de rendimiento.  
   *► Insertar captura de pantalla de los detalles del área de trabajo.*

6. **Seleccionar Data Lake Storage Gen2**:  
   - **Nombre de la cuenta**: Selecciona **Crear nuevo** y asigna un nombre único, por ejemplo, `contosolake`.  
   - **Nombre del sistema de archivos**: Selecciona **Crear nuevo** y asigna el nombre `users`.  
   - Marca la casilla para asignarte el rol de **Colaborador de datos de Storage Blob**.  
   *► Insertar captura de pantalla mostrando la configuración del almacenamiento.*

7. **Revisar y crear**: Selecciona **Revisar y crear** y luego **Crear**. Espera unos minutos hasta que el área de trabajo esté lista.  
   *► Insertar captura de pantalla de la pantalla de revisión.*

### Paso 2: Abrir Synapse Studio
1. **Abrir Synapse Studio desde Azure Portal**: Una vez que el área de trabajo esté creada, ve a la sección "Información general" y selecciona **Abrir en el cuadro Abrir Synapse Studio**.  
   *► Insertar captura de pantalla mostrando cómo abrir Synapse Studio.*

2. **Iniciar sesión en Synapse Studio**: Inicia sesión utilizando tu suscripción de Azure.  
   *► Insertar captura de pantalla de la pantalla de inicio de sesión.*

### Paso 3: Cargar Datos de Ejemplo
1. **Descargar el dataset de NYC Taxi**: Descarga el dataset "NYC Taxi - green trip" en tu equipo desde el siguiente [vínculo](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).  
   - Cambia el nombre del archivo descargado a `NYCTripSmall.parquet`.  
   *► Insertar captura de pantalla de la ubicación del dataset.*

2. **Cargar el archivo en Synapse Studio**:  
   - Ve al centro **Datos** en Synapse Studio.  
   - Selecciona **Vinculado** y luego el contenedor llamado `users (Primary)`.  
   - Selecciona **Cargar** y sube el archivo `NYCTripSmall.parquet`.  
   *► Insertar captura de pantalla mostrando el proceso de carga del archivo.*

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
   *► Insertar captura de pantalla mostrando el código y el resultado de la consulta.*

### Paso 5: Creación de una Base de Datos de Exploración de Datos
1. **Crear una Base de Datos Independiente**: Usa la base de datos `master` para crear una nueva base de datos denominada `DataExplorationDB`:  
   ```sql
   CREATE DATABASE DataExplorationDB 
   COLLATE Latin1_General_100_BIN2_UTF8
   ```  
   *► Importante: Usa una intercalación con sufijo `_UTF8` para que el texto UTF-8 se convierta correctamente en columnas `VARCHAR`.*  
   *► Insertar captura de pantalla mostrando el resultado de la creación de la base de datos.*

2. **Cambiar el Contexto de la Base de Datos**: Cambia el contexto de la base de datos desde `master` a `DataExplorationDB`:  
   ```sql
   USE DataExplorationDB
   ```  
   *► Insertar captura de pantalla mostrando cómo cambiar el contexto de la base de datos.*

3. **Crear Objetos de Utilidad**:  
   - Crea un origen de datos externo:  
   ```sql
   CREATE EXTERNAL DATA SOURCE ContosoLake
   WITH ( LOCATION = 'https://contosolake.dfs.core.windows.net')
   ```  
   *► Nota: Los orígenes de datos externos se pueden crear sin credenciales.*  
   *► Insertar captura de pantalla mostrando el código y el resultado.*

4. **Crear un Inicio de Sesión para un Usuario**:  
   - Crea un inicio de sesión y un usuario de base de datos:  
   ```sql
   CREATE LOGIN data_explorer WITH PASSWORD = 'My Very Strong Password 1234!';
   CREATE USER data_explorer FOR LOGIN data_explorer;
   GO
   GRANT ADMINISTER DATABASE BULK OPERATIONS TO data_explorer;
   GO
   ```  
   *► Insertar captura de pantalla mostrando el resultado de la creación del usuario.*

5. **Explorar el Contenido del Archivo**:  
   ```sql
   SELECT
       TOP 100 *
   FROM
       OPENROWSET(
           BULK '/users/NYCTripSmall.parquet',
           DATA_SOURCE = 'ContosoLake',
           FORMAT='PARQUET'
       ) AS [result]
   ```  
   *► Insertar captura de pantalla mostrando el código y el resultado de la consulta.*

### Paso 6: Creación de un Grupo de Apache Spark Sin Servidor
1. **Crear un Grupo de Apache Spark**:  
   - En Synapse Studio, en el lado izquierdo, selecciona **Administrar > Grupos de Apache Spark**.  
   - Selecciona **Nuevo paso**.  
   - En **Nombre del grupo de Apache Spark** escribe `Spark1`.  
   - En **Tamaño del nodo** escribe `Pequeño`.  
   - En **Número de nodos**, establece el valor mínimo en `3` y el máximo en `3`.  
   - Selecciona **Revisar y crear > Crear**.  
   *► Insertar captura de pantalla mostrando la configuración del grupo de Apache Spark.*

### Paso 7: Análisis de Datos de Taxis de Nueva York con un Grupo de Spark
1. **Crear un Nuevo Notebook**: Crea un nuevo notebook en Synapse Studio y asocia el grupo de Spark `Spark1`.  
   *► Insertar captura de pantalla mostrando el notebook creado y la asociación.*

2. **Cargar y Mostrar Datos**:  
   ```python
   %%pyspark
   df = spark.read.load('abfss://users@contosolake.dfs.core.windows.net/NYCTripSmall.parquet', format='parquet')
   display(df.limit(10))
   ```  
   *► Insertar captura de pantalla mostrando el resultado de la ejecución de la celda.*

3. **Ver el Esquema de la Trama de Datos**:  
   ```python
   %%pyspark
   df.printSchema()
   ```  
   *► Insertar captura de pantalla mostrando el esquema de la trama de datos.*

4. **Cargar los Datos en una Base de Datos de Spark**:  
   ```python
   %%pyspark
   spark.sql("CREATE DATABASE IF NOT EXISTS nyctaxi")
   df.write.mode("overwrite").saveAsTable("nyctaxi.trip")
   ```  
   *► Insertar captura de pantalla mostrando el resultado de la creación de la base de datos.*

5. **Análisis de los Datos Mediante Spark y Notebooks**:  
   ```python
   %%pyspark
   df = spark.sql("SELECT * FROM nyctaxi.trip") 
   display(df)
   ```  
   *► Insertar captura de pantalla mostrando los datos de la tabla `nyctaxi.trip`.*

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
   *► Insertar captura de pantalla mostrando el gráfico generado.*

### Conclusión
En este tutorial, hemos aprendido a:
- Crear un área de trabajo de Azure Synapse Analytics.
- Configurar una cuenta de almacenamiento y cargar datos.
- Utilizar un grupo de SQL sin servidor para analizar datos.
- Crear una base de datos de exploración de datos y objetos de utilidad.
- Configurar y utilizar un grupo de Apache Spark sin servidor.
- Realizar análisis de datos utilizando Apache Spark y notebooks.

Este es solo el comienzo. Puedes seguir explorando las herramientas disponibles en Synapse Studio, como la integración con Power BI para crear visualizaciones interactivas.

*► Insertar captura de pantalla final con el dashboard de Synapse Studio.*

