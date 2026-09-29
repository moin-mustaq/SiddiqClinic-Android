# Siddiq Clinic v71 — Android Edition

This project is an Android port of the supplied Siddiq Clinic v71 Tkinter application.

## Architecture

Tkinter cannot run as a native Android UI. The Android edition therefore reimplements the UI as a local HTML/CSS/JavaScript application rendered inside Android WebView, while keeping the supplied SQLite data model and implementing native Android services for:

- SQLite database access
- A4 printing through the Android print framework
- Database backup/restore through Android file/share pickers
- CSV/XLSX exports through Android sharing
- WhatsApp hand-off for patient/bill messaging

The application is offline-first and does not require a server for the clinic data.

## Included v71 functionality

The port includes the supplied v71 workflows for patient registration/visit history, admission, OPD billing, LAB billing, bill records and voiding, checkout/consolidated bill, patient dashboard, doctors, staff, bill categories, appointments, pharmacy, reports/analysis, hospital profile, users, audit log, backup/restore, prescription templates, Doctor Patient List, clinical options, and doctor consultation/prescription printing.

The v71 prescription print removes the doctor-signature block and uses the v71 right-side clinical boxes and A4 layout approach.

## First-run database

The supplied `TEST.db` is copied into the app's private storage on first launch. It contains the v71 schema, default profiles/templates, and the seeded admin account from the supplied application.

For a production deployment, replace the initial database asset with the clinic's real v71 database before the first installation, or use the in-app Restore flow after installation.

## Android differences from the Windows Tkinter edition

Windows-only printing APIs (`win32print`, `win32ui`, `os.startfile`, PowerShell/Acrobat printer launching, and direct Windows printer routing) are replaced with Android's system print dialog. On Android, choosing a printer or “Save as PDF” is handled by the operating system.

The original Tkinter widgets are not reused; the Android UI is responsive and touch-oriented while preserving the workflows and labels.

## Build the APK

A GitHub Actions workflow is included at `.github/workflows/build-apk.yml`.

It builds `app/build/outputs/apk/debug/app-debug.apk` and uploads it as the workflow artifact `SiddiqClinic-v71-debug-apk`.

### Local Android Studio build

Open the project folder in Android Studio, let Gradle sync, then run **Build → Build APK(s)**. The project targets Android API 35 and uses Gradle 8.7 / Android Gradle Plugin 8.5.2.

### Command-line build

On a machine with Android SDK, Java 17 and Gradle 8.7:

```bash
gradle :app:assembleDebug
```

The resulting installable APK is:

```text
app/build/outputs/apk/debug/app-debug.apk
```

## Important validation status in this environment

The original v71 Python package was compiled/smoke-tested, the Android JavaScript layer passes `node --check`, and the Android project is packaged with its database/assets. The current execution environment does not contain the Android SDK/build tools and has no usable outbound package download path, so the binary APK cannot be compiled inside this sandbox. The included GitHub Actions workflow is ready to perform that Android build on a GitHub-hosted runner.
