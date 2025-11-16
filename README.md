# TCAE & Auxiliar de Geriatría – App de estudio

App web y APK ligera para preparar oposiciones (bloques comunes, TCAE SAS y geriatría).

## Funciones
- Resúmenes en Markdown
- Tarjetas flash (JSON)
- Mini‑tests (20 preguntas)
- Simulacros (50 preguntas)
- PWA offline

## Estructura
- `data/` → contenido (summaries, flashcards, tests)
- `src/` → componentes UI y router
- `assets/` → estilos e iconos

## Cómo ejecutar
- Abrir `index.html` localmente o publicar en GitHub Pages.

## Publicar en GitHub Pages
1. Subir todo el repo.
2. En Settings → Pages → Source: `Deploy from a branch`.
3. Branch: `main` / `gh-pages` y carpeta `/root`.
4. Espera el build y abre la URL pública.

## Añadir contenido
- Resúmenes: `data/summaries/*.md`
- Tarjetas: `data/flashcards/*.json`
- Tests: `data/tests/{comunes|tcae_sas|geriatria}/(mini|simulacro).json`

## Créditos
Hecho por Lourdes. Licencia MIT.
