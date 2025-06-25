# Battery Monitor Android App

Esta aplicación Android envía información de la batería de tu teléfono al portal web de monitoreo en tiempo real.

## Características

- **Monitoreo en tiempo real**: Detecta automáticamente cambios en la batería
- **Datos completos**: Nivel, temperatura, voltaje, estado de carga y salud
- **Interfaz moderna**: Diseñada con Jetpack Compose y Material Design 3
- **Envío automático**: Se conecta automáticamente al servidor cuando cambia la batería
- **Registro fácil**: Un botón para registrar el dispositivo en el portal

## Configuración

### 1. Cambiar la URL del servidor

En los archivos `MainActivity.kt` y `BatteryReceiver.kt`, cambia:

```kotlin
private val baseUrl = "https://tu-dominio.replit.app"
```

Por la URL real de tu servidor Replit.

### 2. Compilar la app

```bash
cd android-app
./gradlew assembleDebug
```

### 3. Instalar en el dispositivo

```bash
adb install app/build/outputs/apk/debug/app-debug.apk
```

## Uso

1. **Registrar dispositivo**: Presiona "Register Device" para registrar tu teléfono en el portal
2. **Monitoreo automático**: Presiona "Start Auto Monitoring" para enviar datos automáticamente
3. **Envío manual**: Usa "Send Battery Data" para enviar datos inmediatamente
4. **Ver en el portal**: Los datos aparecerán en el dashboard web

## Permisos requeridos

- `BATTERY_STATS`: Para acceder a información de la batería
- `INTERNET`: Para enviar datos al servidor
- `ACCESS_NETWORK_STATE`: Para verificar conectividad
- `WAKE_LOCK`: Para mantener el monitoreo activo

## Datos enviados

La app envía la siguiente información:

- **Nivel de batería**: Porcentaje actual (0-100%)
- **Temperatura**: En grados Celsius
- **Estado de carga**: Si está conectado al cargador
- **Voltaje**: En voltios
- **Salud de la batería**: Excellent, Good, Fair, Poor
- **ID del dispositivo**: Identificador único del teléfono

## Arquitectura

- **Kotlin**: Lenguaje principal
- **Jetpack Compose**: UI moderna y reactiva
- **OkHttp**: Cliente HTTP para comunicación con el servidor
- **Coroutines**: Programación asíncrona
- **BroadcastReceiver**: Detecta cambios automáticamente

## Desarrollo

### Requisitos

- Android Studio Arctic Fox o superior
- Android SDK 24+ (Android 7.0)
- Kotlin 1.9+

### Estructura del proyecto

```
android-app/
├── app/src/main/
│   ├── java/com/batterymonitor/
│   │   ├── MainActivity.kt          # Actividad principal
│   │   ├── BatteryReceiver.kt       # Receptor de eventos
│   │   └── ui/theme/               # Tema y estilos
│   ├── AndroidManifest.xml         # Configuración y permisos
│   └── res/                        # Recursos (strings, colores, etc.)
└── build.gradle.kts                # Dependencias
```

## Troubleshooting

### Error de conexión

1. Verifica que la URL del servidor sea correcta
2. Asegúrate de tener conexión a internet
3. Revisa que el servidor esté ejecutándose

### Permisos denegados

1. Ve a Configuración > Apps > Battery Monitor > Permisos
2. Habilita todos los permisos necesarios
3. Reinicia la app

### Datos no aparecen en el portal

1. Verifica que el dispositivo esté registrado primero
2. Revisa los logs del servidor para errores
3. Intenta enviar datos manualmente primero

## Próximas características

- [ ] Notificaciones push desde el portal
- [ ] Configuración de frecuencia de envío
- [ ] Modo de ahorro de batería
- [ ] Histórico local de datos
- [ ] Sincronización offline

## Soporte

Para problemas o dudas, revisa los logs en Android Studio o contacta al equipo de desarrollo.