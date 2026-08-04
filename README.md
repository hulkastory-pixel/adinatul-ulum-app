name: Android Release Build

on:
  push:
    branches: [ "main", "master" ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v4

    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        java-version: '17'
        distribution: 'temurin'

    - name: Setup Gradle
      uses: gradle/gradle-build-action@v3

    - name: Make Gradlew Executable
      run: chmod +x ./gradlew

    - name: Build Release APK and AAB
      run: |
        ./gradlew clean assembleRelease
        ./gradlew bundleRelease

    - name: Upload APK Artifact
      uses: actions/upload-artifact@v4
      with:
        name: release-apk
        path: app/build/outputs/apk/release/*.apk

    - name: Upload AAB Artifact
      uses: actions/upload-artifact@v4
      with:
        name: release-aab
        path: app/build/outputs/bundle/release/*.aab
