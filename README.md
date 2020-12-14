# Uber Clone - Flutter

Aplicación móvil estilo **Uber** para **Android** e **iOS**, desarrollada con **Flutter**. Permite a los usuarios registrarse como **cliente** o **conductor**, solicitar viajes en tiempo real, seguir el recorrido en el mapa y calificar la experiencia.

## Características

### Cliente
- Registro e inicio de sesión con correo y contraseña.
- Mapa en tiempo real con la ubicación del usuario y conductores cercanos.
- Búsqueda de origen y destino con autocompletado de Google Places.
- Cálculo automático de la tarifa según distancia y tiempo estimado.
- Solicitud de viaje y seguimiento del conductor en el mapa.
- Información del conductor asignado (nombre, vehículo, placa, calificación).
- Calificación del conductor al finalizar el viaje.
- Historial de viajes con detalle de cada recorrido.
- Edición de perfil con foto.

### Conductor
- Registro con datos del vehículo (placa, marca, modelo).
- Mapa con su posición publicada en tiempo real (Geofire).
- Notificaciones push con las solicitudes de viaje cercanas.
- Aceptar/rechazar viajes con cuenta regresiva.
- Navegación al punto de recogida y al destino con polylines.
- Calificación del cliente al finalizar el viaje.
- Edición de perfil con foto.

## Tecnologías

| Área | Tecnología |
|---|---|
| Framework | Flutter 1.22 / Dart 2.10 |
| Autenticación | Firebase Auth |
| Base de datos | Cloud Firestore |
| Almacenamiento | Firebase Storage |
| Notificaciones | Firebase Cloud Messaging |
| Mapas | Google Maps SDK + Places + Directions |
| Geolocalización | Geolocator + Geoflutterfire (Geofire) |
| UI | Material Design, Custom Clippers, Lottie |

## Estructura del proyecto

```
lib/
├── main.dart                    # Punto de entrada y rutas
└── src/
    ├── api/                     # Configuración de API keys
    ├── models/                  # Client, Driver, TravelInfo, TravelHistory, Prices...
    ├── pages/
    │   ├── home/                # Selección de rol
    │   ├── login/               # Inicio de sesión
    │   ├── client/              # map, register, travel_request, travel_info,
    │   │                        # travel_map, travel_calification, history, edit
    │   └── driver/              # map, register, travel_request, travel_map,
    │                            # travel_calification, edit
    ├── providers/               # auth, client, driver, geofire, google,
    │                            # prices, travel_info, travel_history,
    │                            # storage, push_notifications
    ├── utils/                   # colores, shared_pref, snackbar, otp, progreso
    └── widgets/                 # botones y bottom sheets reutilizables
```

## Requisitos

- Flutter SDK `>=1.22.0 <2.0.0` (Dart `>=2.10.0 <2.11.0`)
- Android Studio / SDK de Android (minSdk 21, targetSdk 29)
- Proyecto de Firebase con Authentication, Firestore, Storage y Cloud Messaging habilitados
- API Key de Google Maps con **Maps SDK**, **Places API** y **Directions API** habilitadas

## Configuración

1. **Firebase**: descarga el archivo `google-services.json` de tu proyecto de Firebase y colócalo en `android/app/`. Para iOS, agrega `GoogleService-Info.plist` en `ios/Runner/`.

2. **Google Maps API Key**: configura tu key en:
   - `lib/src/api/environment.dart` (`API_KEY_MAPS`)
   - `android/app/src/main/AndroidManifest.xml` (`com.google.android.geo.API_KEY`)

3. **Instala dependencias y ejecuta**:

```bash
flutter pub get
flutter run
```

## Ejecutar con Docker

El `Dockerfile` incluye Flutter 1.22.6, el SDK de Android 29 y todo lo necesario para compilar la app. El contenedor se conecta al **emulador de tu máquina (host)** a través de `adb`, por lo que la app corre en tu emulador normal.

### 1. Inicia el emulador en tu máquina

Desde Android Studio (AVD Manager) o por terminal:

```bash
emulator -avd <nombre_de_tu_avd>
```

### 2. Expón el servidor adb del host

El contenedor necesita hablar con el servidor `adb` de tu máquina. Detén el servidor actual y relánzalo escuchando en todas las interfaces:

```bash
adb kill-server
adb -a -P 5037 nodaemon server &
```

### 3. Construye la imagen

```bash
docker build -t uber-clone-flutter .
```

### 4. Ejecuta la app en el emulador

```bash
docker run --rm -it --network host uber-clone-flutter
```

O con docker-compose:

```bash
docker compose up
```

El contenedor espera a que el dispositivo `emulator-5554` esté disponible y luego ejecuta `flutter run -d emulator-5554`. Para cambiar el dispositivo:

```bash
docker run --rm -it --network host -e DEVICE=emulator-5556 uber-clone-flutter
```

> Nota: en Linux se usa `--network host` para que el contenedor alcance el servidor `adb` del host en `127.0.0.1:5037`. Si prefieres compilar solo el APK sin emulador:
>
> ```bash
> docker run --rm -v $(pwd)/build:/app/build uber-clone-flutter flutter build apk --debug
> ```

## Licencia

Proyecto académico con fines educativos.
