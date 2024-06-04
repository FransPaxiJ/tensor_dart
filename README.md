## Flutter Version

- Flutter 2.10.0 (obbligatory)
- Java 11.0.22 (obbligatory)

## Required Software

- Android Studio : https://developer.android.com/studio
- Flutter : https://docs.flutter.dev/get-started/install/windows/mobile?tab=download
- JDK : https://www.oracle.com/pe/java/technologies/downloads/

## Configuring Software

- In Android Studio, we have installed the Flutter and Dart plugins.
- In Android Studio, go to `File -> Settings -> Languages & Frameworks -> Android SDK/SDK Tools` and install Android SDK Command-line.
- In Android Studio, create a emulator by going to `Tools -> AVD Manager` and create a new virtual device.

## Optional Section

- Activate the developer mode in the mobile device.
- Enable USB debugging in the mobile device.
- Connect the mobile device to the computer using a USB cable.

## Getting Started

0. verify the installation of flutter and dart by running the following commands in the terminal

```bash
flutter doctor
```

1. Clone the repository

2. Connected the device or run the emulator (follow the steps Optional section)

3. Create android, ios, linux, macos, web, windows and test directories in the root of the project

```bash
flutter create .
```

4. Install the required packages

```bash
flutter pub get
```
5. Run the app

```bash
flutter run
```

6. Change build.gradle file in android/app/build.gradle

```bash
android {
    compileSdkVersion flutter.compileSdkVersion

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

    kotlinOptions {
        jvmTarget = '1.8'
    }

    sourceSets {
        main.java.srcDirs += 'src/main/kotlin'
    }

    defaultConfig {
        // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
        applicationId "com.example.flutter_realtime_detection"
        minSdkVersion 21
        targetSdkVersion flutter.targetSdkVersion
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
    }
  }
}
```

## Other Useful Commands

Agree to the licenses

```bash
flutter doctor --android-licenses
```

For creating a new flutter project

```bash
flutter create project_name
```
Ctrl+Shift+P in vscode and select `Flutter: New Project` to create a new flutter project

See device list

```bash
flutter devices
```

## Other commands

Link for add dependencies: https://pub.dev

1. Use log in the app (Recommended)
```dart
import 'package:flutter/foundation.dart';

Future<PerfilUsuario> verPerfil() async {
  final token = await storage.read(key: Constants.TOKEN);
  final decodedToken = JwtDecoder.decode(token!);
  final id = decodedToken['id'];

  final response = await http.get(
    Uri.parse('${Constants.API_HOST}/verperfil?id=$id'),
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer $token',
    },
  );

  if (response.statusCode == 201) {
    final jsonResponse = jsonDecode(response.body);
    final perfilUsuario = PerfilUsuario(
      id: jsonResponse['id'],
      email: jsonResponse['email'],
      password: jsonResponse['password'],
      nombre: jsonResponse['nombre'],
      apellidos: jsonResponse['apellidos'],
      codigo: jsonResponse['codigo'],
      avatar: jsonResponse['avatar'],
      tipo: jsonResponse['tipo'],
    );
    if (kDebugMode) {
      print('Perfil obtenido: $jsonResponse');
    }
    return perfilUsuario;
  } else {
    throw Exception('Error al obtener el perfil: ${response.body}');
  }
}
```

2. Use log in the app
```dart
import 'package:logging/logging.dart'; // Import logging

final _logger = Logger('AuthService'); // Set up logger

Future<bool>login(String email, String password) async {
  final response = await http.post(
    Uri.parse('${Constants.API_HOST}/login'),
    headers: <String, String>{
      'Content-Type': 'application/json; charset=UTF-8',
    },
    body: jsonEncode(<String, String>{
      'email': email,
      'password': password,
    }),
  );

  if (response.statusCode == 201) {
    final jsonResponse = jsonDecode(response.body);
    final token = jsonResponse['token'];
    await storage.write(key: Constants.TOKEN, value: token);
    // Decode the token
    //Map<String, dynamic> decodedToken = JwtDecoder.decode(token);

    Logger.root.level = Level.ALL;
    Logger.root.onRecord.listen((record) {
      print('----------------------------------------');
      print('Nivel: ${record.level.name}');
      print('Tiempo: ${record.time}');
      print('Mensaje: ${record.message}');
      print('----------------------------------------');
    });
    // Log the decoded token
    _logger.info('Decoded token: $token');

    return true;
  } else {
    _logger.info('ERROR PAXI');
    return false;
  }
}
```

3. Add dependencies
```bash
flutter pub add jwt_decoder
```

4. Upgrade dependencies
```bash
flutter pub upgrade
```

5. Add dependencies
```bash
flutter pub add http
```

6. run the app with debug mode
```bash
flutter run --debug
```

