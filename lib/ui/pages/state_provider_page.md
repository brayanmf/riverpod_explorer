## stateProvider

> StateProvider es un provider que expone una forma de modificar su estado. Es una simplificación de StateNotifierProvider, diseñada para evitar tener que escribir una clase StateNotifier para casos de uso muy simples.


se utiliza tipicamente para :

- un enum, como un tipo de filtro
- un String, normalmente el contenido plano (raw) de un campo de texto
- un boolean, para casillas de verificación
- number, para paginación o campos de formulario de edad

No debe usar StateProvider si:

- su estado necesita lógica de validación
- su estado es un objeto complejo (como una clase personalizada, List/Map, ...)
- la lógica para modificar su estado es más avanzada que un simple count++.