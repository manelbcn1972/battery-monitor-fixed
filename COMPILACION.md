# Guía de Compilación - Battery Monitor Android

## Requisitos Previos

### 1. Instalar Android Studio
- Descargar desde: https://developer.android.com/studio
- Instalar con las configuraciones por defecto
- Asegurarse de instalar el Android SDK

### 2. Configurar el proyecto

1. **Clonar/Descargar el código**
   ```bash
   # Si tienes el código en un repositorio
   git clone <tu-repositorio>
   cd android-app
   ```

2. **Abrir en Android Studio**
   - File → Open → Seleccionar carpeta `android-app`
   - Esperar a que se sincronicen las dependencias

### 3. Configurar la URL del servidor

Cambiar en estos archivos la URL por la de tu servidor Replit:

**MainActivity.kt (línea ~21):**
```kotlin
private val baseUrl = "https://390c9ac5-bcc6-4fb8-852d-c21d164c4a01-00-1rscpmggn2u4h.spock.replit.dev"
```

**BatteryWorker.kt (línea ~21):**
```kotlin
private val baseUrl = "https://390c9ac5-bcc6-4fb8-852d-c21d164c4a01-00-1rscpmggn2u4h.spock.replit.dev"
```

## Compilación

### Opción 1: Desde Android Studio (Recomendado)

1. **Conectar dispositivo Android**
   - Habilitar "Opciones de desarrollador" en el teléfono
   - Activar "Depuración USB"
   - Conectar por USB

2. **Compilar e instalar**
   - Hacer clic en el botón "Run" (▶️) en Android Studio
   - Seleccionar tu dispositivo
   - Esperar que compile e instale automáticamente

### Opción 2: Línea de comandos

1. **Compilar APK**
   ```bash
   cd android-app
   ./gradlew assembleDebug
   ```

2. **Encontrar el APK**
   ```bash
   # El APK estará en:
   app/build/outputs/apk/debug/app-debug.apk
   ```

3. **Instalar en dispositivo**
   ```bash
   # Con adb (Android Debug Bridge)
   adb install app/build/outputs/apk/debug/app-debug.apk
   ```

## Solución de Problemas

### Error de sincronización de Gradle
```bash
# Limpiar proyecto
./gradlew clean

# Sincronizar de nuevo
./gradlew build
```

### Error de SDK
1. En Android Studio: File → Project Structure
2. Verificar que SDK Level sea 24 o superior
3. Instalar SDK faltantes si es necesario

### Error de permisos en dispositivo
1. Configuración → Apps → Battery Monitor → Permisos
2. Habilitar todos los permisos
3. Configuración → Batería → Optimización → Excluir Battery Monitor

### Error de conexión de red
1. Verificar que la URL del servidor sea correcta
2. Probar la URL en un navegador web
3. Asegurarse de que el servidor esté funcionando

## Pruebas

### 1. Registrar dispositivo
- Abrir la app
- Presionar "Register Device"
- Debe mostrar mensaje de éxito

### 2. Enviar datos manualmente
- Presionar "Send Battery Data"
- Verificar en el portal web que aparezcan los datos

### 3. Programar monitoreo semanal
- Presionar "Schedule Weekly Monitoring"
- Los datos se enviarán automáticamente cada semana

## Versión de Producción (APK firmado)

Para crear una versión de producción:

1. **Generar keystore**
   ```bash
   keytool -genkey -v -keystore battery-monitor.keystore -alias battery-monitor -keyalg RSA -keysize 2048 -validity 10000
   ```

2. **Configurar signing en build.gradle**
   ```kotlin
   android {
       signingConfigs {
           release {
               keyAlias 'battery-monitor'
               keyPassword 'tu-password'
               storeFile file('battery-monitor.keystore')
               storePassword 'tu-password'
           }
       }
       buildTypes {
           release {
               signingConfig signingConfigs.release
               // resto de configuración
           }
       }
   }
   ```

3. **Compilar versión de release**
   ```bash
   ./gradlew assembleRelease
   ```

## Estructura del Proyecto

```
android-app/
├── app/
│   ├── src/main/
│   │   ├── java/com/batterymonitor/
│   │   │   ├── MainActivity.kt          # Actividad principal
│   │   │   ├── BatteryWorker.kt         # Trabajo semanal programado
│   │   │   └── ui/theme/               # Tema de la app
│   │   ├── AndroidManifest.xml         # Configuración y permisos
│   │   └── res/                        # Recursos
│   └── build.gradle.kts                # Configuración del módulo
├── build.gradle.kts                    # Configuración del proyecto
├── settings.gradle.kts                 # Configuración de Gradle
└── gradle/libs.versions.toml           # Versiones de dependencias
```

## Características Implementadas

- ✅ Monitoreo semanal automático (no tiempo real)
- ✅ Registro de dispositivo con un botón
- ✅ Envío manual de datos
- ✅ Interfaz moderna con Jetpack Compose
- ✅ WorkManager para tareas programadas
- ✅ Datos completos de batería (nivel, temperatura, voltaje, salud)
- ✅ Manejo de permisos automático

## Próximos Pasos

Después de instalar la app:

1. Registrar el dispositivo en el portal
2. Programar el monitoreo semanal
3. Verificar que los datos aparezcan en el dashboard web
4. La app enviará datos automáticamente cada semana

## Soporte

Si tienes problemas durante la compilación:

1. Revisar los logs de Android Studio
2. Verificar que todos los SDKs estén instalados
3. Asegurarse de que la URL del servidor sea correcta
4. Probar la conexión manualmente primero