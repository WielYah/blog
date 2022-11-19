# Patrones de diseño

Tenemos una funcion que recibe como parametro un estado y dentro de esa funcion tengo otra funcion que verifica si el estado camnbia; si es asi, llamo a otra funcion que vendria a cambiar todos los datos a mostrar
```js
    //algo asi seria?, o como el estado en react
    const funObserver=(estado)=>{
        console.log(`cambio de estado y actualizacion de datos que me llega por parametro ${estado}`);
    }
    
    const funcestado=(estado)=>{
        
        funObserver(estado)
    }


```