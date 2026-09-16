# IXVOς v2 — Contexto do projeto

## Arquivos
- `/home/claude/ixvoc-site/v2.html` — redesign em desenvolvimento (único arquivo, tudo inline)
- Destino local: `C:\Users\bi_sa\OneDrive\Área de Trabalho\Clientes IXVOC\IXVOC - Site\v2.html`
- GitHub: `gabriel-ixvoc/ixvoc-site` (branch `main`)

## Git
```bash
# Push (proxy workaround obrigatório)
NO_PROXY="github.com" GIT_CONFIG_COUNT=0 git -c http.proxy="" push origin main
# Remote com token (configurado no git config local, não salvar aqui)
```

## Fluxo de entrega
1. Editar `/home/claude/ixvoc-site/v2.html`
2. `git add v2.html && git commit -m "..." && NO_PROXY=... push`
3. `cp v2.html /mnt/user-data/outputs/v2.html`
4. `SendUserFile` → `device_commit_files` para pasta OneDrive

## Foto do Gabriel
- Embutida como base64 diretamente no `<img src="data:image/jpeg;base64,...">` (linha ~1515)
- Arquivo original: `assets/gabriel-sarzi.jpeg` (74924 bytes)

## Estrutura HTML relevante
- `#hero` — hero com `.hero-badge`, `.hero-h1`, `.hero-sub`, `.hero-btns`, `.hero-metrics`
- `#problema` — `.prob-top` (2 cols: eyebrow+h2+sec-lead | quote)
- `#diagnostico` — `.diag-split` (2 cols: mapa | plan-doc sticky)
- `#metodo` — método com trilho `.method-step`
- `#servicos` — `.svc-grid` injetado via JS (SVCS array), modal lateral
- `#cases` — `.cases-grid`
- `#sobre` — `.about-grid > .about-txt` (grid interno: foto esquerda sticky | conteúdo direita)
- `#faq` — `.faq-grid` (faq-head sticky | faq-list)
- `#cta` — CTA final
- `footer` — `.foot-grid`

## CSS — padrões importantes
- Tema escuro: `:root` / tema claro: `.light`
- Reveal on scroll: `.reveal` + `.in` via IntersectionObserver (`observeReveals(root)`)
- Sempre chamar `observeReveals(grid)` APÓS injetar HTML via JS
- Sticky dentro de grid: parent precisa de `align-items:stretch` (default) — não usar `align-items:start`

## Seção Sobre (about) — layout atual
```css
.about-grid { display:block }
.about-txt {
  display:grid;
  grid-template-columns:.72fr 1fr;
  column-gap:3.5rem;
  align-items:start;
}
/* Foto: coluna esquerda, sticky */
.about-txt>.founder-photo { grid-column:1; grid-row:1/99; position:sticky; top:6.5rem }
/* Eyebrow, h2 e resto: coluna direita */
.about-txt>.eyebrow { grid-column:2; grid-row:1 }
.about-txt>.sec-h   { grid-column:2; grid-row:2 }
/* demais filhos: grid-column:2 */
```
Mobile: `.about-txt { display:block }` → foto full-width em fluxo, após h2

## Mobile — breakpoints e alinhamentos
- `@media(max-width:1020px)` — 1 coluna em diag-split, prob-top, faq-grid; nav hamburger
- `@media(max-width:860px)` — nav links somem
- `@media(max-width:680px)` — hero centralizado, foto quadrada, botões full-width, footer centralizado

## Textura
- Grain: `body::after` SVG feTurbulence, `opacity:.055`, `mix-blend-mode:overlay`
- Dot grid: `body` radial-gradient `rgba(255,255,255,.07) 1px`, `28px 28px`

## Cards de serviço (execução)
- `.svc-card` tem `display:flex; flex-direction:column`
- "Ver solução" (`.svc-open`) sempre visível (`opacity:.7`), sobe para `1` no hover

## Regras do Gabriel
- Resumo final curto após cada tarefa
- Sempre salvar este memory.md atualizado
