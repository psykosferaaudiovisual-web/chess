# Configuración y build en Windows

Este documento explica los pasos mínimos para ejecutar y compilar la aplicación en Windows (desarrollo web y build Android).

## Requisitos previos
- Node.js (v16+ recomendado) y `npm` o `pnpm`.
- JDK 17 (Temurin / Adoptium) instalado.
- Android Studio (incluye Android SDK) o al menos las Command Line Tools del SDK.
- Git y acceso al repositorio.

## Variables de entorno recomendadas
- `JAVA_HOME` -> carpeta raíz del JDK 17 (por ejemplo `C:\Program Files\Eclipse Adoptium\jdk-17.0.10.7-hotspot`).
- `ANDROID_SDK_ROOT` -> ruta del SDK (por ejemplo `C:\Users\TuUsuario\AppData\Local\Android\Sdk`).

## Plantillas en el proyecto
- `android/local.properties.template`: plantilla que debes copiar a `android/local.properties` y editar `sdk.dir` con tu ruta del SDK.
- `android/keystore.properties.template`: plantilla para la configuración de firma (no subir contraseñas reales al repositorio).

## Pasos rápidos (línea de comandos)
1. Abrir PowerShell o terminal y situarse en la raíz del proyecto:

```powershell
cd C:\ruta\a\chess
```

2. Instalar dependencias y construir los assets web:

```powershell
npm install
npm run build
```

3. Configurar `local.properties` (usar la plantilla):

```powershell
copy android\\local.properties.template android\\local.properties
# luego editar android\\local.properties y poner la ruta correcta en sdk.dir
```

4. Sincronizar con Capacitor (crea/actualiza proyecto Android):

```powershell
npx cap sync android
```

5A. Abrir en Android Studio (recomendado):
- Ejecuta `npx cap open android` y usa Android Studio para Build > Build APK(s).

5B. O compilar por línea de comandos (Windows):

```powershell
cd android
.\\gradlew.bat assembleDebug
# o assembleRelease una vez que configures la firma
```

## Firmar builds (release)
- No subas tu keystore ni contraseñas al repositorio.
- Copia `android/keystore.properties.template` a `android/keystore.properties` y completa los valores.
- Asegúrate de que `app/build.gradle` contiene la configuración de `signingConfigs` (ya se añadió una versión de ejemplo en el proyecto).

## Ejecución local (web)
- Para probar la app en desarrollo en Windows solo necesitas:
```powershell
npm install
npm run dev
# Abrir http://localhost:5173
``` 

## Notas de seguridad
- El keystore es sensible: guarda `keystore.jks` en un lugar seguro y no lo subas al repositorio.
- Reemplaza las contraseñas de ejemplo en `keystore.properties` por valores seguros y, preferiblemente, usa un gestor de secretos.

## Problemas comunes
- Si Gradle indica que no encuentra el SDK, revisa `android/local.properties` (sdk.dir) o la variable `ANDROID_SDK_ROOT`.
- Si hay errores de versión de Java/Gradle/AGP, usa JDK 17 y el wrapper `gradlew.bat` incluido en `android/`.

Si quieres, puedo crear automáticamente `android/local.properties` con tu ruta del SDK y/o generar una `keystore.properties` plantilla con instrucciones.
