# Environment Setup Guide - [Project Name] (Mobile)

> **AI Context**: This is the single source of truth for environment setup.
> For current status: see `../status/progress.yaml`
> For project overview: see `../index.md`

## Prerequisites

- **Flutter SDK**: stable channel
- **Dart**: included with Flutter
- **Android Studio / Xcode**: platform toolchains
- **VS Code** (optional): Flutter/Dart extensions

## Quick Setup (5 minutes)

### 1. Install Flutter
Follow official guide: https://docs.flutter.dev/get-started/install

### 2. Verify Environment
```bash
flutter doctor
```

### 3. Install Dependencies
```bash
flutter pub get
```

### 4. Run App
```bash
flutter run
```

## Environment Configuration

- `.env` approach via `flutter_dotenv` (optional)
- Flavor-based configs (dev/staging/prod) with build-time variables

## Development Scripts

```json
{
  "scripts": {
    "analyze": "flutter analyze",
    "test": "flutter test",
    "build:android": "flutter build apk --release",
    "build:ios": "flutter build ipa --release"
  }
}
```

## Troubleshooting

- Update Flutter: `flutter upgrade`
- Clear caches: `flutter clean && flutter pub get`
- Android licenses: `flutter doctor --android-licenses`
