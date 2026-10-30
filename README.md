# VORTEX Engine 2.0

Proyecto Android nativo para desarrollar una nueva grabadora de pantalla basada en investigación técnica de código abierto.

## Objetivo

Crear una aplicación original para Android con Kotlin y Jetpack Compose. Captord se utiliza como referencia técnica para estudiar flujos de captura de pantalla, audio, servicio en primer plano, almacenamiento local y galería. No se pretende copiar su marca ni publicar un clon literal.

## Investigación inicial

- Referencia: [Captord](https://github.com/Drenzzz/Captord)
- Licencia de Captord: MIT. Si se reutiliza código, conservar el aviso de copyright y la licencia correspondiente.
- Ingeniería inversa estática: [REA](https://github.com/maidanamauro816-cpu/rea) + JADX headless.
- Informe de arquitectura y plan: [docs/reverse-engineering/captord.md](docs/reverse-engineering/captord.md)

## Alcance de la primera etapa

1. Construir el APK de referencia desde su código fuente público.
2. Analizar el APK con REA/JADX en GitHub Actions.
3. Guardar el informe de análisis como artefacto descargable.
4. Diseñar la arquitectura propia a partir de hallazgos verificados.
5. Implementar, probar y compilar la nueva aplicación nativa.

## Límites importantes

El análisis REA es estático: no ejecuta la app ni verifica su comportamiento real en un teléfono. La validación de MediaProjection, audio interno, notificaciones y guardado MP4 requiere pruebas en un dispositivo Android físico.

## Estado

- [x] Repositorio preparado.
- [x] Flujo automatizado de compilación y análisis estático configurado.
- [ ] Primera ejecución de GitHub Actions y revisión del informe REA.
- [ ] Implementación de la aplicación VORTEX Recorder.
- [ ] Prueba en Android físico y APK de prueba.
