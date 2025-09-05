# Deployment Guide (Mobile)

> **AI Context**: This is the single source of truth for deployment processes.
> For current status: see `../status/progress.yaml`
> For system overview: see `../architecture/overview.md`

## Android (Google Play)

### Build
```bash
flutter build appbundle --release
```

### Sign & Upload
- Configure signing in `android/` (keystore, gradle configs)
- Upload `.aab` to Google Play Console

## iOS (Apple App Store)

### Build
```bash
flutter build ipa --release
```

### Sign & Upload
- Configure signing/certificates in Xcode
- Upload via Xcode or Transporter

## CI/CD

- Codemagic / GitHub Actions / Bitrise templates
- Cache dependencies, run tests, build artifacts

## Environment Variables

- Store secrets securely (CI secrets, not in repo)
- Platform-specific config handling

## Monitoring & Rollback

- Crashlytics/Sentry setup
- Staged rollouts, revert plans
