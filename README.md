# GitHub Actions Demo

This GitHub Actions workflow is designed to automate the build and testing process for a Flutter application whenever changes are pushed to the `main` or `development` branches, or when a pull request is opened against these branches.

Let's break down the workflow step by step:

### Workflow Triggers

```yaml
on:
  push:
    branches:
      - main
      - development
  pull_request:
    branches:
      - main
      - development
```

- **Triggers**: This workflow triggers on two events:
  - **push**: When code is pushed to the `main` or `development` branches.
  - **pull_request**: When a pull request is opened against the `main` or `development` branches.

### Jobs

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

- **Job**: The workflow defines a single job named `build` that runs on the latest version of Ubuntu.

### Steps

```yaml
steps:
  - name: Checkout Repository
    uses: actions/checkout@v4
```
- **Step 1**: Checks out the code from the repository using the `actions/checkout` action.

```yaml
  - name: Set up Flutter
    uses: subosito/flutter-action@v2
    with:
      channel: stable
      flutter-version-file: pubspec.yaml
```
- **Step 2**: Sets up Flutter environment using the `subosito/flutter-action` action.
  - `channel: stable`: Specifies the Flutter channel to use (stable in this case).
  - `flutter-version-file: pubspec.yaml`: Specifies the location of the `pubspec.yaml` file to determine Flutter version.

```yaml
  - name: Flutter Version Check
    run: flutter --version
```
- **Step 3**: Checks the Flutter version that was set up.

```yaml
  - name: Get Dependencies
    run: flutter pub get
```
- **Step 4**: Fetches the dependencies listed in `pubspec.yaml` file using `flutter pub get`.

```yaml
  - name: Run Tests
    run: flutter test
```
- **Step 5**: Executes tests for the Flutter application using `flutter test`.

```yaml
  - name: Build APK
    run: flutter build apk
```
- **Step 6**: Builds an APK (Android Package) of the Flutter application.

```yaml
  - name: Build App Bundle
    run: flutter build appbundle
```
- **Step 7**: Builds an App Bundle for the Flutter application.

```yaml
  - name: Archive APK and App Bundle
    uses: actions/upload-artifact@v4
    with:
      name: flutter-build-artifacts
      path: |
        build/app/outputs/flutter-apk/app-release.apk
        build/app/outputs/bundle/release/app-release.aab
```
- **Step 8**: Archives the APK (`app-release.apk`) and App Bundle (`app-release.aab`) generated in the previous steps using `actions/upload-artifact` action.

### Explanation

This workflow automates the build, testing, and artifact generation process for a Flutter application on GitHub. It starts by setting up the Flutter environment, fetching dependencies, running tests, and then building both an APK and an App Bundle. Finally, it archives these artifacts (`app-release.apk` and `app-release.aab`) so they can be downloaded or deployed later.

By triggering on pushes to `main` or `development` branches and on pull requests to these branches, this workflow ensures that changes are automatically validated and built for both development and potentially production environments. This setup helps streamline the development and release process for Flutter applications hosted on GitHub.
