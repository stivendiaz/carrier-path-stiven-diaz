# Callback Function in Javascript

## Functions are Objects

The first thing we need to know is that in Javascript, functions are first class objects. As such, we can work with them in the same way that we work with other objects, such as assigning them to variables and passing them as arguments to other functions. This is important, because this last technique allows us to extend the functionality of our applications.

## Callback functions
A callback function is one that is passed as an argument to another function to be "called back" at a later time. A function that accepts other functions as arguments is called a High-Order function, and contains the logic to determine when the callback function is executed. It is the combination of these two that allows us to extend our functionality.

Short example:

```
function crearCita(cita, callback){ 
  var miCita = "Como yo siempre digo, " + cita;
  callback(miCita); // 2
}

function logCita(cita){
  console.log(cita);
}

crearCita("come tus vegetales!", logCita); // 1
```