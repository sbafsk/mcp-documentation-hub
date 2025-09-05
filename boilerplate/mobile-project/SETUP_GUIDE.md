# 🚀 Mobile Project Boilerplate Setup Guide (Flutter)

**Complete guide for setting up a new mobile project using this AI-friendly documentation boilerplate**

---

## 🎯 What This Boilerplate Provides

- 📋 AI-Optimized Architecture (SSOT + machine-readable status)
- 🤖 MCP Integration (docs + standards servers)
- 📚 Structured Documentation (overview, status, architecture, guides)
- 🧩 Flutter/Dart Patterns (state mgmt, API, storage)

---

## 🚀 Quick Setup (5 Minutes)

### Step 1: Copy Boilerplate
```bash
cp -r boilerplate/mobile-project/* your-mobile-project/
cd your-mobile-project
```

### Step 2: Customize Project Name
```bash
find . -type f -name "*.md" -exec sed -i 's/\[Project Name\]/Your Project Name/g' {} \;
```

### Step 3: Install & Verify
```bash
flutter doctor
flutter pub get
```

### Step 4: Initialize Git
```bash
git init
git add .
git commit -m "Initialize Flutter mobile docs boilerplate"
```

---

## 🔧 Detailed Customization

### 1) Update Project Information
- `.ai/context.yaml` → update project metadata and references
- `docs/index.md` → overview, status links, tech stack
- `docs/status/progress.yaml` → status placeholders

### 2) Choose State Management
- Default recommendation: **Riverpod**
- Option: **BLoC** (for strict SoC/event-driven apps)

### 3) API & Storage
- Pick **Dio** or `http`
- Choose storage: **Hive**, **SharedPreferences**, or **SQLite**

### 4) CI/CD
- GitHub Actions template at `.github/workflows/ci.yml`
- Add signing steps when preparing for releases

---

## 📁 File Structure Overview

```
your-mobile-project/
├── README.md
├── SETUP_GUIDE.md
├── .ai/
│   ├── context.yaml
│   ├── mcp-config.json
│   └── agent-instructions.md
├── docs/
│   ├── index.md
│   ├── status/
│   │   ├── progress.yaml
│   │   └── priorities.md
│   ├── architecture/
│   │   ├── overview.md
│   │   ├── api.md
│   │   └── local_storage.md
│   └── guides/
│       ├── setup.md
│       └── deployment.md
└── standards/
    ├── coding.md
    └── patterns.md
```

---

## ✅ Next Steps

1. Update status in `docs/status/progress.yaml`
2. Define priorities in `docs/status/priorities.md`
3. Initialize app code in `lib/`
4. Set up MCP in Cursor/IDE and test

---

## 📚 References

- Flutter docs: https://docs.flutter.dev/
- Dart docs: https://dart.dev/docs
