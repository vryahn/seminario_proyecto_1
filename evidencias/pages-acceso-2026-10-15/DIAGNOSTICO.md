# Diagnóstico de acceso a GitHub Pages · 2026-10-15

## Resultado

La página y sus cinco PDFs están accesibles desde un runner hospedado por GitHub. El workflow manual obtuvo HTTP 200 para los seis archivos, validó los cinco enlaces PDF del índice y comparó bytes y SHA-256 de Pages contra los archivos `raw` del mismo commit. En los PDFs, Pages respondió `application/pdf` y el cuerpo comenzó con `%PDF-`.

- Sitio: <https://vryahn.github.io/seminario_proyecto_1/>
- Repositorio y commit verificado: `vryahn/seminario_proyecto_1@a601654a91f6feca330d918026f712c5747fc6c6`
- Verificación externa: [GitHub Actions run 38804981717](https://github.com/vryahn/seminario_proyecto_1/actions/runs/38804981717) — `completed / success`, disparado con `workflow_dispatch` en `main`.
- Verificador: [`.github/workflows/verify-pages.yml`](../../.github/workflows/verify-pages.yml), sin acciones de terceros, con permisos `contents: read` y límite de cinco minutos.

| Archivo | HTTP Pages | Bytes | SHA-256 | Content-Type |
|---|---:|---:|---|---|
| `index.html` | 200 | 44343 | `808f88120aa09a1c7da6c2a11f2e5c8efcbf450a2ff374ccd66f4ca7fe61fd46` | `text/html` |
| `1A_Bryan_Rodriguez.pdf` | 200 | 88318 | `f01275acea6bf0928a68a4f69b8d144a800d2114c6e47126a850288ed3d68193` | `application/pdf` |
| `entregas/2A_Bryan_Rodriguez.pdf` | 200 | 95503 | `9299fd1648f27d32b1e8948c869b93e431c4dd8247305b61baabd1d0084b91f4` | `application/pdf` |
| `entregas/3A_Bryan_Rodriguez.pdf` | 200 | 132296 | `bdf21b07d4b4608278e4cfec39889f401d16c369ce4e3bbfedf308671c27f0b1` | `application/pdf` |
| `entregas/4A_Bryan_Rodriguez.pdf` | 200 | 204365 | `b094c36eb2419f7f2f9079388f90f295c2a2762699ffef54f91702e12cb1cceb` | `application/pdf` |
| `entregas/5A_Rodriguez_Bryan.pdf` | 200 | 379986 | `622c14b3bdc07a3a5541c8c468e2c3848b614b6659404cad05fffe83e5710b00` | `application/pdf` |

## Acceso desde el Mac

Las pruebas locales previas a la ejecución externa resolvieron `vryahn.github.io` a las cuatro direcciones IPv4 documentadas por GitHub Pages (`185.199.108.153` a `185.199.111.153`), pero el handshake TCP/443 expiró con las cuatro. El intento HTTPS con cada dirección, conservando el hostname/SNI y la validación TLS normal, también expiró. No había proxy HTTP/HTTPS configurado en `scutil --proxy` ni variables de entorno con nombre de proxy; la ruta consultada salía por `en0` a través del gateway `172.20.10.1`. GitHub API y `raw.githubusercontent.com` respondían HTTP 200 desde el mismo Mac. Bryan confirmó que el Mac estaba conectado al hotspot del teléfono.

GitHub documenta también los endpoints IPv6 de Pages (`2606:50c0:8000::153` a `2606:50c0:8003::153`); el socket IPv6 local devolvió `No route to host`, así que no había ruta IPv6 utilizable para contrastar. [Documentación oficial de GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).

## Intentos y conclusión

- Run `38804876036` terminó fallido por una condición demasiado estricta del verificador: para el PDF raíz `raw.githubusercontent.com` respondió `application/octet-stream`, mientras Pages respondió correctamente `application/pdf`. El workflow se ajustó para exigir el tipo MIME PDF únicamente en Pages y mantener la comparación de bytes/hash y firma PDF.
- Run `38804981717` pasó con los seis archivos verificados.
- GitHub Status API informó `All Systems Operational` durante el diagnóstico.

La publicación, el índice, los archivos, DNS observado y la configuración de Pages quedan respaldados por respuestas correctas desde el runner. La falla observada queda acotada a la ruta de red desde el Mac conectado al hotspot hacia las direcciones de Pages; estas pruebas no identifican un filtro, operador o componente de red concreto. Reconectar o probar desde otra red permitiría confirmar si el síntoma cambia con esa ruta.
