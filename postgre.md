# Postgre
<!-- link de un [blog](https://www.todopostgresql.com/comandos-postgresql-alter/) -->

<a href="https://www.todopostgresql.com/comandos-postgresql-alter" target="_blank">link de un blog</a>

curso [youtube](https://www.youtube.com/playlist?list=PL49mtc79R0IPOUl9RhoawGnf6EOWkV7Ev)

Ver todas las tablas que tenemos en postgre

```sql
select*from pg_tables
```

RESTRICCIONES son: (se agregan  en las columnas de  las tablas)

```sql
Primary key, foreign key, unique, check
```

- Concatenar
    
    ```sql
    select nombre||' '||ap_paterno as NombresApellidos from usuario
    ```
    

- DISTINCT
    
    ```sql
    ---muestra los distintos valores de las filas; es decir no muetra repetidos
    select distinct ap_paterno from usuario
    ```
    
- GROUP BY
    
    ```sql
    --muestra la cantidad de registros de los 2 tipos de documento que hay en este caso
    select tipo_documento,count(*)from documento group by tipo_documento
    ```
    
- Foreign key
    
    nos ayuda a relacionar 2 tablas por la clave principal, y la validez de la misma; es decir tiene que existir la clave foranea en su tabla  como clave principal
    
- Modificar  la estructura de una tabla
    
    ```sql
    alter table productos ADD precio_compra int --Agregar una nueva columna
    alter table productos RENAME TO produc --Rename TABLE
    alter table productos drop column col1, drop column ****col2--eliminar una o mas columnas 
    alter table productos add PRIMARY KEY (id_prod);--le asignamos como clave primaria a esta columna
    alter table productos add foreign key(user_id) references users(id); --asignar como llave foranea a una columna
    alter table productos drop constraint  user_id--eliminar una foreign key
    
    ```
    

- Obtener datos de un archivo excel
    
    guardar el archivo con csv(delimitado por comas)
    
    ```sql
    COPY productos FROM 'C:/registros/productos.csv (direccion del archivo con extencion)' HEADER CSV DELIMITER ','--de acuerdo a como este separado los datos del csv
    
    --COPY usuario FROM 'C:\productosexcelcsv/users.csv' HEADER CSV DELIMITER ';'
    ```
    

- Crear tabla a partir de otra tabla
    
    ```sql
    create table productos as select nombre, precio,stock FROM Prod where nombre='yogurt' --crear una tabla productos basado en la tabla Prod
    
    ```
    

- Crear una tabla  usando JOINS
    
    ```sql
    CREATE TABLE cantmovi AS SELECT * from movimientos m join documentos d on m.documento_id=d.id 
    ```
    
- Insertar registros con select
    
    ```sql
    INSERT INTO productos(nombre,precio,stock)
    SELECT nombre,precio,stcok from Porducts where precio>200
    ```
    
- Actualizar datos de otra tabla con valores de otra tabla con JOINS
    
    ```sql
    UPDATE productos pro SET nombre=c.nombre from componentes c where pro.nom=c.nombre
    ```
    
- Borrar registros de una tabla consultando otra tabla  “DELETE USING”, usarlo en caso no sepamos cual es su id
    
    ```sql
    DELETE FROM PRODUCTOS pro USING comp_es ce where pro.id =ce.id and nomEspe="pantalla"
    --eliminar todos los productos usando la tabla comp_esp cuando el id del pro sea = al id del comp_es; es deicr eliminar todos los regitros de mi tabla productos que tengan el compes tal
    ```
    
- Modificar registros usando subconsultas
    
    ```sql
    update productos p set p.nombre =subquery.nombre from(
    select id from componenetes where id=5 
    ) as subquery where p.id=subquery.id;
    ```
    
- LIKE y NO LIKE
    
    ```sql
    select * from productos where nombre LIKE 'A%' --importante, al buscar lo hara identificando mayusculas de minusculas 
    																					'%A%'
    --si quiero mayus y minus
    select * from productos where nombre LIKE 'A%' and nombre LIKE '%a'
    ```
    
    LINK TIPOS DE [DATOS](http://codigoelectronica.com/blog/postgresql-tipo-de-datos)
    
    otra manera de incrementar
    
    ```sql
    SMALLSERIAL -- se aumenta en automatico
    ```
    
    ```sql
    CREATE TABLE productos(
    id SMALLSERIAL PRIMARY KEY,
    nombre VARCHAR(40) not null,
    fecha DATE DEFAULT '1900-01-01'--que al registrar y no envi datos de este campo por dfecto se llena con esa fecha
    )
    ```
    
    - Cascade y no action
        
        al crear mi tabla o al modificarla 
        
        ```sql
        ALTER TABLE productos add constraint user_id foreign key(user_id) references users(id) ON DELETE CASCADE ON UPDATE CASCADE
        --Por defecto al crear una tabla esta como "NO ACTION"; que no elimine en cascada 
        ```
        
    - ON DELETE SET NULL
        
        ```sql
        --que al eliminar un registro de una tabla y un registro de otra tabla dependa de ella no se elimine, que se llenene con null
        ALTER TABLE productos add constraint user_id foreign key(user_id) references users(id) ON DELETE SET NULL
        ```
        
    - Operador IN y NOT IN
        
        ```sql
        select * from productos where nombre IN('laptop','almacenamiento',CPU) --buscar en productos lo que son ()
        
        select * from productos where nombre NOT IN('laptop','almacenamiento',CPU) --buscar en productos lo que no son ()
        ```
        
    - Operador Diferente
        
        ```sql
        select * from productos where nombre <> "laptops" --buscar todos los productos que no son laptops
        ```
        
    - LIMIT y OFFSET
        
        ```sql
        select * from productos LIMIT 5 OFFSET 0 --mostrar los primerros 5 empezandos desde el registro 0
        ```
        
    - Restricciones UNIQUE y CHECK
        
        ```sql
        UNIQUE-> --el valor no se va a repetir 
        CHECK->  -- es como una condicional, que tiene que cumplir 
        
        alter table productos add contraint ckproductos CHECK(precio<0 and precio>10000)-- le decimos que el precio no tiene que ser menor a 0 y mayor a 10000
        ```
        
    - Consultar por null
        
        ```sql
        select * from c where nombre is NULL
        ```
        
    
    CLASE 3
    
    - Comando CAST y símbolo '::'.
        
        ```sql
        select CAST(100 as VARCHAR)  --convertir el tipo de dato int a varchar solo en consulta
        o
        select 100::VARCHAR 
        select '1999-01-19'::DATE 
        
        --caso especial
        select 100::DOUBLE PRECISION--que es float8?
        ```
        
    - Funciones fecha
        
        ```sql
        CURRENT_DATE --solo fecha
        CURRENT_TIMESTAMP || NOW() --fevha y hora y zona horaria
        CURRENT_TIME -- fechca y hora 
        
        ```
        
    - Funcion EXTRACT
        
        ```sql
        select EXTRACT(MONTH FROM '2020-05-12'::DATE)-- si queremos extraer un dia, año, mes de una fecha
        select EXTRACT(century FROM CURRENT_TIMESTAMP)--extraer el siglo
        select EXTRACT(century FROM CURRENT_TIMESTAMP)-1 --siglo anterior
        
        select EXTRACT(DOY FROM CURRENT_TIMESTAMP) --dia del año
        ```
        
    - Interval
        
        ```sql
        select NOW()-'7 day'::INTERVAL --saber la fecha de hace 7 dias; interval nos permite tener intervalos de tiempo
        
        ```
        
    - Funcion AGE
        
        ```sql
         select AGE(NOW(),DATE('2021-1-1'))--diferencia entre una fecha y otra
        ```
        
    - Registros de ayer || SELECT YESTERDAY y Registros de hoy || SELECT TODAYaaa
        
        ```sql
        select 'yesterday'::DATE --me muestra la fecha de ayer
        select 'yesterday'::TIMESTAMP WITH TIME ZONE  --fecha de ayer con zona horaria
        select 'today'::DATE--HOY
        ```
        
    - Obtener los ingresos de este mes
        
        ```sql
        select*from movimientos where EXTRACT(MONTH FROM fecha)=EXTRACT(MONTH FROM CURRENT_DATE) -- obtener los registros que coincidan la fecha de este mes
        select*from  movimientos where EXTRACT(YEAR FROM fecha)='2022'--obtener los ingresos de este año
        ```
        
    - Obtener el ultimo dia del mes(DATE_TRUNC)
        
        ```sql
        select DATE_TRUNC('MONTH',current_date)-> --"2022-11-01 00:00:00-05" obtenemos el primer dia del mes 0:00 horas
        select DATE_TRUNC('MONTH',current_date)+'1 month'::interval - '1 sec'::INTERVAL --"2022-11-30 23:59:59-05" ultimo dia del mes  con un segundo para el siguiente mes
        select EXTRACT(DAY FROM(select DATE_TRUNC('MONTH',current_date)+'1 month'::interval - '1 sec'::INTERVAL)) --dia 30
        ```
        
    - date_part [link](https://www.postgresqltutorial.com/postgresql-date-functions/postgresql-date_part/)
        
        
    
    Clase 4
    
    - s
        
        ```sql
        select CAST(RANDOM()*1000 as INT)-- numero aleatorio desde el 0 a 999; el INT es para que no salgan decimales
        o
        select RANDOM()*1000
        
        select ROUND(4890.0978,2)-- 4890.10 redondear un digito
        select CAST('hola como estas' as char(2))--"ho" obtener la cantidad de caracteres que quiero mostrar(CASTEO)
        select TRANSLATE('López','áéíóúÁÉÍÓÚ','aeiou')--quitar acentos a una palabra
        select RTRIM('wiel yahies','yahies')--"wiel " recorat caracteres de un texto
        select REVERSE('wiel')--reverse
        select substring('wiel',length('wiel')-3,2)--de la cadena, obtienes la longitud y de ella restas, y el resultado lo muestras hasta el 2do caracter en adelante
        select TRIM('  wiel  ')--quitar espacion al principio y al final
        select TRIM(TRAILING '*' from 'wiel********')--eliminar ciertos caracteres al final de una cadena de texto
        select LTRIM('----wiel','-')--Eliminar caracteres al inicio.
        select RTRIM('----wiel','-')--Eliminar caracteres al final.
        select CHAR_LENGTH('2321321wiel')-- Saber longitud de una cadena de texto.
        select to_char(current_timestamp,'YYYY-MM-DD')--Función To_Char | Mostrar sólo año y mes de una fecha.
        select REPEAT('wiel',4) -- repetir mi nombre 4 veces
        select Bpcharcmp('wiel','wiél')-- si no coincide "-1", si coincide "0" Función Bcpcharcmp | Comparar dos cadenas de texto.
        select ASCII('@')--"64" devolver el codigo ascii de un caracter
        select CHR('96')->--"`" Función Chr | Devolver caracter de un determinado código Ascii.
        select INITCAP('FLOR')--"Flor" Poner en mayúsculas la primera letra de una cadena.
        select LOWER('FLOR') --Convertir caracteres en minúsculas.
        select UPPER('flor')--Convertir caracteres en mayúsculas.
        select QUOTE_LITERAL(800)--'800' Convertir valor a comidas SIMPLES
        select Quote_indent(800)--"800" Convertir valor a comidas DOBLES
        select SPLIT_PART('2022-01-01','-',1)-- 2022 SEPARAR CADENAS
        ```
        
    
    Clase 5
    
    - funciones
        - crear una funcion
            
            ```sql
            CREATE O REPLACE FUNCTION mifuncion(num INT) --replace esta de ejemplo
            RETURNS INT
            AS $$
            BEGIN
            
            RETURN num *2;
            END;
            $$ LANGUAGE PLPGSQL;
            --si tiene 2 parametros
            
            CREATE O REPLACE FUNCTION mifuncion(num INT,num INT) --replace esta de ejemplo
            RETURNS INT
            AS $$
            BEGIN
            
            RETURN num *2;
            END;
            $$ LANGUAGE PLPGSQL;
            ```
            
        - Eliminar una funcion
            
            ```sql
            DROP FUNCTION mifuncion(INT)
            --si tiene 2 parametros
            DROP FUNCTION mifuncion(INT,INT)
            ```
            
        - Crear una tabla en una funcion(mayormente temporales o que tienen una duracion definida)
            
            ```sql
            CREATE O REPLACE FUNCTION creartabla() --replace esta de ejemplo
            RETURNS text
            AS $$ --el simbolo de dolar es como el cuerpo de la funcion
            BEGIN
            	
            	CREATE TABLE tablaprueba(
            		nom vachar(45)
            		edad int
            	)	
            
            RETURN "tabla creada";
            END;
            $$ LANGUAGE PLPGSQL;
            ```
            
        - Agregar condicionales
            
            ```sql
            CREATE OR REPLACE FUNCTION creartabla()
            RETURNS text --el tipo de dato que nos va a devolver
            AS $$ --es el cuertpo de la funcion(tiene que llamarce igual en el inicio y el final "incluyendo mayus y minus")
            declare variable TEXT;
            BEGIN-- este es el comiezo de la funcion; despues de ella es lo que queremos que ejecute esta funcion
            	
                
                IF EXISTS(select tablename FROM pg_tables where tablename='tablaprueba')
                    THEN 
                        variable:='Drop table tablaprueba;';
                        execute variable;
                end if;
                RAISE notice 'borrando la tabla prueba';
                
                 IF NOT EXISTS(select tablename FROM pg_tables where tablename='tablaprueba')
                    THEN 
                        variable:='CREATE TABLE tablaprueba(
            		            nom varchar(45),
            		            edad int
            	                );';
                        execute variable;
                 end if;
                 raise notice 'creando la tabla prueba';
                
                insert into tablaprueba(nom,edad)values('jose',23);
                raise notice 'insercion de datos';
            
                alter table tablaprueba add column telefono varchar(12);
                raise notice 'se añadio una nueva tabla';
            
            RETURN 'tabla creada';
            END;--el fi de la funcion
            $$ LANGUAGE PLPGSQL;--para programar nuestras funciones en postgre
            
            select creartabla()
            ```
        - Obtener el valor del parametro

            ```sql
            CREATE OR REPLACE FUNCTION creartabla(Num integer)
            RETURNS text
            AS $$
            declare 
                TEXT:=(select * from productos where id=$1); ---'$1 obtener lo que me llega por parametro; $2 seria el 2do paramtro y asi sucesivamente'
            ```
        - Tipo RECORD(marcador de posicion) NO CLARO "es guardar una fila en una tabla llamada record?"
                
            nos sirve para recorrer las filas de una tabla
            ```sql
                CREATE OR REPLACE FUNCTION creartabla()
                RETURNS text
                AS $$
                declare 
                    r RECORD;
                BEGIN
            ```

        - SETOF NO CLARO
            
            conjunto de  en filas?

        - PARAMETROS DE SALIDA 

            ```sql
            CREATE OR REPLACE FUNCTION creartabla(OUT res TEXT)--sirven para ver si hay algun tipo de problema

            ```
            
        - Funcion para hacer BACKUPS de una tabla

            ```sql
            create or replace function backupsTabla()returns boolean as 
            $Body$
            declare
            begin
                copy(select*from componentes) to 'C:\backups/productos.csv'
                csv header delimiter ';';
            return true;
            end;
            $Body$

            language plpgsql;

            --ejecutable select backupsTabla()

            --para crear un new user
            create or replace function newregisteruser(varchar,varchar,varchar,numeric,varchar,varchar,varchar,varchar,timestamp,varchar)
            returns boolean 
            as $body$
            begin
            insert into componentes values(default,$1,$2,$3,$4,$5,$6,$7,$8,$9,$10);
            return true;
            end
            $body$
            language plpgsql;
            ```



         - Tipos de datos compuestos

        
        - condicionales con excepcion
            ```sql
                select*From productos where id=$1
             if not found then --quiere decir si no se encuentra el registro
                raise exception 'No se registro con exito';
            end if;
            ```

        - Sentencia select*into
            ```sql
              select *into prod from productos where id=1 -- Selecciona las filas definidas por una consulta y las inserta en una nueva tabla. Puede especificar si va a crear una tabla temporal o persistente
            ```
        -Clase 8

        - Crear vistar

            Es almacenar una consulta con un nombre y llamarla como si fuera una tabla normal; mayormente almacenar consulta que muestro los datos de varias tablas
            ```sql
            --Crear una vista
                create view comandsubcat as
                select s.nombre nombpro, c.nombre nombresubcat from componentes c join subcategoria s on c.subcategoria_id=s.id

                --llamadad select*from comandsubcat

            --Borrar una vista
            drop view comandsubcat
            ```

        -Clase 9

        - Crear función que acepte un archivo como parámetro

        - Limpiza de datos

        -Clase 10

        - TRIGGERS
            son como funciones que se ejecutan despues o antes de ejecutar una sentencia "insert, update, delete, select?" 
            ```sql
                create or replace function insertprod() 
                returns trigger as --returnamos un trigger
                $body$
                begin
                    
                    if length(new.nombre)>10 or new.nombre is null then --new. significa que cogera el nuevo registro al ejecutar una sentencia
                    --if length(old.nombre)>10 or old.nombre is null then new. significa que cogera el anterior registro al ejecutar una sentencia
                        raise exception 'no se acepta esta cantidad de letras';--mensaje de error
                    else
                    raise exception 'ta bien'; --no es necesario
                    end if;
                    return new; --significa el ya puede realizar la accion con la nueva informacion
                end;
                $body$
                language plpgsql;

                /*Creacion del trigger
                le decimos que se va a ejecutar antes de hacer un insert o un update en la tabla categoria
                por cada fila o registro que se realice ejecute la funcion*/
                create trigger tg_nombre before insert or update 
                on categoria for each row execute procedure insertprod()

                -- insert into categoria values(default,1,'ssssssssssssssssssss',null,null,'A',null);
            ```
         - Extra

            ```sql
                /*  variables implícitas? */
                --guarda las sentencias de sql "insert, update, delete"
                TG_OP 
                --ejemplo
                if(TG_OP='INSERT') --en mayuscula?
            ```
              ```sql
               /* variables en funciones */
                create or replace function backupsTabla()returns boolean as 
                $Body$
                --se declaran antes del begin
                declare prod text; --podemos guardar consultas, registros, cualquier cosa? y tambien asignarle su tipo de dato
                begin
                    prod:=select*From productos --asi se le da valor a la variable
                return true;
                end;
                $Body$

                language plpgsql;
            ```
            <a href="https://www.youtube.com/watch?v=LPtF87C3LOA link" target="_blank">Link del video</a>

               
            ```sql
               /* Expresiones Regulares*/
               select regexp_replace('F#l#o$r%','[^a-zA-Z0-9]','','g');

               --pasar string  con metodos de postgre

               --agregar una ' de mas para que no afecte en nada, guardando sin problema la ' ingresada
                insert into categoria values(default,1,'sss''ss',null,null,'A',null);
                -- se guarda asi sss'ss

                --barra invertida  '\' no se puede csmr


            ```