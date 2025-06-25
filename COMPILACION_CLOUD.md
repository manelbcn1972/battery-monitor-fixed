# Compilación en la Nube - Battery Monitor Android

## GitHub Actions (Recomendado)

### 1. Subir código a GitHub

```bash
# Crear repositorio en GitHub y luego:
git init
git add .
git commit -m "Initial Android Battery Monitor app"
git branch -M main
git remote add origin https://github.com/tu-usuario/battery-monitor.git
git push -u origin main
```

### 2. Compilación automática

Una vez que subas el código a GitHub, automáticamente:

1. Se ejecutará la compilación en GitHub Actions
2. Se generarán dos APKs:
   - `battery-monitor-debug.apk` (para desarrollo)
   - `battery-monitor-release.apk` (para producción)
3. Los APKs estarán disponibles en la pestaña "Actions" del repositorio

### 3. Descargar APKs

1. Ve a tu repositorio en GitHub
2. Clic en "Actions"
3. Selecciona la última compilación exitosa
4. Descarga los artifacts (APKs)

## GitHub Codespaces (Editor en línea)

### 1. Abrir en Codespaces

1. En tu repositorio de GitHub, haz clic en "Code"
2. Selecciona "Codespaces"
3. Clic en "Create codespace on main"

### 2. Compilar en Codespaces

```bash
# Una vez en Codespaces:
cd android-app
chmod +x gradlew
./gradlew assembleDebug
```

### 3. Descargar APK

```bash
# El APK estará en:
app/build/outputs/apk/debug/app-debug.apk
```

Usa el explorador de archivos de Codespaces para descargar el APK.

## Gitpod (Alternativa)

### 1. Abrir en Gitpod

Agrega `https://gitpod.io/#` antes de la URL de tu repositorio:
```
https://gitpod.io/#https://github.com/tu-usuario/battery-monitor
```

### 2. Configurar Gitpod

Crea `.gitpod.yml` en la raíz:

```yaml
image: gitpod/workspace-android

tasks:
  - name: Setup Android
    init: |
      cd android-app
      chmod +x gradlew
      ./gradlew build
```

### 3. Compilar

```bash
cd android-app
./gradlew assembleDebug
```

## Replit (En tu entorno actual)

### 1. Instalar dependencias Android

```bash
# Instalar OpenJDK
sudo apt update
sudo apt install openjdk-17-jdk -y

# Descargar Android SDK Command Line Tools
wget https://dl.google.com/android/repository/commandlinetools-linux-10406996_latest.zip
unzip commandlinetools-linux-10406996_latest.zip
mkdir -p android-sdk/cmdline-tools
mv cmdline-tools android-sdk/cmdline-tools/latest

# Configurar variables de entorno
export ANDROID_HOME=$PWD/android-sdk
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools
```

### 2. Instalar Android SDK

```bash
# Aceptar licencias
yes | sdkmanager --licenses

# Instalar componentes necesarios
sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"
```

### 3. Compilar

```bash
cd android-app
chmod +x gradlew
./gradlew assembleDebug
```

## Servicios de CI/CD alternativos

### CircleCI

Crea `.circleci/config.yml`:

```yaml
version: 2.1

orbs:
  android: circleci/android@2.3.0

jobs:
  build:
    executor:
      name: android/android-machine
      resource-class: large
    steps:
      - checkout
      - android/restore-gradle-cache
      - run:
          name: Build APK
          command: |
            cd android-app
            ./gradlew assembleDebug
      - store_artifacts:
          path: android-app/app/build/outputs/apk/debug/
      - android/save-gradle-cache
```

### Bitrise

1. Conecta tu repositorio a Bitrise.io
2. Selecciona "Android" como tipo de proyecto
3. La configuración se genera automáticamente
4. Los APKs se generan en cada push

## Ventajas de cada método

### GitHub Actions ✅ Recomendado
- **Gratis** para repositorios públicos
- **Fácil configuración** automática
- **Artifacts disponibles** por 90 días
- **Integración completa** con GitHub

### GitHub Codespaces
- **Editor completo** en el navegador
- **Compilación rápida** en servidores potentes
- **Gratis** 60 horas/mes para usuarios básicos

### Gitpod
- **Configuración automática** con Docker
- **IDE completo** en la nube
- **Gratis** 50 horas/mes

### Replit (actual)
- **Ya tienes el entorno** configurado
- **No necesitas** cuenta adicional
- **Limitaciones** de recursos en plan gratuito

## Recomendación

**Para máxima facilidad:** Usa GitHub Actions

1. Sube tu código a GitHub
2. Los APKs se compilan automáticamente
3. Descarga desde la pestaña Actions
4. Instala en tu teléfono Android

## Instalación del APK

Una vez que tengas el APK:

### En Android
1. Activar "Orígenes desconocidos" en Configuración
2. Transferir APK al teléfono (email, cable USB, etc.)
3. Abrir APK y seguir las instrucciones
4. Permitir permisos cuando se soliciten

### Verificar instalación
1. Abrir app "Battery Monitor"
2. Presionar "Register Device"
3. Programar "Weekly Monitoring"
4. Verificar datos en el portal web

## Solución de problemas

### Error de compilación
- Verificar que todas las dependencias estén instaladas
- Revisar logs de compilación en Actions/Codespaces
- Asegurar que Android SDK esté correctamente configurado

### APK no instala
- Verificar que "Orígenes desconocidos" esté habilitado
- Probar APK debug en lugar de release
- Verificar compatibilidad con versión Android (mínimo API 24)

### App no conecta
- Verificar que el servidor esté funcionando
- Comprobar URL del servidor en el código
- Revisar conexión a internet del dispositivo