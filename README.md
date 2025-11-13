TyG-ABSI Calculator

A Flutter app that calculates TyG-ABSI from simple inputs so non-experts can understand their metabolic and body shape indicators. Built with Flutter and Riverpod.

Features

Simple inputs with unit selection

Auto conversions for glucose and triglycerides

Step-by-step calculations: BMI, TyG Index, ABSI, TyG-ABSI

Live results card with risk color states and short tips

Preset chips for quick demo values

Clean Material 3 UI with responsive layout

Flutter Web ready

Screenshots

Add screenshots here

assets/screenshots/home_desktop.png
assets/screenshots/home_mobile.png

Formulas

Unit conversions

Height: meters = centimeters ÷ 100

Triglycerides: mg/dL = mmol/L × 88.57

Glucose: mg/dL = mmol/L × 18

BMI

BMI = weight_kg / (height_m)^2


TyG Index

TyG = ln( (Triglycerides_mgdl × Glucose_mgdl) / 2 )


ABSI
App uses the cm version for consistency with the UI.

ABSI_cm = Waist_cm / ( BMI^(2/3) × sqrt(Height_cm) )


If you prefer literature alignment, use meters everywhere:

ABSI_m = Waist_m / ( BMI^(2/3) × sqrt(Height_m) )


TyG-ABSI

TyG_ABSI = TyG × ABSI

Risk bands used in the app

These bands help color the score card and show a short tip.

TyG-ABSI < 0.65 → Low risk

0.65 to < 0.70 → Borderline

0.70 to < 0.76 → Elevated

≥ 0.76 → High risk

Additional red flag applied if both TyG > 9.04 and ABSI > 0.085.

Note: These bands are for educational feedback, not diagnosis.

Tech stack

Flutter stable

Riverpod 3 for state management

Material 3 design

GitHub Actions for CI and web deploy

Project structure
lib/
├── main.dart
├── theme/
│   └── app_theme.dart
└── features/
└── tyg_absi/
├── domain/
│   ├── models.dart
│   ├── conversions.dart
│   ├── calculations.dart
│   ├── risk.dart
│   └── controllers.dart
├── application/
│   └── tyg_absi_notifier.dart
└── presentation/
├── tyg_absi_screen.dart
└── widgets/
├── animated_header_backdrop.dart
├── blood_input_row.dart
├── body_inputs.dart
├── demographics_inputs.dart
├── header_score_card.dart
├── input_field.dart
├── preset_chips.dart
├── result_card.dart
├── section_card.dart
└── note.dart

Getting started
Prerequisites

Flutter SDK installed and on path

Dart SDK bundled with Flutter

Chrome for web testing

Install dependencies
flutter pub get

Run on web
flutter run -d chrome

Run on Android or iOS

Set up device or emulator, then:

flutter run

Build for production
Flutter Web release build
flutter build web --release --web-renderer html
# If deploying to GitHub Pages project URL:
# flutter build web --release --web-renderer html --base-href /<repo-name>/


Use --web-renderer html for faster initial load.

Use --web-renderer canvaskit for better canvas quality if needed.

Deploy to GitHub Pages

Automatic deploy on every push to main.

Create workflow file in your repo at .github/workflows/flutter-web-deploy.yml:

name: Deploy Flutter Web to GitHub Pages

on:
push:
branches: [ main ]

permissions:
contents: write

jobs:
build-deploy:
runs-on: ubuntu-latest
steps:
- uses: actions/checkout@v4

      - uses: subosito/flutter-action@v2
        with:
          channel: stable

      - name: Flutter pub get
        run: flutter pub get

      - name: Build web (HTML renderer)
        run: flutter build web --release --web-renderer html --base-href /${{ github.event.repository.name }}/

      - name: Add 404 fallback
        run: cp build/web/index.html build/web/404.html

      - name: Deploy to gh-pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: build/web
          publish_branch: gh-pages


In GitHub Settings → Pages

Source: Deploy from a branch

Branch: gh-pages at root

Save

Open your site
https://<username>.github.io/<repo-name>/

If you use a custom domain, add CNAME in build/web before deploy and switch build to --base-href /.

Configuration notes

The app accepts triglycerides and glucose in mg/dL or mmol/L and converts internally to mg/dL for TyG.

Height is entered in cm but BMI is computed using meters converted from cm.

ABSI is computed using cm in the UI for consistency. If you change to meter-based ABSI, adjust any thresholds accordingly.

Development tips

Keep parsing and validation in the notifier layer.

Round numbers only at display time.

Use Provider for derived results and StateNotifier for input mutations.

Prefer ConstrainedBox(maxWidth: 960) with a centered container for desktop layouts.

Disclaimer

This app is for education and research support. It is not a medical device. It does not provide medical diagnosis or treatment. Users should consult qualified health professionals for clinical decisions.

License

Choose a license for your repo, for example MIT:

MIT License
Copyright (c) {year} {your-name}

Contributing

Issues and pull requests are welcome. Please open an issue to discuss major changes.