## StreamProvider
- StreamProvider es similar a FutureProvider pero aplicado a Streams en lugar de [Futures].

StreamProvider normalmente es utilizado para:

- escuchar a Firebase o web-sockets.
- reconstruir otro proveedor cada pocos segundos.

> Dado que los Streams exponen naturalmente una manera de escuchar las actualizaciones, algunos podrían pensar que utilizar un StreamProvider tiene poco valor. De hecho, podría pensar que los StreamBuilder de Flutter funcionarían de la misma manera al momento de escuchar un Stream, pero esto es un error.



El uso de un StreamProvider en lugar de StreamBuilder tiene numerosos beneficios:

- permite que otros proveedores escuchen al stream, utilizando ref.watch.
- asegura que los casos de carga y error se manejen correctamente, gracias a AsyncValue.
- elimina la necesidad de tener que diferenciar los streams de transmisión (broadcast streams) de los streams normales.
- almacena en caché el último valor emitido por el stream, lo que garantiza que si se agrega un oyente luego de - emitirse un evento, el oyente seguirá teniendo acceso inmediato al evento más actualizado.
- permite simular (mocking) fácilmente el stream durante las pruebas, anulando el StreamProvider.