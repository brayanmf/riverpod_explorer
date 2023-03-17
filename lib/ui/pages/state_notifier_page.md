## StateNotifier

> StateNotifierProvider es un provider que se usa para escuchar y exponer un StateNotifier (del paquete state_notifier, que Riverpod reexporta).
junto con StateNotifier es la solución recomendada por Riverpod para administrar el estado que puede cambiar en reacción a una interacción del usuario.StateNotifierProvider


Normalmente se utiliza para:

- exponer un estado inmutable que puede cambiar con el tiempo después de reaccionar a eventos personalizados.
- centralizar la lógica para modificar algún estado (también conocido como "business logic") en un solo lugar, mejorando la capacidad de mantenimiento con el tiempo.

ejemplo de un carrito.
```
final cartStateNotifierProvider =
    StateNotifierProvider.autoDispose<CartStateNotifier, List<Product>>((ref) {
  return CartStateNotifier();
});

class CartStateNotifier extends StateNotifier<List<Product>> {
  CartStateNotifier() : super([]);

  void addProduct(Product product) {
    state = [...state, product];
  }

  void clearCart() {
    state = [];
  }
}

```
y en el widget 
    `final cart = ref.watch(cartStateNotifierProvider);`

en nuestro list builder

```
  child: ListView.builder(
              itemCount: productList.length,
              itemBuilder: (context, index) {
                final product = productList[index];
                return ListTile(
                  title: Text(product.title),
                  subtitle: Text('\$${product.price}'),
                  trailing: IconButton(
                    icon: const Icon(Icons.add_shopping_cart),
                    onPressed: () {
                      // Add the product to the cart  
                      ref
                          .read(cartStateNotifierProvider.notifier)
                          .addProduct(product);
                    },
                  ),
                );
              },
            )

```

```

AppBar(
        backgroundColor: color,
        title: const Text('ChangeNotifier Provider'),
        actions: [
          Stack(children: [
            //Shopping cart icon with a badge
            IconButton(
              icon: const Icon(Icons.shopping_cart),
              onPressed: () {
                // Show a dialog with the cart contents
                showDialog(
                  context: context,
                  builder: (context) {
                    return AlertDialog(
                      title: const Text('Cart'),
                      content: Column(
                        mainAxisSize: MainAxisSize.min,
                        children: [
                          // Show the cart contents
                          ...cart.map((item) => Text(item.title)),
                          const SizedBox(height: 16),
                          // Sum the total price of the cart
                          Text(
                            'Total: \$${cart.fold<double>(0, (sum, item) => sum + item.price)}',
                            style: Theme.of(context).textTheme.headline6,
                          ),
                        ],
                      ),
                      actions: [
                        TextButton(
                          onPressed: () {
                            // al hacer click aca  llamo a mi provider y limpiar el carrito
                            ref
                                .read(cartStateNotifierProvider.notifier)
                                .clearCart();
                          },
                          child: const Text('Clear'),
                        ),
                      ],
                    );
                  },
                );
              },
            ),
            //Badge
            Positioned(
              right: 0,
              top: 6,
              child: Container(
                padding: const EdgeInsets.all(1),
                decoration: const BoxDecoration(
                  color: Colors.red,
                  borderRadius: BorderRadius.all(Radius.circular(6)),
                ),
                constraints: const BoxConstraints(
                  minWidth: 16,
                  minHeight: 16,
                ),
                child: Text(
                  cart.length.toString(), //el numero de items en el carrito
                  style: const TextStyle(
                    color: Colors.white,
                    fontSize: 12,
                  ),
                  textAlign: TextAlign.center,
                ),
              ),
            ),
          ]),
        ],
      ),
```