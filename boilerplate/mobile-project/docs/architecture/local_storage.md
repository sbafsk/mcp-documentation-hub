# Local Storage Strategy (Mobile)

> **AI Context**: This is the single source of truth for local storage design.
> For current status: see `../status/progress.yaml`
> For system overview: see `overview.md`

## Options

- **Hive**: Lightweight, fast key-value store with adapters
- **SharedPreferences**: Simple key-value for primitives
- **SQLite**: Structured relational storage (sqflite)
- **Secure Storage**: Keychain/Keystore for secrets

## Patterns

- **Repository Abstraction**: Hide storage behind interfaces
- **Adapters**: Hive type adapters for models
- **Migrations**: Versioned schema/data migrations
- **Caching**: Strategy (LRU, TTL), invalidation rules

## Example (Hive)

```dart
@HiveType(typeId: 1)
class User extends HiveObject {
  @HiveField(0)
  String id;
  @HiveField(1)
  String name;
  @HiveField(2)
  String email;
}
```
