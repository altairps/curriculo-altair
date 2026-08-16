# Currículo digital · Altair Santos

CV web estático em **HTML5 + CSS3 puro**, com versões em português e inglês, foco em performance, SEO, impressão em PDF e acessibilidade auditável.

**Produção pretendida:** <https://altair.work>

Este projeto foi adaptado a partir do repositório open-source de Bené Aragão, preservando a proposta original: um currículo web que também funciona como peça de portfólio técnico.

## Estrutura

```text
.
├── bene-cv/
│   ├── index.html          # Versão PT-BR
│   ├── en/index.html       # Versão EN
│   ├── styles.css          # Tokens, layout, responsivo, print e a11y
│   ├── robots.txt
│   ├── sitemap.xml
│   └── assets/
│       ├── favicon.svg
│       ├── og-image.svg
│       └── og-image.png
├── tests/
│   ├── a11y.spec.js
│   ├── keyboard.spec.js
│   ├── regen-og.js
│   └── package.json
├── MEU-CV-DADOS-PTBR.md
└── MY-CV-DATA-EN.md
```

## Rodar localmente

```bash
cd bene-cv
python3 -m http.server 4321
```

Abrir:

- PT-BR: <http://localhost:4321/>
- EN: <http://localhost:4321/en/>

## Validar

```bash
cd tests
npm install
BASE_URL=http://localhost:4321/ npm test
BASE_URL=http://localhost:4321/en/ npm run test:keyboard
BASE_URL=http://localhost:4321/en/ npm run test:a11y
```

O script `test:html` valida as duas páginas. Os testes de teclado e axe usam a rota definida em `BASE_URL`.

## Gerar Open Graph

Depois de editar `bene-cv/assets/og-image.svg`:

```bash
node tests/regen-og.js
```

Isso recria `bene-cv/assets/og-image.png` em 1200x630.

## Conteúdo

O conteúdo do CV foi estruturado a partir de:

- `MEU-CV-DADOS-PTBR.md`
- `MY-CV-DATA-EN.md`

O telefone foi omitido da versão pública conforme briefing.

## Crédito

Base original: currículo digital open-source de Bené Aragão.  
Adaptação: Altair Santos.

## Licença

MIT. Veja `LICENSE`.
