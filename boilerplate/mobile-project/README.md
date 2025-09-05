# 🚀 [Project Name] - Mobile Application

**Mobile application built with Flutter (Dart) and AI-friendly documentation**

---

## 📚 Documentation & Getting Started

**🎯 For complete project documentation, start with `docs/index.md`**

This root README provides quick setup instructions. For comprehensive documentation including:
- Current project status and progress
- Development standards and guidelines
- MCP integration setup
- Technical implementation details

**→ Go to `docs/index.md` for the complete picture**

## 🛠️ Tech Stack (Template)

- **Mobile**: Flutter (Dart)
- **State Management**: Riverpod (recommended) or BLoC
- **Navigation**: GoRouter or Navigator 2.0
- **Networking**: Dio or http
- **Local Storage**: Hive, SharedPreferences, or SQLite
- **Testing**: Unit, Widget, Integration tests
- **CI/CD**: GitHub Actions (template included)

## ✅ Recommendation: Riverpod vs BLoC

- **Choose Riverpod** if you prefer a simpler, modern solution with compile-time safety and flexibility. Great default for most projects and teams.
- **Choose BLoC** for very large/complex apps requiring strict separation of concerns and event-driven flows.

## 🚀 Quick Start

### Prerequisites
- Flutter SDK (stable)
- Android Studio / Xcode (toolchains)
- VS Code (optional)

### 1. Install & Verify
```bash
flutter doctor
flutter pub get
```

### 2. Development
```bash
# Run app (device/emulator)
flutter run

# Analyze & test
flutter analyze
flutter test
```

## 📁 Project Structure

```
[project-name]/
├── .ai/                    # AI-specific configuration
├── docs/                   # Project documentation
│   ├── status/            # Current state only
│   ├── architecture/      # Technical implementation
│   └── guides/            # How-to documentation
├── standards/              # Immutable standards
└── lib/                   # Flutter source code
```

## 🤖 AI Development Assistance

- **MCP Integration**: 2-server setup (docs + standards). See `.ai/mcp-config.json`.
- Follow SSOT principles: status in `docs/status/`, overview in `docs/index.md`.

## 📦 GitHub Actions CI

A ready-to-use CI workflow is included at `.github/workflows/ci.yml`:
- Checkout, Flutter setup, analyze, tests, Android/iOS builds

## 📚 Documentation Links

- Flutter docs: https://docs.flutter.dev/
- Dart docs: https://dart.dev/docs

---

## 🔄 Documentation Status

- **Last Updated**: [Current Date]
- **Documentation**: ✅ **CONSOLIDATED** - Single source of truth in `docs/index.md`
- **MCP Integration**: ✅ Ready for implementation

**📚 For complete documentation, start with `docs/index.md`**
