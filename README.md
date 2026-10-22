# Bluetooth Thermal Printer Demo (Flutter)

A small Flutter app I wrote in 2022 to test printing receipts to an 80 mm Bluetooth thermal printer from Android. Everything lives in `lib/main.dart`, so it works as a reference for wiring up ESC/POS printing in another app.

## What it does

- Lists the Bluetooth devices already paired with the phone (it doesn't scan for new ones, so pair the printer in system settings first).
- Tap a device to connect to it by MAC address.
- "Print" sends a QR code, a UPC-A barcode and a paper cut, then a few lines of plain text.
- "Print Ticket" (enabled once connected) prints a full sample restaurant receipt: header, server/date rows, line items in a 5-column table, subtotal, tax, a large-font total and a footer note.

Receipts are built with `esc_pos_utils` (`Generator` with `PaperSize.mm80`) and the raw bytes are sent through `bluetooth_thermal_printer`. All receipt content is hard-coded sample data.

## Stack

- Flutter / Dart (SDK constraint `>=2.17.0-69.2.beta <3.0.0`)
- [bluetooth_thermal_printer](https://pub.dev/packages/bluetooth_thermal_printer) 0.0.6 for device listing, connecting and writing bytes
- [esc_pos_utils](https://pub.dev/packages/esc_pos_utils) 1.1.0 for building ESC/POS commands

`pubspec.yaml` also lists `bluetooth_print`, `intl` and `image`, but the current code doesn't use them.

## Running it

This was built against Flutter on a Dart 2.17 beta, with Android Gradle Plugin 4.1, Gradle 6.7, Kotlin 1.6.10 and compileSdk 32 (minSdk 28). I haven't updated it for current Flutter or Dart 3, so expect to bump the SDK constraint and Gradle setup before it builds on a recent toolchain.

```sh
flutter pub get
flutter run
```

Run it on a real Android device with a paired printer. The Android manifest already requests `BLUETOOTH`, `BLUETOOTH_ADMIN`, `ACCESS_FINE_LOCATION` and `ACCESS_BACKGROUND_LOCATION`. Android 12+ also needs the newer `BLUETOOTH_CONNECT`/`BLUETOOTH_SCAN` runtime permissions, which aren't declared here. The iOS project is the default Flutter template and hasn't been set up for Bluetooth.

`test/widget_test.dart` is still the default counter test from `flutter create` and doesn't match this app.
