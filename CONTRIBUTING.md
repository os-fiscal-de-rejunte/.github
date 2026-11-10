# Guia de contribuição — Os Fiscal de Rejunte

Regras válidas para **todos** os repositórios da org. Tudo que dá para validar automaticamente é validado na CI. O que não dá, vale como acordo de time.

---

## 1. Nome de repositório

**Formato:** `<area>-<nome>`, em kebab-case.

| Regra | Certo | Errado |
|---|---|---|
| minúsculas, sem acento, sem espaço | `pipeline-firms` | `Pipeline_FIRMS` |
| palavras separadas por `-` | `web-dashboard` | `webDashboard` |
| começa com uma área da tabela | `model-harmonizer` | `harmonizer-final-v2` |
| sem versão ou data no nome | `docs-pitch` | `pitch-2026-final` |
| no máximo 3 palavras depois da área | `api-fire-calendar` | `api-fire-calendar-service-backend` |

| Área | Uso |
|---|---|
| `pipeline-` | ingestão e tratamento de dados (FIRMS, MODIS, VIIRS) |
| `model-` | ML, harmonização, notebooks de treino |
| `api-` | backend / serviços |
| `web-` | frontend, visualização |
| `docs-` | documentação, pitch, vídeo |
| `infra-` | deploy, IaC, configs compartilhadas |
| `exp-` | spikes e protótipos descartáveis (podem ser apagados) |

**Exceções fixas:** `.github` (padrões da org) e `repo-template` (template).

> **Recomendação para o hackathon:** um **monorepo** (`modis-viirs-harmonizer`) com as pastas `pipeline/`, `model/`, `api/` e `web/`. Em 48h, vários repos só geram atrito. Crie repos separados apenas para `exp-*`.

---

## 2. Nome de branch

**Formato:** `<tipo>/<nº-issue>-<descricao-curta>`

```
feat/12-matching-espaco-temporal
fix/31-fuso-horario-firms
docs/7-readme-submissao
exp/spike-sonificacao          # nº da issue é opcional
```

- `<tipo>` = um dos tipos de commit (seção 3) ou `exp` / `hotfix`.
- Kebab-case, minúsculas, sem acento.
- A branch vive pouco: abra o PR no mesmo dia. Atualize com `git rebase origin/main`.
- `main` é protegida: ninguém faz push direto. Não existe `develop` (trunk-based).

Validado na CI pelo job `branch-name`. Exceções automáticas: `dependabot/*` e `release-please--*`.

---

## 3. Commits — Conventional Commits

**Formato:**

```
<tipo>(<escopo>)!: <descrição>

[corpo opcional — o porquê, não o como]

[rodapé opcional — BREAKING CHANGE: ..., Refs #12]
```

### Tipos

| Tipo | Quando | Efeito na versão |
|---|---|---|
| `feat` | nova funcionalidade | minor |
| `fix` | correção de bug | patch |
| `perf` | melhoria de desempenho | patch |
| `refactor` | muda código sem mudar comportamento | — |
| `docs` | só documentação | — |
| `test` | só testes | — |
| `data` | scripts/configs de dados, schemas, amostras | — |
| `build` | dependências, empacotamento | — |
| `ci` | workflows e pipelines | — |
| `chore` | manutenção geral | — |
| `style` | formatação, sem mudar lógica | — |
| `revert` | reverte um commit | — |

`!` depois do tipo/escopo, ou `BREAKING CHANGE:` no rodapé, gera **major**.

### Escopos

`pipeline` · `model` · `api` · `web` · `viz` · `data` · `infra` · `docs` · `deps` · `release`

O escopo é opcional, mas se usado tem que ser da lista.

### Regras da descrição

- **Em inglês** (o repo é avaliado globalmente pela NASA).
- Imperativo: `add`, `fix`, `remove` (não `added`, `fixes`).
- Começa em minúscula, sem ponto final, com até 72 caracteres no cabeçalho.
- Um commit = uma mudança lógica.

### Exemplos

```
feat(pipeline): add FIRMS archive ingestion for Pantanal AOI
fix(model): handle missing FRP values in MODIS overlap set
perf(api): cache weekly calendar aggregation in DuckDB
docs: add data provenance section to README
data(pipeline): pin AOI bounding box for Cerrado
feat(api)!: change calendar endpoint to ISO week keys

BREAKING CHANGE: /calendar now returns {year, week} instead of date strings
```

### Validação

- **Local:** `pre-commit install --hook-type commit-msg` bloqueia mensagens fora do padrão.
- **CI:** `commitlint` valida todos os commits do PR, e `pr-title` valida o título do PR.

---

## 4. Pull Requests

- **Título = Conventional Commit.** No squash merge, o título vira o commit na `main`, então ele é o que mais importa.
- Corpo: preencha o template e use `Closes #N` para fechar a issue.
- PR pequeno: idealmente menos de 400 linhas alteradas.
- Requisitos para merge: CI verde (`ci`, `pr-title`, `branch-name`, `commitlint`), 1 aprovação e conversas resolvidas.
- Merge **somente por squash**. A branch é apagada automaticamente.
- Rascunho? Abra como **Draft PR**.

---

## 5. Issues

- Título curto no imperativo: `Implement spatiotemporal matching`.
- Use um template (bug / feature / task) e coloque labels `area:*` + `prio:*`.
- Quem pega a issue se atribui (`Assignees`) antes de criar a branch.

---

## 6. Releases e versão

- SemVer (`vMAJOR.MINOR.PATCH`), calculado **automaticamente** pelos commits (release-please).
- O release-please mantém um PR `chore(main): release x.y.z` com o CHANGELOG. Fazer merge dele cria a tag e a GitHub Release.
- A versão entregue à NASA é a tag `v1.0.0`.

---

## 7. Código

- **Python:** `ruff` (lint + format) e `pytest`. Config no `pyproject.toml`.
- **JS/TS:** scripts `lint`, `test` e `build` no `package.json` (a CI roda se existirem).
- Segredos (ex.: `FIRMS_MAP_KEY`) só em `.env` (fora do git) ou em GitHub Secrets. Nunca commite `.env`.
- Dados brutos ficam fora do git (`data/raw/` está no `.gitignore`). Commite só amostras pequenas em `data/sample/`.
