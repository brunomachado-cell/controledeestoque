# Controle de Estoque — Caminito · Nazo · Bar

Controle de estoque e CMV das operações (Caminito Parrilla e Nazo Sushi Bar),
com dashboard e leitura assistida por IA de notas e planilhas.

## Objetivo

Ter, num único painel, controle completo do estoque: volume por item, semáforo
por setor (🟢/🟡/🔴), itens abaixo do mínimo, sugestão de compra e o ciclo
`estoque inicial + compras − consumo = estoque final` para CMV com previsibilidade.

## Como o app opera (ciclo)

1. **Cadastro** — itens e mínimos vêm da planilha de *estoque zero*.
2. **Fichas técnicas** — base de consumo (BOM) que explode a venda até o insumo.
3. **Contagem** — upload da planilha do checklist (posição atual do estoque).
4. **Vendas** — upload do relatório; consumo é calculado pela ficha.
5. **Notas (PDF)** — IA lê a nota → **você confere** → entra no estoque.
6. **Dashboard** — leitura de tudo, gráficos e sugestão de compra.

> Regra de ouro: entradas por nota e leituras de planilha passam **sempre** por
> uma tela de conferência antes de gravar. Nenhum lançamento cego.

## Estrutura do repositório

- `docs/` — especificação. Começe por [`docs/MODELO_DADOS.md`](docs/MODELO_DADOS.md).
- `data/` — base de dados versionada (catálogo de itens e fichas técnicas em JSON).
- `app/` — código do artefato (dashboard + uploads + IA).

## Status

Em construção — fase de modelagem e extração das fichas técnicas.
Veja o modelo de dados e as lacunas em `docs/MODELO_DADOS.md §7`.
