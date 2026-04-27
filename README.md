# Magnum-Builds

Repositorio de releases compiladas y obfuscadas de **Magnum API**.

- **Página de descarga**: https://diseno-informatico-llc.github.io/Magnum-Builds/
- **Releases**: https://github.com/Diseno-Informatico-LLC/Magnum-Builds/releases
- **Source**: https://github.com/Diseno-Informatico-LLC/Magnum-Api

## Cómo se genera

El workflow `build-dotnet-reactor.yml` en `Magnum-Api` corre:
- En cada push a `master`
- Los lunes 09:00 AR (cron semanal)
- A demanda (workflow_dispatch)

Cada build exitoso publica una Release acá con tag `YYYY.MM.DD-buildN` y un ZIP con el output completo (ya con `MagnumAPI.dll` ofuscado por .NET Reactor).

## Estructura de una Release

- **Tag**: `2026.04.27-build42` (fecha + run number)
- **Asset**: `MagnumAPI-2026.04.27-build42.zip` con todo `bin/Release/net8.0-windows/`
- **Body**: commit, branch, trigger, target framework

## GitHub Pages

`index.html` es una página estática (vanilla JS) que consume la API pública de Releases de GitHub. No necesita build step ni dependencias.

Para activar Pages:
- Settings → Pages → Source: `Deploy from a branch` → Branch: `master`, folder: `/ (root)` → Save.
