# Mongo DB

### Crear una db
```js
    use newDB //si listamos no se guardara, paraa que una base de datos se guarde necesitamos agregarle al menos una coleccion con documentos
    db //para saber en que db me encuentro
    show dbs //listar nuestras BD
```

### Eliminar una BD
```js
    db.dropDatabase()// elimina la base de datos actual (la que estamos usando)
```

### Crear una coleccion con documentos
```js
    db.productos.insertOne({"nombre":"jose"});
    //si deseo pegar una collecion ya creada necesito oprimir click derecho y cerrar el )
   
```


### Crear una coleccion con documentos
```js

//insert y update estan en desuso seria insertOne y updateOne

    show collection // para listar las colecciones
    db.createCollection("users") //creare solo una collection
    db.users.drop(); //eliminar una collection
    db.productos.insertOne({"nombre":"jose"}); // crear una collection con su documento solo para un docuemnto, devolviendonos el id nuevo
    db.products.insertMany([ //insertar varios documentos devuelte un array con los id nuevos
            {
                "nombre":"laptop",
                "precio":50,
            },
            {
                "nombre":"monitor",
                "precio":100,   
            }
        ])

    db.productos.find() //lista todos los docuemntos de esta colleccion
    db.products.find({nombre:"monitor",precio:50})//Buscar un documento especifico; en este caso lo buscamos por la prop nombre y el precio; devuelve un curso
    db.products.findOne({nombre:"monitor"}) //obtener de todas las coincidencias el primero; devuelve un documento
    db.products.findOne({nombre:"monitor"},{precio:1,nombre:0,id:0})//buscar un documento y que me devuelva determinadas propiedades, gracias a 0 y 1 determino si se van a mostrar o no
    db.products.find().limit(2) //estableceer un limite
    db.products.find().foreach(pro=>print(pro.nombre))   //utilizar funciones
 
    db.products.count() //ver cuantos datos tengo, cuantos documentos

    /*   db.products.update({nombre:"jose"},{Apellido:"maer"})// tiene 2 operacions (busqueda y reeemplazo); ojo reemplazara todo el documeto(aunque no me salio xd) */

    db.products.update({"prod":"2"},{$set:{"nombre":"m"}})// actualizar mi documneto agregando otra propiedad; gracias al "$set"
    db.products.update({"prod":"333"},{$set:{"nombre":"m2222"}},{upsert:true}) //actualizar un dato aunque no exista? gracias a un 3er parametro "{upsert:true}
    db.products.updateOne({nombre:"laptop"},{$inc:{"precio":10}})//incrementar un valor numerico gracias al valor de "$inc" como 2do arametro
    db.products.updateOne({"nombre":"laptop"},{$rename:{"nombre":"name"}}) //renombrar como se llama la propiedad gracias a $rename del 2do parametro

     //eliminar un registro o un documento
    db.products.remove({"prod":"2"})//deprecado
     db.products.deleteOne({"prod":"2"})  
    //eliminar todos los documentos
    db.products.remove({})

```


### Tipos de datos
```js
   {
    "nombre":"laptop",
    "precio":50,
    "active":true,
    "create_at":new Date("12/02/2000"),
    "regex":/laptop/ig, //expresiones regulares?
    "listData":[1,"a",[]],
    "facturacion":{
        "name":"dell",
        "localitation":{
            "ciudad":"usa"
        }
    }
   }
```

# Datos adicionales
```js
    //mongo db convierte nuestros json a BSON para que la respuesta sea mas rapida
    //no tiene estrucctura, puede tener datos(o propiedades) de mas a comparacion de la fila o ducumento anterior; es decir tomando en base sql tiene columnas de mas
```
# Ejercicios

```js
    //obtener por condicion que el precio sea mayor que 10
    db.productos.find({
        precio:{$gt:10} //para condicionar utilizando un operador condicional necesitamos  "$" y gt "mayor que"
    })

    //operadores condicionales
    /* 
    gt ->mayor que >
    gte mayor igual >=
    lt menor que <
    lte menor igual <=
    ne diferente !=
    */ 
```

```js
    //contar la cantidad de documentos que tiene
    db.productos.find({
        precio:{$gt:10} 
    }).count()    
```

```js
    //Condicionar sobre 2 atributos; para esto utilizamos el operador logico AND
    db.productos.find({
        $and:[
            {precio:{$gt:10}},
            {estado:true}
        ]
    })   
```

```js
    // utilizamos el operador logico OR(basta que se cumpla una condicion para poder obtener los documentos)
    db.productos.find({
        $or:[
            {precio:10},
            {precio:5},
            {precio:30},
        ]
    })   

    //en el caso de buscar con mas considciones? "in" permite buscar en un listado
      db.productos.find({
        precio:{$in:[10,5,30]} //buscamos sobre un listado de numeros enteros y no comparando diferentes valores con el mismo atributo como en el "or"
    }) 

```


```js
    //obtener los registros o documentos que tengan o exista un atributo especifico gracias al metodo $exist; muy importante
    db.productos.find({
        estado:{$exist:true}// todos los documentos  que tengan el atributo estado
    })

     db.productos.find({
        estado:{$exist:false}// todos los documentos  que no tengan el atributo estado
    })

```

```js
    //obtener los registros o documentos con estado true
    db.productos.find({
        status:true
    })

    //la manera mas recomendable?; verifico si existe y verifico si su valor es true
    db.productos.find({
       $and:[
            {status:{$exist:true}},
            {status:true}
       ]
    })

```

```js
    //obtener los registros o documentos con mayor precio
    /* utilizamos fins(ya que devuelve un obj cursor y este tiene un metodo sort(); permite ordenar)
 */
    db.productos.find().sort({
        age:-1 //ordenamos de forma descendente
    })


```

# Validar los valores con EXPRESONES REGULARES
```js
 
    db.productos.find({
        email:/.com$/ //le decimos que termine en .com; mysql->like %.com
        email:/^user/ //le decimos que al inicio tiene que tener user; mysql->like user%
         email:/@/ //mysql->like %user%
    })
    //evidentemente pueden llegar a ser mas complejas
```

# Cursores

 es un obj que permite conoceer los doc obtenidos a partir de una consulta
 ```js
    //gracias a
    db.productos.find()//obtenemos todos los docuemntos solo los 20 primeros ya que trabaja por paginacion; para continuar utilizamos "it"

    //lo interesante son los metodos que podemos utilizar 
    db.productos.find().count()//cantidad de dodcumentos obtenidos
    db.productos.find().limit()//limitar los documentos obtenidos
    db.productos.find().skink(2)//saltar documentos pasamos al 3r documento; a su vez retorna un cursor; asi que podemos utilizar sus funciones db.productos.find().skink(2).limit(2)
    db.productos.find().skink(2).pretty()// devulve los documentos con una salida mas legible; formato JSON

    //find, sort, limit, skip -> retornan cursores
    //count , pretty
```
# proyecciones(obtener solo un atributto)
forma en la que podemos obtener atributos de los documnetos  de manera precisa; es como decir las columnas en MYSQL?
 ```js
 //mostrara solo los atributos (nombre y estado en este caso) con sus valores 
    db.productos.find(
        {},
        {
            nombre:true,
            estado:true
        }
    )
```

# Actualizar documentos

 ```js
 // este metodo recibe 2 argumentos, (la condicion y definir los nuevos cambios de mi documento)
    //actualizar mi documento agregando nuevos valores a los atributos o nuevos atributos con sus valores
    db.productos.update(
        {
            _id:213123
        },
        {
           $set:{ //los nuevos valores
                nombre:'new nombre'
           }
        }
    )

    //quitar un atributto al modificar
      db.productos.update(
        {
            _id:213123
        },
        {
           $unset:{ //quitar el atribut
                nombre:true
           }
        }
    )

     //actualizar multiples documentos; necesito un 3er argumento
     db.productos.update(
        {
            estado:false
        },
        {
           $set:{ //quitar el atribut
                estado:true
           }
        },
        {
            multi:true
        }
    )
    o
      db.productos.updateMany(
        {
            estado:false
        },
        {
           $set:{ //quitar el atribut
                estado:true
           }
        }
    )
```

# incrementar el valor nuestros atribiutos con valores de nros enteros

```js
    //incrementar el valor de un atributo de acuerdo a la cantidad que declare
   
    db.productos.updateMany(
        {},
        {
           $inc:{ //quitar el atribut
                cantidad:2
           }
        }
    )

```

# Eliminar documentos
```js
     db.productos.remove({
        _id:21321321321
     })

```

# Ejemplos mas complejos

```js
//para  buscar un documento por el valor de su propiedad
db.productos.find(
    {//condicion
        "direccion.localidad":"PERU" //ingresamos al primer atributo "direccion" y a su atributo "localidad" con el valor "peru" "CONOCIDA COMO DOT NOTATION"
    },
    {
       'direccion.localidad':true //propiedades a mostrar(que la pripiedad y su valor de direccion y localidad se muestren)   
    }
)

//$elemMatch -> nos permite filtrar sobre atributos de documentos dentro de listados; es decir un array; ejemplo
"especificaciones":[
    {
        "title":"asdasd",
        estado:true
    },
    {
        "title":"asdasd",
        estado:true
    }
]
//indicamos que queremos todos los documentos que tengo dentro del listado especificaciones posean el atributo like como true
db.productos.find(
    {
        especificaciones:{
            $elemMatch:{
                estado:true
            }
        }
    }
)

//agregar un nuevo elemento(docuemnto) a mi array especificaciones gracias a $push; puedeo añadir de cualquier tipo de dato

db.productos.updateOne(
    {
        _id:123123
        /* para el uso del comodin $
        'especificaciones.estado:false' //nos permite conocer el indice de los docuementos dentro de la lista que queremos actualizar  
        */
    },
    {
        $push:{
            "especificaciones":{ // tambien puede ser texto solo con "nuevo elemento"
                "title":"asaaaaaaaa",
                 estado:true
            }
            /*añadir a un elemto especifico de un array que es un documento o obj
             "especificaciones.1.estado":{ la posision "1"
                "title":"asaaaaaaaa",
                 estado:true
            } */
            /*si no conocemos el indice utilizamos el comodin "$"; que sera reemplado por el indice de todos los elemntos que cumplan la condicion
             "especificaciones.$.estado":{ la posision "1"
                "title":"asaaaaaaaa",
                 estado:true
            } */
        }
    }
)


```

# Hacer respaldo de mi db

```js
    // para hacer back ups en mongo db necesito instalar los tools: link https://www.mongodb.com/try/download/database-tools y agregarlo a mi path
    // en mi  shell, pero ya tengo que estar en mi carpeta donde hare mis backups 
    /* C:\backups> */  mongodump --db newdb 
    
    //para restaurar necesitamos ingresar al la carpeta de mis backups 
    /* C:\backups> */ mongorestore --db newdb dump/newdb/
```