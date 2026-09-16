# IXVOς v2 — Contexto do projeto

## Arquivos
- `/home/claude/ixvoc-site/v2.html` — redesign em desenvolvimento (único arquivo, tudo inline)
- `index.html` — cópia do v2.html, é o que sobe pro site oficial (sempre manter em sincronia)
- Destino local: `C:\Users\bi_sa\OneDrive\Área de Trabalho\Clientes IXVOC\IXVOC - Site\v2.html`
- GitHub: `gabriel-ixvoc/ixvoc-site` (branch `main`)

## Git
```bash
# Push (proxy workaround obrigatório)
NO_PROXY="github.com" GIT_CONFIG_COUNT=0 git -c http.proxy="" push origin main
# Sempre copiar v2.html → index.html antes do commit
cp v2.html index.html && git add v2.html index.html
```

## Fluxo de entrega
1. Editar `/home/claude/ixvoc-site/v2.html`
2. `cp v2.html index.html && git add v2.html index.html && git commit -m "..." && NO_PROXY=... push`
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

## Nav pill (liquid glass)
- Estado inicial (não rolado):
```css
background:linear-gradient(135deg,rgba(255,255,255,.04),rgba(255,255,255,.005) 40%,rgba(6,8,15,.08));
backdrop-filter:blur(52px) saturate(220%) brightness(1.06);
border:none;
outline:1px solid rgba(255,255,255,.16); /* outline evita borda preta no border-radius */
box-shadow:0 18px 50px rgba(0,0,0,.14),
           inset 0 1px 0 rgba(255,255,255,.22),
           inset 0 -1px 0 rgba(255,255,255,.04),
           inset 1px 0 0 rgba(255,255,255,.07);
```
- Estado `.scrolled` (rolado — mantém legibilidade sobre seções claras):
```css
background:linear-gradient(135deg,rgba(4,8,28,.72),rgba(6,8,15,.82));
outline-color:rgba(255,255,255,.14);
box-shadow:0 16px 46px rgba(0,0,0,.22),inset 0 1px 0 rgba(255,255,255,.18);
```
- **Regra crítica**: usar `outline` em vez de `border` nas pílulas glass — `border` com `border-radius` + `backdrop-filter` gera faixa preta nas bordas pelo anti-aliasing

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
.about-txt>.founder-photo { grid-column:1; grid-row:1/99; position:sticky; top:6.5rem; align-self:start }
/* Eyebrow, h2 e resto: coluna direita */
.about-txt>.eyebrow { grid-column:2; grid-row:1 }
.about-txt>.sec-h   { grid-column:2; grid-row:2 }
/* demais filhos: grid-column:2 */
```
Mobile: `.about-txt { display:block }` → foto full-width em fluxo, após h2

## Mobile — breakpoints e alinhamentos
- `@media(max-width:1020px)` — 1 coluna em diag-split, prob-top, faq-grid; nav hamburger
- `@media(max-width:860px)` — nav links somem; pílula mobile aparece
- `@media(max-width:680px)` — hero centralizado, foto quadrada, botões full-width, footer centralizado

## Pílula mobile (bottom fixed)
```css
.mob-pill{display:none}
@media(max-width:860px){
  body{padding-bottom:calc(4.8rem + env(safe-area-inset-bottom))}
  .mob-pill{
    display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:.55rem;
    position:fixed;left:.75rem;right:.75rem;
    bottom:calc(.65rem + env(safe-area-inset-bottom));z-index:480;
    padding:.55rem;
    background:linear-gradient(145deg,rgba(255,255,255,.12),rgba(10,13,24,.88) 38%,rgba(6,8,15,.95));
    border:none;outline:1px solid rgba(255,255,255,.18);border-radius:22px;
    backdrop-filter:blur(28px) saturate(190%);
    box-shadow:0 18px 50px rgba(0,0,0,.5),inset 0 1px 0 rgba(255,255,255,.18);
  }
  .mob-wpp{background:linear-gradient(135deg,#168a48,#25b965)}
  .mob-diag{background:linear-gradient(135deg,rgba(0,180,255,.22),rgba(0,120,255,.35))}
}
```
- WhatsApp: `https://wa.me/5511993223865`
- Diagnóstico: `#cta`
- Some quando nav menu está aberto (JS MutationObserver em `nav.classList`)

## Cards de serviço (execução)
- `.svc-card` tem `display:flex; flex-direction:column`
- "Ver solução" (`.svc-open`) sempre visível (`opacity:.7`), sobe para `1` no hover

## Textura
- Grain: `body::after` SVG feTurbulence, `opacity:.055`, `mix-blend-mode:overlay`
- Dot grid: `body` radial-gradient `rgba(255,255,255,.07) 1px`, `28px 28px`

## Regras do Gabriel
- Resumo final curto após cada tarefa
- Sempre salvar este memory.md atualizado
- Sempre manter `index.html` em sincronia com `v2.html`
