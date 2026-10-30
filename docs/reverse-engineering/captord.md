# Análisis técnico de Captord con REA

## Artefacto de referencia

- Repositorio: https://github.com/Drenzzz/Captord
- Rama consultada: `main`
- Licencia declarada: MIT
- Plataforma: Android nativo, Kotlin y Jetpack Compose
- Identificador de paquete de referencia: `com.captord.screenrecorder`
- SDK mínimo declarado: 29
- SDK objetivo declarado: 36

## Lo que ya se confirmó en el código fuente

El README y el manifiesto de Captord describen:

- Captura de pantalla con Android MediaProjection y consentimiento del sistema.
- Servicio en primer plano para mantener la grabación activa.
- Dos motores: MediaRecorder (estándar) y MediaCodec + MediaMuxer (avanzado).
- Audio de micrófono y, en Android 10/API 29 o superior, audio interno mediante el motor avanzado.
- Salida MP4 local, galería, reproducción, compartir y borrar.
- Notificación persistente, controles flotantes y mosaico de Ajustes rápidos.
- Permisos de micrófono, notificaciones, foreground service, MediaProjection y acceso a videos.

Estos son datos declarados por el proyecto y el código fuente. No constituyen todavía resultados de una ejecución de REA ni una prueba en dispositivo.

## Flujo de análisis automatizado

El workflow `.github/workflows/captord-rea-analysis.yml`:

1. Descarga Captord en un directorio de trabajo separado.
2. Compila el APK debug desde su código fuente.
3. Prepara Node.js compatible con REA, JDK 21 y JADX headless MCP 0.7.1.
4. Verifica SHA-256 del JAR de JADX contra el valor fijado en la documentación de REA.
5. Ejecuta `inspect-android-package` sobre el APK generado.
6. Publica el informe como artefacto de GitHub Actions.

El workflow no modifica ni hace push al repositorio original Captord.

## Preguntas que debe responder el análisis

- ¿Qué Activities, Services, Receivers y Providers declara el APK?
- ¿Qué permisos y tipos de servicio en primer plano requiere?
- ¿Cómo están separadas las capas UI, ViewModel, repositorio, servicio y motores?
- ¿Cómo se inicia y detiene MediaProjection?
- ¿Cómo se conectan MediaCodec, MediaMuxer y la captura de audio?
- ¿Cómo se finaliza el archivo temporal y se publica en la galería?
- ¿Qué limitaciones, dependencias y rutas de error aparecen en el código decompilado?

## Precauciones

REA/JADX produce una representación decompilada; no reconstruye necesariamente el código fuente original ni prueba semántica completa. El análisis es estático y no demuestra que la grabación funcione en tiempo de ejecución. Las pruebas reales deben realizarse en un teléfono.

## Siguiente etapa

Tras obtener el informe, implementar VORTEX Recorder con identidad, paquete, pantallas y arquitectura propios. Reutilizar únicamente componentes cuya licencia y procedencia estén claras, manteniendo los avisos MIT cuando corresponda. No copiar claves, firmas, identificadores privados ni datos de usuario.

## Diagnóstico del primer fallo de CI (2026-10-30)

El primer workflow terminó en el paso `Set up Android SDK`, antes de descargar las plataformas, compilar Captord o iniciar REA. El log registró `Warning: Failed to find package 'tools'` y `sdkmanager ... failed with exit code 1`. La versión anterior de la acción Android solicitaba el paquete obsoleto `tools`, que Google ya no publica.

Corrección aplicada:

- Actualizar la acción `android-actions/setup-android` a una revisión actual que usa command-line tools 22.0 y no solicita el paquete obsoleto.
- Declarar explícitamente `packages: platform-tools`.
- Dejar el workflow únicamente con `workflow_dispatch`: los commits de corrección no lanzan compilaciones automáticamente. La siguiente ejecución debe iniciarse manualmente después de revisar esta corrección.

El fallo original no aporta evidencia sobre errores del código fuente de Captord ni de REA, porque esos pasos nunca comenzaron.
