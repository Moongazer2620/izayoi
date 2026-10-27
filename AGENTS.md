# AGENTS.md — Izayoi (fork de SimpMusic)

Cliente de YouTube Music con cuenta propia. Android primero, Desktop (Debian) después. GPL-3.0. Uso personal.
Dueño: Angel. Habla en español, directo y por pasos. Código y commits en inglés.
`CLAUDE.md` es la guía del upstream (referencia de arquitectura). Si choca con este archivo, manda este.

## Reglas de trabajo
- Siempre rama + PR; `git pull` antes de seguir. Nunca `git push --force`. Rama base: `dev` (ramas salen de `dev`, PR a `dev`). Remotos: `origin` = Moongazer2620/izayoi, `upstream` = SimpMusic.
- Sin secretos en el repo (claves de IA, keystore, cookies, tokens). Nunca imprimas cookies ni URLs de googlevideo en logs.
- Cambios de menos de ~30 líneas: directo. Más grandes: espera la ficha de tarea.
- Entrega: commit, rama, PR y resumen de 5 líneas. No declares pruebas que no corriste.
- Strings nuevos solo en `composeResources/values/` (inglés). No edites `values-*` (Crowdin).

## Compilar (HP, 8 GB de RAM)
- Sí se compila aquí. Ignora el "no compilar / JetBrains MCP" de CLAUDE.md.
- Android debug FOSS: `./gradlew --max-workers=2 :androidApp:assembleDebug`
- Instalar: `adb install -r <apk>` (Honor 400 por USB, sin emulador).
- Memoria de Gradle en `~/.gradle/gradle.properties`, nunca en el del repo.
- Build FOSS (sin Sentry/Cast). No uses `--release` sin ficha.

## Mapa mínimo
- UI compartida: `composeApp/src/commonMain/kotlin/com/maxrave/simpmusic/`
- Tema/colores: `ui/theme/{Theme,Color,Typo}.kt`, `expect/ui/PlatformColorScheme*.kt`
- Reproductor: `ui/screen/player/NowPlayingScreen.kt` + `content/NowPlayingContent*.kt`
- Letras: `ui/component/LyricsView.kt`, `AppleMusicLyricsLines.kt`, `LyricText.kt`
- Capas: UI → domain → data → service → common. Código en `commonMain` debe servir en Android y Desktop.

## No tocar sin ficha
- `core/` es un **submódulo** de upstream: no podemos subir cambios ahí. Si una tarea lo necesita, para y avisa.
- `vivoAndroidApp/`, `crashlytics*/`, `cast*/`, `lastfm*/`, `fastlane/`, `changelogs/`.
- Versiones de Compose, haze, skiko, Media3 (están fijadas por motivos de compatibilidad).
- La comprobación de copia no oficial (`Config.OFFICIAL_PACKAGE_NAMES` y firma): el debug la salta; un release firmado por nosotros se bloquearía.
- Iconos: son `ImageVector` (`SimpIcons.X`, un import por icono); sigue "Add a New Icon" de CLAUDE.md.
- Nunca `Color.Black` fijo como fondo: usa `colorScheme.background`.
