# Magnum-Builds

Repositorio de releases compiladas de **Magnum API**.

- **Página de descarga**: https://diseno-informatico-llc.github.io/Magnum-Builds/
- **Releases**: https://github.com/Diseno-Informatico-LLC/Magnum-Builds/releases
- **Source**: https://github.com/Diseno-Informatico-LLC/Magnum-Api

## Cómo se genera

El workflow corre:
- En cada push a `master`
- A demanda (workflow_dispatch)

Cada build exitoso publica una Release acá con tag `YYYY.MM.DD-buildN` y un ZIP con el output completo.

## Estructura de una Release

Hay **dos lineas de release** publicadas en este repo, distinguidas por el prefijo del tag:

| Linea | Prefijo | Ejemplo de tag | Asset |
|---|---|---|---|
| API / Backend | _(sin prefijo)_ | `2026.04.27-build42` | `MagnumAPI-2026.04.27-build42.zip` |
| Frontend | `frontend-` | `frontend-2026.04.27-build17` | `MagnumFrontend-2026.04.27-build17.zip` |

- **Body**: commit, branch, trigger, target framework
- `index.html` detecta el tipo con `tag.startsWith("frontend-")` para etiquetar visualmente.
- `JobDescargarMagnumBuilds` corre diariamente en modo **incremental**: itera releases desc por `published_at` y, por cada linea (API/frontend), frena al toparse con una carpeta ya existente en disco. Nunca re-baja lo que el usuario haya borrado a proposito. Primera ejecucion de una linea: solo el latest, no toda la historia.

## GitHub Pages

`index.html` es una página estática (vanilla JS) que consume la API pública de Releases de GitHub. No necesita build step ni dependencias.

Para activar Pages:
- Settings → Pages → Source: `Deploy from a branch` → Branch: `master`, folder: `/ (root)` → Save.
