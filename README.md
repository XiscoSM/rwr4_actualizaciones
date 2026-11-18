# rwr4_actualizaciones

Actualizaciones de la app **RWR4** (PDA Android) para las empresas que la instalan a mano, sin Intune.

La app comprueba al abrirse si hay una versión nueva en este repositorio, pregunta al operario y, si
acepta, descarga el APK, comprueba su huella SHA-256 y abre el instalador de Android.

Las PDA instaladas por Google Play o gestionadas por Intune no consultan este repositorio: allí las
actualizaciones llegan por Intune.

## Estructura

```
estable/            RWR4-<versión>-Signed.apk       + version.json   (la que se instala)
estable/anterior/   la versión publicada antes                       (por si hay que volver atrás)
beta/               RWR4Beta-<versión>-Signed.apk   + version.json
beta/anterior/
```

El repositorio **no guarda historial**: cada publicación lo rehace con un único commit (push
forzado). Solo existen la versión actual y la anterior de cada canal.

`version.json`:

```json
{"version":"1.10.0","fichero":"RWR4-1.10.0-Signed.apk","sha256":"..."}
```

La app lee `https://raw.githubusercontent.com/XiscoSM/rwr4_actualizaciones/main/<canal>/version.json`.

## Publicar una versión

No se edita a mano. Desde el repositorio de la app:

```
powershell -ExecutionPolicy Bypass -File tools\publicar-actualizacion.ps1 -Apk <ruta del -Signed.apk>
```

El script lee la versión y el canal del propio APK, sustituye el APK anterior del canal, genera
`version.json`, y hace commit y push. GitHub tarda unos minutos en servir la versión nueva.

Android solo instala la actualización si va firmada con la misma clave que la app instalada.
