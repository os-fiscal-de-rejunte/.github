<!--
TÍTULO DO PR = Conventional Commit (vira o commit da main no squash):
  feat(pipeline): add FIRMS archive ingestion
  fix(model): handle missing FRP values
Tipos: feat fix perf refactor docs test data build ci chore style revert
Escopos: pipeline model api web viz data infra docs deps release
-->

## O que muda

<!-- 1–3 frases. O quê e por quê. -->

Closes #

## Tipo

- [ ] `feat` — nova funcionalidade
- [ ] `fix` — correção
- [ ] `perf` / `refactor`
- [ ] `data` — dados / schema
- [ ] `docs` / `chore` / `ci`
- [ ] **Breaking change** (título com `!`)

## Como testar

```bash
# comandos para reproduzir / validar
```

## Evidência

<!-- Print, gráfico, métrica antes/depois, link do deploy de preview. Para mudanças no modelo: métrica no conjunto de validação. -->

## Checklist

- [ ] Título do PR segue Conventional Commits
- [ ] Branch segue `<tipo>/<nº-issue>-<descricao>`
- [ ] Testes passam localmente (`pytest` / `npm test`)
- [ ] Sem segredos, `.env` ou dados brutos no diff
- [ ] Documentação/README atualizados, se necessário
- [ ] Dados novos têm fonte e licença citadas (FIRMS, etc.)
