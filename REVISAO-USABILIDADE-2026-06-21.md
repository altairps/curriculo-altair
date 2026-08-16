# Revisão de Usabilidade — 10 Heurísticas de Nielsen

**Produto:** Currículo digital Bené Aragão — <https://benearagao.com.br> (PT + `/en/`)
**Data:** 2026-06-21
**Referência:** *10 Usability Heuristics for User Interface Design* (Jakob Nielsen, NN/g)
**Escopo:** `altair-cv/index.html`, `altair-cv/en/index.html`, `styles.css` e o JS de compartilhar
**Resultado:** ✅ Conforme — 1 achado relevante corrigido (affordance de ícone no mobile), demais heurísticas sem ação obrigatória

> Complementa o [Relatório de Acessibilidade](AUDITORIA-A11Y-2026-06-01.md) (WCAG 2.2 AA). Usabilidade e acessibilidade se sobrepõem, mas não são a mesma coisa: aqui o foco é a clareza percebida pelo usuário, não a conformidade técnica.

---

## Placar por heurística

| # | Heurística | Status | Observação |
|---|---|---|---|
| 1 | Visibilidade do status do sistema | ✅ | Feedback `aria-live` no compartilhar ("Link copiado!"); diálogos nativos de impressão/share. |
| 2 | Correspondência com o mundo real | ⚠️→✅ | Ícones reconhecíveis (globo/impressora/seta). Rótulos visíveis adicionados no mobile (era o ponto cego). |
| 3 | Controle e liberdade do usuário | ✅ | Share nativo cancelável; troca de idioma reversível; links externos sinalizam "nova aba"; skip link; sem armadilha de teclado. |
| 4 | Consistência e padrões | ✅ | Botão = ação (`<button>`), link = navegação (`<a href>`); ícones com mesmo traço; links externos sempre com `rel="noopener"` + aviso. |
| 5 | Prevenção de erros | ✅ | Página estática (poucas superfícies de erro); cadeia de fallback no share; `onerror` remove imagem quebrada. |
| 6 | Reconhecimento em vez de memorização | ⚠️→✅ | **Achado principal:** ícones sem rótulo no mobile exigiam adivinhação. Corrigido. |
| 7 | Flexibilidade e eficiência | ✅ | Skip link, `tel:`/`mailto:`, atalho de impressão, Web Share API quando disponível. |
| 8 | Design estético e minimalista | ✅ | Layout limpo, hierarquia tipográfica clara, sem ruído. |
| 9 | Ajuda a reconhecer/recuperar de erros | ✅ | Fallback do share orienta: "Copie o link na barra de endereços". |
| 10 | Ajuda e documentação | ✅ (N/A) | Currículo autoexplicativo; `aria-label`/`title` cobrem affordances. Sem necessidade de manual. |

---

## Achado #1 (relevante) — Heurística 6: ícone sem rótulo no mobile

**Sintoma:** na barra de ações fixa do rodapé (mobile), os três botões apareciam **só com ícone**. Globo, impressora e seta forçavam o usuário a *adivinhar* a função — exatamente o que a heurística 6 pede para evitar (reconhecimento > memorização).

**Por que `title`/hover não bastava:** `title` só dispara no **hover**, que **não existe em touch**. Resolveria o desktop e deixaria o mobile — onde o problema foi notado — sem solução.

**Correção aplicada:**
- **Mobile:** ícone **+ rótulo curto embaixo** (padrão *tab bar* de app). Os 3 botões dividem a largura da barra igualmente (`flex: 1`). Resolve no touch.
- **Desktop:** mantém ícone + texto e ganha `title` (tooltip no hover) nos 3 botões.
- **Leitor de tela:** `aria-label` já existia e continua sendo a fonte autoritativa do nome acessível (o `title` é ignorado pela AT quando há `aria-label`, então não há anúncio duplicado).

**Regra que fica para o futuro:** botão só-ícone precisa de **rótulo visível OU contexto inequívoco** — `title`/tooltip é reforço para apontadores, **nunca** a única fonte de significado, porque exclui touch e leitores de tela. Registrado na skill `usabilidade`.

---

## Notas menores (sem ação obrigatória)

- **H2 — ícone de impressora para "Baixar PDF":** o rótulo diz "Baixar/Download" mas o ícone é de impressora e a ação abre o diálogo de impressão do navegador (que oferece "Salvar como PDF"). É honesto e é um padrão comum na web; o `title` reforça. Mantido por ora — só trocar para um ícone de download se quiser correspondência 1:1.
- **H1 — sem indicador de "carregando":** desnecessário numa página estática; o navegador já dá feedback de navegação. Sem ação.

---

## Como revalidar

A usabilidade não tem scanner automático equivalente ao axe, mas estes checks dão cobertura prática:

```bash
cd altair-cv && python3 -m http.server 4321 &
cd tests && npm test            # html-validate + teclado + axe (não quebrar a base)
```

Inspeção manual recomendada a cada mudança na barra de ações:
1. **Mobile real (touch):** todo botão de ação tem rótulo visível? (heurística 6)
2. **Desktop:** `title` aparece no hover?
3. **Leitor de tela:** o nome anunciado faz sentido e não duplica?
