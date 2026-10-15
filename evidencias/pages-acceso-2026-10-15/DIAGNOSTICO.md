# Verificación de GitHub Pages · 2026-10-15

El índice y sus cinco PDFs respondieron HTTP 200 desde un runner hospedado por GitHub. El workflow validó los cinco enlaces del índice y comparó el contenido publicado con los archivos `raw` del mismo commit. GitHub Pages respondió `application/pdf` para los PDFs y cada archivo comienza con `%PDF-`.

- Sitio: <https://vryahn.github.io/seminario_proyecto_1/>
- Repositorio y commit verificado: `vryahn/seminario_proyecto_1@a601654a91f6feca330d918026f712c5747fc6c6`
- Verificación externa: [GitHub Actions run 38804981717](https://github.com/vryahn/seminario_proyecto_1/actions/runs/38804981717) — `completed / success`, ejecución manual en `main`.
- Verificador: [`.github/workflows/verify-pages.yml`](../../.github/workflows/verify-pages.yml), con permisos de solo lectura y límite de cinco minutos.

| Archivo | HTTP | Bytes | SHA-256 | Content-Type |
|---|---:|---:|---|---|
| `index.html` | 200 | 44343 | `808f88120aa09a1c7da6c2a11f2e5c8efcbf450a2ff374ccd66f4ca7fe61fd46` | `text/html` |
| `1A_Bryan_Rodriguez.pdf` | 200 | 88318 | `f01275acea6bf0928a68a4f69b8d144a800d2114c6e47126a850288ed3d68193` | `application/pdf` |
| `entregas/2A_Bryan_Rodriguez.pdf` | 200 | 95503 | `9299fd1648f27d32b1e8948c869b93e431c4dd8247305b61baabd1d0084b91f4` | `application/pdf` |
| `entregas/3A_Bryan_Rodriguez.pdf` | 200 | 132296 | `bdf21b07d4b4608278e4cfec39889f401d16c369ce4e3bbfedf308671c27f0b1` | `application/pdf` |
| `entregas/4A_Bryan_Rodriguez.pdf` | 200 | 204365 | `b094c36eb2419f7f2f9079388f90f295c2a2762699ffef54f91702e12cb1cceb` | `application/pdf` |
| `entregas/5A_Rodriguez_Bryan.pdf` | 200 | 379986 | `622c14b3bdc07a3a5541c8c468e2c3848b614b6659404cad05fffe83e5710b00` | `application/pdf` |

La verificación confirma el estado de la publicación desde ese runner en el momento de la ejecución. No diagnostica la ruta de red de otros dispositivos o conexiones.
