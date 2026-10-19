# Red Social Pascualina — Informe técnico del modelo físico

## 1. Introducción

En este proyecto realicé la implementación del modelo físico de la base de datos de la Red Social Pascualina utilizando PostgreSQL y pgAdmin 4. El objetivo fue llevar el modelo lógico proporcionado a una estructura funcional en la que se pudieran almacenar y relacionar los datos de los estudiantes, sus intereses, habilidades, grupos, publicaciones, comentarios, reacciones, recursos y eventos académicos.

Para desarrollar la base de datos tomé como referencia el modelo lógico en tercera forma normal (3FN), procurando mantener la organización de la información y las relaciones definidas en el diagrama.

## 2. Decisiones de diseño

### Organización de las tablas

La base de datos quedó organizada en 15 tablas. Decidí mantener separada la información de cada entidad para evitar repetir datos innecesariamente y facilitar su administración.

Por ejemplo, los datos de los estudiantes se almacenan en la tabla `estudiante`, mientras que sus intereses y habilidades se gestionan mediante tablas independientes. De esta manera, un estudiante puede tener varios intereses y habilidades sin necesidad de agregar múltiples columnas o repetir información dentro de su registro.

También se incluyeron las tablas `programa_academico` y `recurso_publicacion`, que permiten organizar mejor la información relacionada con los programas académicos y los recursos asociados a las publicaciones.

### Llaves primarias

Definí llaves primarias para identificar de manera única los registros de cada tabla. En las tablas principales utilicé identificadores enteros, como `id_estudiante`, `id_grupo` e `id_publicacion`.

En las tablas que representan relaciones de muchos a muchos utilicé llaves primarias compuestas. Es el caso de `estudiante_interes`, `estudiante_habilidad`, `membresia`, `reaccion` y `asistencia`, donde la combinación de dos identificadores permite distinguir cada relación y evitar duplicados.

### Llaves foráneas

Utilicé llaves foráneas para mantener la integridad referencial entre las tablas. Estas restricciones permiten que las relaciones definidas en el modelo lógico se mantengan también en la base de datos.

Por ejemplo, cada estudiante está asociado a un programa académico, cada publicación tiene un autor y cada comentario está relacionado con una publicación y con el estudiante que lo escribió.

También se implementaron las relaciones entre estudiantes y grupos, publicaciones y reacciones, y eventos y asistentes. En total, la base de datos cuenta con 21 llaves foráneas.

### Tipos de datos

Seleccioné los tipos de datos según la información que almacena cada columna:

- `INTEGER` para los identificadores y las referencias entre tablas.
- `VARCHAR(n)` para textos de longitud definida, como nombres, correos, estados y categorías.
- `TEXT` para el contenido de las publicaciones, que puede tener una extensión mayor.
- `DATE` para fechas sin necesidad de almacenar una hora.
- `TIMESTAMP` para los registros que necesitan guardar tanto la fecha como la hora.

El campo `semestre` quedó definido como `VARCHAR(6)`, de acuerdo con el modelo lógico. Esto permite almacenar el valor como texto, respetando el tipo de dato establecido en el diagrama.

Los identificadores principales se configuraron con generación automática de valores para facilitar la creación de nuevos registros.

### Campos obligatorios y valores nulos

Definí como obligatorios los campos que son necesarios para registrar correctamente la información, utilizando la restricción `NOT NULL`.

En cambio, permití valores nulos en los campos que pueden no tener información en determinados casos. Por ejemplo, la biografía y la foto de un estudiante son opcionales. También pueden quedar sin valor la descripción de un grupo, el lugar de un evento o el enlace virtual, según corresponda.

De la misma manera, `id_grupo` puede quedar nulo en las publicaciones y los eventos que no estén asociados a un grupo específico.

### Restricciones UNIQUE

Agregué restricciones `UNIQUE` para evitar que se repitan determinados valores que deben identificar de forma única la información registrada. Estas restricciones se aplicaron al correo institucional del estudiante y a los nombres de los programas académicos, intereses y habilidades.

Con esto se mejora la consistencia de los datos y se evitan duplicaciones en esos campos.

## 3. Implementación

La base de datos se creó en PostgreSQL y se administró mediante pgAdmin 4. Primero se creó la estructura de las tablas y después se verificaron las llaves primarias, las llaves foráneas, los tipos de datos y las restricciones definidas.

Para representar las relaciones entre las entidades también se generó un diagrama ERD en pgAdmin 4, con el fin de comprobar visualmente la conexión entre las tablas y contrastarla con el modelo lógico original.

## 4. Verificaciones realizadas

Después de implementar la estructura ejecuté consultas SQL para comprobar que las tablas y restricciones se hubieran creado correctamente.

Los resultados obtenidos fueron los siguientes:

| Elemento verificado | Resultado |
|---|---:|
| Tablas creadas | 15 |
| Llaves primarias | 15 |
| Llaves foráneas | 21 |
| Restricciones UNIQUE | 4 |
| Tipo de dato de `semestre` | `VARCHAR(6)` |

Las comprobaciones realizadas finalizaron correctamente. También revisé las columnas que admiten valores nulos para verificar que coincidieran con las decisiones definidas en el modelo lógico.

Estas pruebas permitieron confirmar que la estructura principal de la base de datos quedó implementada y que las restricciones esperadas están presentes.

## 5. Conclusiones

Con este trabajo pude llevar el modelo lógico de la Red Social Pascualina a una estructura física en PostgreSQL. La separación de las entidades, el uso de llaves primarias y foráneas, y la aplicación de restricciones permiten mantener los datos organizados y las relaciones correctamente definidas.

La implementación quedó conformada por 15 tablas, 15 restricciones de llave primaria, 21 llaves foráneas y 4 restricciones `UNIQUE`. Además, se verificaron los tipos de datos y la configuración de los campos opcionales.

Como resultado, se cuenta con una estructura de base de datos preparada para almacenar la información de la red social y servir como base para futuras operaciones y funcionalidades.

## 6. Herramientas utilizadas

- **PostgreSQL:** sistema gestor de bases de datos.
- **pgAdmin 4:** herramienta para administrar la base de datos, ejecutar consultas SQL y generar el diagrama ERD.
- **SQL:** lenguaje utilizado para definir las tablas, relaciones y restricciones.

