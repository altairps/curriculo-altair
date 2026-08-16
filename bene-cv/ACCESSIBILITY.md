# Acessibilidade

Checklist de acessibilidade do CV web de Altair Santos.

## Páginas

- PT-BR: <https://altair.work/>
- EN: <https://altair.work/en/>

## Implementação

- HTML semântico com `header`, `main`, `aside`, `section`, `article`, `footer` e `nav`.
- Skip link como primeiro elemento focável, apontando para `#main-content`.
- Foco visível padronizado via `:focus-visible`.
- Botões de ação com `aria-label`.
- Links externos com `target="_blank"` e `rel="noopener noreferrer"`.
- Alternância de idioma com `hreflang`, `lang`, `canonical` e `alternate`.
- Conteúdo principal agrupado por headings hierárquicos.
- Skills estruturadas com `<dl>`, evitando barras percentuais sem significado acessível.
- Feedback do botão compartilhar via região `role="status"` com `aria-live="polite"`.
- Suporte a `prefers-reduced-motion`.
- Suporte a `forced-colors: active`.

## Testes automatizados

Rodar o servidor local:

```bash
cd bene-cv
python3 -m http.server 4321
```

Validar PT-BR:

```bash
cd ../tests
BASE_URL=http://localhost:4321/ npm test
```

Validar EN:

```bash
BASE_URL=http://localhost:4321/en/ npm run test:keyboard
BASE_URL=http://localhost:4321/en/ npm run test:a11y
```

## Resultado atual

Validação em 2026-08-16:

- `html-validate`: 0 erros em `index.html` e `en/index.html`.
- Teste de teclado PT-BR: 9/9 checks.
- Teste de teclado EN: 9/9 checks.
- axe-core PT-BR: 0 violações.
- axe-core EN: 0 violações.

O axe retornou um item `color-contrast` como revisão manual/incomplete nas duas rotas. Não houve violação automática de contraste.
