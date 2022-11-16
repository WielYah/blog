# Recorrer un array

 ### Foreach
```js
    var array =[
      {nombre:'rosa', edad:12},
      {nombre:'yele', edad:11}
    ]

    array.forEach((valor,indice,array)=>{
        console.log(valor); 	      
    })
```
 ## For in
 ```js
    //existe para recorrer las claves de un obj (PROPIEDAD: VALOR); CUIDADO AL UTILIZAR PARA ARRAYS ya que coge la "propiedad" mas no el "indice"
    var array =[
      {nombre:'rosa', edad:12},
      {nombre:'yele', edad:11}
    ]

    for(let indice in array){
        console.log(valor); 	      
    }
```
 ## For of
  ```js
     //Nos muestra solo el valor de array mas no el indice; sirve para cuando solo necesitamos obtener todos los elementos
    var array =[
      {nombre:'rosa', edad:12},
      {nombre:'yele', edad:11}
    ]

    for(let indice of array){
        console.log(valor); 	      
    }
```

## Metodos para Arrays

###  Map
  ```js
     //Devuelve un nuevo array
    var array =[
        {nombre:'rosa', edad:12},
        {nombre:'yele', edad:11}
    ]

let newarray=  array.map(valor=>{ return {...valor,edad:valor.edad+1}});

console.log(newarray);
```
### Filter
  ```js
     //Devuelve un nuevo array
    var array =[
        {nombre:'rosa', edad:12},
        {nombre:'yele', edad:11}
    ]

    let filtro=array.find(el=>el.nombre==="rosa")
    console.log(filtro);
    //filtrara solo el registro de rosa
```
### Reduce
  ```js
    var array =[1,2,3,4];
    //devuelve la suma de todos los valores del array=> 1+2=3 luego 3+3; es decir la suma del primero con el segundo, el resultado se suma con el tercero y asi sucesivamente 
    //OJO->SIEMPRE TIENEN QUE SER VALORES SIMPLES
    
    let filtro=array.reduce((a,b)=> a+b,0)
    console.log(filtro);
```