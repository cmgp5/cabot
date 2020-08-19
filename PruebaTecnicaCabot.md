# Proyecto Procedimientos Judiciales CABOT
## Dependencias:
    - org.springframework (spring-webmvc, spring-orm, spring-data-jpa)
    - org.hibernate (hibernate-core)
    - jstl
    - org.lucee (postgresql)
    - javax.servlet (javax.servlet-api)
    - javax.servlet.jsp (javax.servlet.jsp-api)
    - org.projectlombok(lombok)
## FASE 1
#### - Pseudocódigo Script SQL
    1. Crear la base de datos ‘procedimientosdb’
    
    2. Crear la tabla ‘juzgados’ con las siguientes columnas
        - ‘id_juzgado’ de tipo int con las propiedades NOT NULL y AUTO_INCREMENT
        - ‘nombre_juzgado’ de tipo varchar con la propiedad NOT NULL
        - Poner la columna ‘id_juzgado’ como PRIMARY KEY
        
    3. Crear la tabla ‘tipos_procedimiento’ con las siguientes columnas
        - ‘id_tipo_procedimiento’ de tipo int con las propiedades NOT NULL y AUTO_INCREMENT
        - ‘tipo_procedimiento’ de tipo varchar con la propiedad NOT NULL
        - Poner la columna ‘id_tipo_procedimiento’ como PRIMARY KEY

    4. Crear la tabla ‘procedimientos_judiciales’ con las siguientes columnas
        - ‘id_procedimiento’ de tipo int con las propiedades NOT NULL y AUTO_INCREMENT
        - ‘autos_procedimiento’ de tipo varchar(9) con la propiedad NOT NULL
        - ‘id_juzgado’ de tipo int con la propiedad NOT NULL,
        - ‘id_tipo_procedimiento’ de tipo int
        - Poner la columna ‘id_procedimiento’ como PRIMARY KEY
        - Crear una Llave Foránea ‘fk_procedimientos_juzgados’ apuntando al campo ‘id_juzgado’ de la tabla ‘juzgados’
        - Crear una Llave Foránea ‘fk_procedimientos_tipos_procedimiento’ apuntando al campo ‘id_tipo_procedimiento’ de la 
          tabla ‘tipos_procedimiento’
### Diseño
#### - Modelo 
    1.  Para configurar el WebApplicationContext usaría el patrón Front Controller en el fichero web.xml
    2. Configuraría el DispatcherServlet que manejaría la resolución de url a través de un fichero xml de configuración de un View Resolver, direccionando los jsp a un directorio de vistas a través de la propiedad prefix y la terminación de las mismas a la extensión .jsp con la propiedad suffix
    3. Crearía el fichero persistence.xml con las propiedades de conexión a la base de datos procedimientosdb
    4. Crearía una clase JpaConfig para habilitar los repositorios JPA implementando los bean de Entity Manager y Transaction Manager
    5. Crearía una interface ‘ProcedimientoRepository’ que extendería la clase CrudRepository del paquete de SpringFramework y heredaría los métodos CRUD para la gestión de la tabla ‘procedimientos_judiciales’
    6. Crearía una interface ‘JuzgadoRepository’ que extendería la clase CrudRepository y heredaría los métodos CRUD para la gestión de la tabla ‘juzgados’
    7. Crearía una interface ‘TipoProcedimientoRepository’ que extendería la clase CrudRepository y heredaría los métodos CRUD para la gestión de la tabla ‘tipos_ procedimiento’
    8. Crearía la clase ‘Procedimiento’ con la anotación JPA @Entity y que creara instancias mapeadas a las columnas de la tabla ‘procedimientos_judiciales’. Para esta clase implementaría el patrón de creación Builder para reducir el acoplamiento y facilitar la inyección de dependencias
    9. Crearía la clase ‘Juzgado’ con la anotación JPA @Entity y que creara instancias mapeadas a las columnas de la tabla ‘juzgados’. Para esta clase implementaría el patrón de creación Builder
    10. Crearía la clase ‘TipoProcedimiento’ con la anotación JPA @Entity y que creara instancias mapeadas a las columnas de la tabla ‘tipos_procedimientos’. Para esta clase implementaría el patrón de creación Builder

### - Controlador
    1. Crearía el controlador ‘ProcedimientoController’, con la anotación @Controller, aquí realizaría el mapeo que realizaría las siguientes tareas
    2. Redireccionaría las peticiones REST a cada uno de los servicios
    3. Recibiría la respuesta de dichos servicios después de hacer una Query en la base de datos
    4. Esta respuesta se mapearía a la vista para ser accedida a través de etiquetas de Expression Language o de anotaciones JSTL .
### - Vista
    1. Crearía un fichero JSP en donde desplegaría una tabla con las siguientes propiedades
    2. Sobre la tabla ubicaría un botón para crear un nuevo procedimiento que llevaría a un nuevo JSP en donde se desplegaría un formulario con los siguientes campos:
        - Autos del procedimiento
        - Nombre del Juzgado (Select cargado desde la tabla Juzgados)
        - Tipo de Procedimiento (Select cargado desde la tabla Tipos de Procedimiento)
        - Botón Guardar, que a través del servicio ProcedimientoService guardaría el nuevo procedimiento en la base de datos
        - Botón Cancelar que retornaría al JSP Principal
    3. En el encabezado de la tabla, bajo los títulos de la columnas crearía unos filtros para realizar una búsqueda en la tabla de la siguiente manera:
        - Id del procedimiento, campo de texto
        - Autos del procedimiento, campo de texto
        - Nombre del Juzgado, Select
        - Tipo de Procedimiento, Select
    4. Cada fila mostraría la información de cada uno de los procedimientos judiciales (Nombre del procedimiento, Autos del procedimiento, Nombre del Juzgado y de manera opcional el Tipo de Procedimiento
    5. Al final de cada fila se ubicaría un botón que editaría la información del proceso, al presionar este botón, se redireccionara al mismo JSP usado para crear un nuevo procedimiento, pero en los campos del formulario estarán precargados los datos del procedimiento que se pretende editar, así reusaría este JSP
### - Servicios
    1. Crearía el servicio ProcedimientoService: Usado para instanciar la Interfaz ProcedimientoRepository, de esta manera se pueden invocar los métodos CRUD para consultar y actualizar la tabla procedimientos_judiciales
        - Método findAll, Realiza la consulta a la base de datos y trae todas las filas de la tabla
        - Método save, Hace un Insert en la tabla con los datos del nuevo procedimiento
    2. Crearía el servicio JuzgadoService: Usado para instanciar la Interfaz JuzgadoRepository, para consultar la tabla juzgados
        - Método findAll, Realiza la consulta a la base de datos y trae todas las filas de la tabla
        - Método findById, Realiza la consulta a la base de datos y trae un juzgado de acuerdo a su ID
    3. Crearía el servicio TipoProcedimientoService: Usado para instanciar la Interfaz TipoProcedimientoRepository, para consultar la tabla tipos_procedimiento
        - Método findAll, Realiza la consulta a la base de datos y trae todas las filas de la tabla
        - Método findById, Realiza la consulta a la base de datos y trae un procedimiento de acuerdo a su ID
### Gestión de Errores
    1. En el paquete de servicios crearía unas clases que funcionarían como validadores y gestores de mensajes de la siguiente manera
    2. Crearía una interfaz Validator que tendría como atributos dos listas, una de errores y otra de mensajes y un método de validar, de esta manera se garantiza el principio de responsabilidad única
    3. Crearía una clase GeneralValidator que implementaría la interfaz Validator sobrescribiendo el método para validar y dando acceso a sus atributos
    4. Crearía una clase ProcedimientoValidator, donde se realizarían las validaciones necesarias para la clase Procedimiento, es decir, verificar la integridad de los datos (Evitar caracteres indeseados), validación de tamaño de strings, validación de existencia de datos en las tablas a través de su id
    - Como resultado se obtendría una lista errores y a cada uno de estos se le mapea un mensaje descriptivo en una lista de mensajes, la cual será presentada al usuario en caso de que se presente algún tipo de excepción o error
### Patrones de diseño usados
    1. Modelo Vista Controlador
    2. Front Controller
    3. View Helper
    4.Builder
#### Query Nativa para obtener procedimientos judiciales
```javascript
public List<Procedimiento> selectAllProcedimientos(){
    EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("procedimientosdb");
    EntityManager entityManager = entityManagerFactory.createEntityManager();
    entityManager.getTransaction().begin();
    
    List<Procedimiento> listProcedimientos = new ArrayList<>();
    Query query = entityManager.createNativeQuery("SELECT p. id_procedimiento, p. autos_procedimiento, j.nombre_juzgado, t.tipo_procedimiento FROM procedimientos_judiciales p juzgados j tipos_procedimiento t WHERE p.id_juzgado=j.id_juzgado AND p.id_tipo_procedimiento = t. id_tipo_procedimiento");
    List<Object[]> procedimientos = query.getResultList();

    for (Object[] procedimiento : procedimientos){
    	listProcedimientos.add(new Procedimiento.Builder(procedimiento[0], procedimiento[1], procedimiento[2]).withTipoProcedimiento(procedimiento[3]).build());
    }
    
    entityManager.getTransaction().commit();
    entityManager.close();
    return listProcedimientos;
}
```
### Query Nativa para obtener procedimientos judiciales búsqueda por id de procedimiento
```javascript
public Procedimiento selectProcedimientoById(int id_procedimiento){
    EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("procedimientosdb");
    EntityManager entityManager = entityManagerFactory.createEntityManager();
    entityManager.getTransaction().begin();
    
    Procedimiento procedimiento;
    Query query = entityManager.createNativeQuery("SELECT p. id_procedimiento, p. autos_procedimiento, j.nombre_juzgado, t. tipo_procedimiento FROM procedimientos_judiciales p juzgados j tipos_procedimiento t WHERE p.id_procedimiento = :id AND p.id_juzgado=j.id_juzgado AND p.id_tipo_procedimiento = t. id_tipo_procedimiento");
    query.setParameter("id", id_procedimiento);
    
    Object[] procedimientoResult = query.getSingleResult();
    procedimiento = new Procedimiento.Builder(procedimientoResult[0], procedimientoResult[1], procedimientoResult[2]).withTipoProcedimiento(procedimientoResult[3]).build());
    
    entityManager.getTransaction().commit();
    entityManager.close();
    return procedimiento;
}
```

## FASE 2
### - Modificaciones ScriptSQL
    1. Crear la tabla ‘subtipos_procedimientos’ con las siguientes columnas
        - ‘id_subtipo_procedimiento’ de tipo int con las propiedades NOT NULL y AUTO_INCREMENT
        - ‘subtipo_procedimiento’ de tipo varchar con la propiedad NOT NULL
        - ‘id_tipo_procedimiento’ de tipo int
        - Poner la columna ‘id_subtipo_procedimiento’ como PRIMARY KEY
        - Crear una Llave Foránea ‘fk_subtipos_tipos_procedimiento’ apuntando al campo ‘id_tipo_procedimiento’ de la tabla ‘tipos_procedimiento’

### - Modificaciones Java
### Modelo 
    1. Crear una interface ‘SubTipoProcedimientoRepository’ que extendería la clase CrudRepository para la gestión de la tabla ‘subtipos_ procedimiento’
    2. Crear la clase ‘SubTipoProcedimiento’ con la anotación JPA @Entity y que creara instancias mapeadas a las columnas de la tabla ‘subtipos_ procedimiento’. implementando el patrón de creación Builder
### Controlador
    1. Crear la petición GET para direccionar el tráfico bidireccional del servicio SubTiposProcedimientoService
### Vista
    1. En el fichero JSP principal en la tabla desplegada se crearía un campo nuevo que mostraría el Subtipo y su respectivo filtro bajo el nombre de la columna en la cabecera de la tabla
    2. En el fichero JSP de creación/edición de cada procedimiento se implementaría un campo adicional para el subtipo de procedimiento con un Select cargado desde la tabla subtipos_procedimiento y cuyo id_tipo_procedimiento sea igual al id_tipo_procedimiento del procedimiento seleccionado
### Servicios
    1. Crear el servicio SubTiposProcedimientoService: Usado para instanciar la Interfaz SubTipoProcedimientoRepository, para consultar la tabla subtipos_procedimientos
    2. Método findByIdTipoProcedimiento, Realiza la consulta a la base de datos y trae todas las filas de la tabla que concuerden con el valor de id_tipo_procedimiento

## FASE 3
### - Modificaciones ScriptSQL
    1. Crear la tabla ‘usuarios’ con las siguientes columnas
        - ‘id_usuario’ de tipo int con las propiedades NOT NULL y AUTO_INCREMENT
        - ‘usuario’ de tipo varchar con la propiedad NOT NULL
        - ‘id_tipo_usuario’ de tipo int
        - Poner la columna ‘id_usuario’ como PRIMARY KEY
        - Crear una Llave Foránea ‘fk_usuario_tipo_usuario’ apuntando al campo ‘id_tipo_usuario’ de la tabla ‘tipos_usuario’
    2. Crear la tabla ‘tipos_usuario’ con las siguientes columnas
        - ‘id_tipo_usuario’ de tipo int con las propiedades NOT NULL y AUTO_INCREMENT
        - ‘tipo_usuario’ de tipo varchar con la propiedad NOT NULL
        - Poner la columna ‘id_tipo_usuario’ como PRIMARY KEY
    3. Crear la tabla ‘usuarios_procedimientos’ con las siguientes columnas
        - 'id_usuario_procedimiento' de tipo int con las propiedades NOT NULL y AUTO_INCREMENT
        - ‘id_tipo_usuario’ de tipo int
        - ‘id_procedimiento’ de tipo int
        - Poner la columna ‘id_usuario_procedimiento’ como PRIMARY KEY
        - Crear una Llave Foránea ‘fk_tipo_usuario_procedimiento’ apuntando al campo ‘id_tipo_usuario’ de la tabla ‘tipos_usuario'
        - Crear una Llave Foránea ‘fk_procedimiento_tipo_usuario’ apuntando al campo ‘id_procedimiento’ de la tabla ‘procedimientos_judiciales'
