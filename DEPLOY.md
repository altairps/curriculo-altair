# Deploy

O site é publicado pelo GitHub Pages usando GitHub Actions.

## Primeiro setup no GitHub

No repositório `altairps/curriculo-altair`:

1. Abra **Settings**.
2. Vá em **Pages**.
3. Em **Build and deployment**, mude **Source** para **GitHub Actions**.
4. Salve.

Esse passo é feito uma vez. Depois disso, cada `push` na `main` pode publicar o site automaticamente.

## Como publicar

```bash
git push origin main
```

Todo `push` na `main` que toque `altair-cv/**`, `tests/**` ou `.github/workflows/deploy.yml` dispara o workflow:

```text
.github/workflows/deploy.yml
```

O workflow:

1. roda a auditoria automatizada (`html-validate`, navegação por teclado e axe-core);
2. valida as versões PT-BR e EN;
3. se passar, publica a pasta `altair-cv/` no GitHub Pages.

## URL esperada

Antes de configurar domínio próprio:

```text
https://altairps.github.io/curriculo-altair/
```

Versão em inglês:

```text
https://altairps.github.io/curriculo-altair/en/
```

## Domínio próprio

Quando for usar `altair.work`, configure o domínio em **Settings → Pages → Custom domain**.

Depois disso, ajuste se necessário:

- `altair-cv/sitemap.xml`
- `altair-cv/robots.txt`
- metadados `canonical`, `og:url` e `og:image` nos HTMLs

Atualmente eles já apontam para `https://altair.work`.

## Acompanhar deploy

Pelo GitHub:

1. Abra a aba **Actions**.
2. Clique no workflow **Deploy to GitHub Pages**.
3. Aguarde os jobs `Accessibility audit` e `Deploy` ficarem verdes.

Pelo terminal, se o GitHub CLI estiver autenticado:

```bash
gh run list --workflow "Deploy to GitHub Pages" --limit 5
gh run watch
```
