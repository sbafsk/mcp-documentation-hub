# API Integration (Mobile)

> **AI Context**: This is the single source of truth for API integration patterns.
> For current status: see `../status/progress.yaml`
> For system overview: see `overview.md`

## HTTP Client

- **Choice**: [Dio | http]
- **Interceptors**: Logging, auth token, retry
- **Timeouts**: Connect/read/write timeouts configured

## Request Patterns

- **Base URL**: Configure environments (dev/staging/prod)
- **Headers**: JSON, auth tokens, locale
- **Error Handling**: Map network errors to domain exceptions

## GraphQL (Optional)

- **Client**: Ferry / graphql_flutter
- **Cache**: Normalized cache, policies
- **Auth**: Link composition for headers/tokens

## Repository Layer

- **Abstraction**: Repositories hide transport details
- **DTOs & Models**: json_serializable / freezed
- **Result Type**: Either/Result for success/error

## Example (Dio)

```dart
final dio = Dio(
  BaseOptions(
    baseUrl: const String.fromEnvironment('API_BASE_URL', defaultValue: 'https://api.example.com'),
    connectTimeout: const Duration(seconds: 10),
    receiveTimeout: const Duration(seconds: 20),
  ),
)..interceptors.addAll([
  LogInterceptor(responseBody: false),
  InterceptorsWrapper(
    onRequest: (options, handler) async {
      final token = await tokenStorage.read();
      if (token != null) options.headers['Authorization'] = 'Bearer $token';
      return handler.next(options);
    },
    onError: (e, handler) {
      // map to domain error
      return handler.next(e);
    },
  )
]);
```
