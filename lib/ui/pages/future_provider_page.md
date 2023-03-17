## FutureProvider

> FutureProvider es el equivalente de Provider pero para código asíncrono.


FutureProvider se usa típicamente para:

- realizar y almacenar en caché operaciones asíncronas (como solicitudes de red)
- manejar bien los estados de error/carga de las operaciones asincrónicas
- combinar múltiples valores asíncronos en otro valor

FutureProvider va muy bien cuando se combina con ref.watch. Esta combinación permite la recuperación automática de algunos datos cuando cambian algunas variables, lo que garantiza que siempre tengamos el valor más actualizado.

- combinacion con provider y futureprovider :]

```
final apiServiceProvider = Provider<ApiService>((ref) => ApiService());

class ApiService {
  Future<Suggestion> getSuggestion(String id) async {
    try {
      final res = await Dio().get('https://www.boredapi.com/api/activity');
      return Suggestion.fromJson(res.data);
    } catch (e) {
      throw Exception('Error getting suggestion');
    }
  }
}

```



usando el provider  apiServiceProvider
```
final suggestionFutureProvider =
    FutureProvider.autoDispose.family<Suggestion, String>((ref, id) async {
  final apiService = ref.watch(apiServiceProvider);
  return apiService.getSuggestion(id);
});

```



en nuestro widget lo usamos de esta manera 



```
     suggestionRef.when(data: (data) {
              return Text(
                data.activity,
                style: Theme.of(context).textTheme.headline4,
              );
            }, error: (error, _) {
              return Text(error.toString());
            }, loading: () {
              return const CircularProgressIndicator();
            }),
```