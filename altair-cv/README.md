# Altair Santos — Currículo Web

Site estático do CV de Altair Santos, com versões em português e inglês.

- **PT-BR:** `index.html`
- **EN:** `en/index.html`
- **SEO:** Open Graph, Twitter Card, Schema.org `Person`, sitemap e hreflang.
- **Print:** botão de PDF via `window.print()` e estilos em `@media print`.
- **Acessibilidade:** HTML semântico, skip link, landmarks, foco visível, `prefers-reduced-motion` e `forced-colors`.
- **Runtime:** HTML + CSS + JavaScript inline mínimo, sem build e sem framework.

## Rodar

```bash
python3 -m http.server 4321
```

Abrir:

- <http://localhost:4321/>
- <http://localhost:4321/en/>

## Assets

- `assets/favicon.svg` usa as iniciais `AS`.
- `assets/og-image.svg` é a fonte editável do preview social.
- `assets/og-image.png` é gerado por `../tests/regen-og.js`.
- `assets/profile.jpg` é a foto usada no avatar do CV.

## Publicação

Publicar a pasta `altair-cv/` como raiz do site em `altair.work`.

Em serviços como Netlify, Vercel, Cloudflare Pages ou GitHub Pages:

- build command: vazio
- publish directory: `altair-cv`

## Validação

```bash
cd ../tests
npm install
BASE_URL=http://localhost:4321/ npm test
BASE_URL=http://localhost:4321/en/ npm run test:keyboard
BASE_URL=http://localhost:4321/en/ npm run test:a11y
```
