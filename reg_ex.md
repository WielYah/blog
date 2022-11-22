# Regulares

## Precio

```js
    /^\d{0,8}(\.\d{1,2})?$/
    ^->//validar que todo el texto que vamos a valdiar se aplique al inicio de la cadena de texto 
    \d{0,8}->//esperamos que sea un digito entre 0 y 8, tiene queser 8 cifras (8 caracteres)
    \.//requerimos un "."
    \d{1,2}// que nuestro precio debe llevar entre 1 o 2 decimales
(\.\d{1,2})-> //? validamos que puede apareceer o no, es decir puede que una cantdad pueda o no llevar decimales
    $->//se aplique la expresion hasta el final de la cadena r

```

## Numeros enteros
```js
 /^\d+$/
 \d-> //nos indica que necesitamos un digito
+ //indica que eese numero puede existir una o mas veces
```

## Numeros decimales
```js
 /^\d*\.\d+$/
 \d*->//esperamos que este valor este presente ninguna vez o varias veces; puede o no existir
 \.//tiene que haber un "."
\d+// que quiero un digito que se repita varias veces
```

## Numeros enteros y decimales
```js
 /^\d*(\.\d+)?$/
    *(\.\d+)?// este grupo quiere decir que exista uno o nada
```

## Numeros enteros positivos y negativos
```js
 /^-?\d*(\.\d+ )?$/
    -?// esperamos que exista o no este signo
```

## Alfanumericos sin espacios
```js
    // no esperamos que la cadena no tenga espacions en cualquier posicion
    /^[a-zA-Z0-9]*$/
    //minuscula o minuscula y numeros del 1 -9
```

## Alfanumericos con espacios
```js
    // no esperamos que la cadena no tenga espacions en cualquier posicion
    /^[a-zA-Z0-9 ]*$/
    // hay un espacio despues del 9 que quiere decir que tambien es un caraacter valido 
```

## Correo electronico
```js
    /^([a-z0-9_\.\+-]+)@([\da-z\.-])\.([a-z\.]{2,6})$/
    _\.\+- // acepta guiones bajos, puntos, operador de suma  y giones medios
``` 

## password fuerte
```js
    /(?=(.*[0-9]))(?=.*[\!@#$%^&*()\\[\]{}\-_+=~`|:;"'<>,./?])(?=.*[a-z])(?=(.*[A-Z]))(?=(.*)).{8,}/
   
   
   '`(?=(.*[0-9]))// ?=en contrar la regex de mi 2do parentesis en cualquier posicion de la cadena, si no lo colocamos pensara que siempre tiene que estar al inicio; encontrara la posicion ya sea al inicio o al final
   .//puede existir cuaqlquier caracter * que puede haber un caracter o muchos
   
   (?=.*[\!@#$%^&*()\\[\]{}\-_+=~`|:;"'<>,./?])   '`//entre los [] estan los caracteres especiales que podemos utilizar
   (?=//marca la posicion donde se ecuentra el patron del grupo 2

(?=.*[a-z])(?=(.*[A-Z]))
(?=//marca la posicion donde se ecuentra el patron del grupo 3 y 4 el siguiente ()

(?=(.*)).{8,}
//?= al final de la cadena
//{8,} que tenga al menos 8 caracteres
``` 

## Nombre del usuario
```js
/^[a-zA-Z0-9_-]{3,16}$/
```

## URL con HTTP O HTTPS
```js
/https?\/\/(www\.)?[-a-zA-Z0-9@:%._+~#=]{2,256}\.[a-z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#()?&//=]*)

// \b ->que sea una plabra y no haya espacios
// \/-> cuando hay una \ le estamos diciendo que queremos el caracter literal este en este caso->/
```


## Fecha
```js
/([12]\d{3}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01]))/
//[12] escoger entre 1 y 2 cualquiera
```

## Hora HH:MM, 12 hrs,0 opcional con am o pm

```js
/((1[0-2]|0?[1-9]):([0-5][0-9]) ?([AaPp][Mm]))/
```

## Hora HH:MM, 24 hrs,0 no opcional

```js
/^(0[0-9]|1[0-9]|2[0-3]):[0-5][0-9]$/
```