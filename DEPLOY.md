# Deploy

O site é publicado **automaticamente**. Você quase nunca precisa fazer nada manual.

## Como publicar (o caminho normal)

```bash
git push origin main
```

Todo `push` na `main` que toque `bene-cv/**` ou `tests/**` dispara o workflow
[`.github/workflows/deploy.yml`](.github/workflows/deploy.yml), que:

1. **Audita acessibilidade** (`tests/`: html-validate + teclado + axe-core). É um
   _gate_ — se a a11y reprovar, **não publica**.
2. **Sobe por FTP** a pasta `bene-cv/` inteira para `/public_html/` na Locaweb
   (action `SamKirkland/FTP-Deploy-Action`). Como é sync de diretório, qualquer
   arquivo/pasta novo (ex.: `bene-cv/en/`) vai junto sem editar nada.

> O `deploy-locaweb.sh` é só um fallback manual. A fonte de verdade é o workflow.

## Acompanhar um deploy

```bash
gh run watch $(gh run list --branch main --limit 1 --json databaseId -q '.[0].databaseId')
```

Quando o job **FTP Upload to public_html** ficar verde, está no ar:
- PT: https://benearagao.com.br/
- EN: https://benearagao.com.br/en/

## Quando o deploy falha no FTP (`530 Login authentication failed`)

Isso é **credencial**, não código (a auditoria passa, só o upload falha). A senha/usuário
FTP da Locaweb mudou e os secrets do GitHub estão desatualizados. Para corrigir **pelo terminal**:

```bash
# 1. (opcional) testar a credencial sem deixar a senha no histórico — o curl pede a senha oculta:
curl -u benearagao8 ftp://ftp.benearagao.com.br/public_html/     # listou os arquivos = OK

# 2. atualizar o secret (gh mostra "Paste your secret:" — cole a senha, fica oculta):
gh secret set FTP_PASSWORD
gh secret set FTP_USERNAME        # só se o usuário FTP também mudou

# 3. redisparar o deploy sem precisar de novo commit:
gh workflow run "Deploy to Locaweb (FTP)" --ref main
```

> Nunca use `gh secret set FTP_PASSWORD --body "senha"` — isso grava a senha em texto puro no histórico do shell.

Pré-requisito: `gh auth login` feito uma vez, com escopo `repo`.

## Manutenção do workflow

O GitHub deprecia o runtime Node das *actions* antigas (ex.: Node 20 saiu de
suporte em 2025). Quando aparecer um aviso do tipo _"the following actions target
Node.js 20 but are being forced to run on Node.js 24"_ nas anotações do run, suba a
*major* das actions oficiais:

- `actions/checkout` e `actions/setup-node` → **`@v5`** (atual).
- `node-version` do runner deve ficar **>= 22** (o `html-validate` 11 exige).
- `SamKirkland/FTP-Deploy-Action` é de terceiro e tem cadência própria (hoje
  `@v4.3.5`) — não estava no aviso; só atualizar se a própria action avisar.

É só warning, não quebra o deploy — mas manter atualizado evita que vire erro
quando o runtime forçado for removido.
