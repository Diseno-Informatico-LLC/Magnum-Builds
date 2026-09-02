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

Hay **tres lineas de release** publicadas en este repo, distinguidas por el prefijo del tag:

| Linea | Prefijo | Ejemplo de tag | Asset |
|---|---|---|---|
| API / Backend | _(sin prefijo)_ | `2026.04.27-build42` | `MagnumAPI-2026.04.27-build42.zip` |
| Frontend | `frontend-` | `frontend-2026.04.27-build17` | `MagnumFrontend-2026.04.27-build17.zip` |
| Desktop (.NET/WPF) | `desktop-` | `desktop-2026.08.05-build7` | `MagnumDesktop-2026.08.05-build7.zip` |

- **Body**: commit, branch, trigger, target framework
- `index.html` detecta el tipo por prefijo del tag (`frontend-`, `updater-`, `desktop-`; sin prefijo = API) para etiquetar visualmente y mostrar el latest de cada linea.
- La pagina **pagina la API de GitHub** (`per_page=100` en bucle hasta agotar). Antes pedia una sola pagina y el historial quedaba cortado en la release numero 100, que es lo que escondia los frontends y desktops viejos.
- Vista **Puntos de actualizacion** (la que abre por defecto): una fila por dia con publicaciones, mostrando que version de API / Frontend / Desktop quedaba **vigente al cierre de ese dia**. Sirve para dejar un cliente en un punto concreto sin tener que aparear tags a mano: los recuadros verdes se publicaron ese dia, los grises son los que seguian vigentes de antes. La busqueda normaliza separadores, asi que `12/06`, `12.06` y `20260612` matchean lo mismo, e indexa tanto lo vigente como lo publicado ese dia.
- Vista **Todas las releases**: el listado plano de siempre, con filtro por linea y busqueda por fecha o tag.
- `JobDescargarMagnumBuilds` corre diariamente en modo **incremental**: itera releases desc por `published_at` y, por cada linea (API/frontend), frena al toparse con una carpeta ya existente en disco. Nunca re-baja lo que el usuario haya borrado a proposito. Primera ejecucion de una linea: solo el latest, no toda la historia. La linea `desktop-` **no** la descarga (el desktop no esta en produccion); se distribuye solo desde la pagina.

## GitHub Pages

`index.html` es una página estática (vanilla JS) que consume la API pública de Releases de GitHub. No necesita build step ni dependencias.

Para activar Pages:
- Settings → Pages → Source: `Deploy from a branch` → Branch: `master`, folder: `/ (root)` → Save.
