# Modelo de Dados — Controle de Estoque (Caminito · Nazo · Bar)

> Documento-contrato da construção. Define como o estoque é estruturado antes de
> qualquer código. Auditável e versionado. Última revisão a partir das 3 fichas
> técnicas (Almanaque do Bar 2026, Caminito Maio/2026, Nazo 2025).

## 1. Princípio: estoque de 3 níveis

Não existe relação direta "produto vendido → insumo cru". Existe uma cadeia:

```
Nível 2 (Produto vendável)  →  Nível 1 (Pré-preparo/Produção)  →  Nível 0 (Insumo comprado)
   Moscow Mule                    Redução de Gengibre                 Gengibre, Açúcar, Água
   Salada Caminito À la carte     Molho Caminito, Base Caminito       Cenoura, Repolho...
   Combinado 20 peças             Niguiri/Sashimi/Uramaki (peça)      Salmão, Atum, Arroz, Nori
   Yakisoba Especial              Molho Domburi, Legumes yakisoba     Macarrão, Óleo, Camarão
```

A baixa de venda (saída) percorre a cadeia até chegar em item **contável** (ver §3).

## 2. Taxonomia de itens (prefixos usados pela operação)

Mantida exatamente como a equipe já usa nas fichas — não reinventar:

| Prefixo | Significado | Nível típico |
|---|---|---|
| (sem prefixo) | Insumo comprado / matéria-prima | 0 |
| `MP` | Matéria-prima | 0 |
| `HORT` | Hortifruti | 0 |
| `PP` | Pré-preparo (feito na loja) | 1 |
| `PROD` | Produção (feita na loja **ou** recebida pronta do CPD) | 1 |
| `PO` | Proteína porcionada (ex. `PO Ancho`, `PO File mignon`) | 1 |

## 3. Atributos de cada item do catálogo

```jsonc
{
  "id": "reducao_gengibre",
  "nome": "Redução de Gengibre",
  "nivel": 1,                 // 0 insumo | 1 intermediário | 2 produto vendável
  "unidade_base": "ml",       // g | ml | un — unidade em que o estoque é medido
  "item_de_estoque": true,    // CONTÁVEL? Fonte da verdade = planilha de estoque zero
  "origem": "producao",       // compra | producao | cpd
  "estoque_minimo": null,     // vem da planilha de estoque zero
  "setor": null,              // vem da planilha de contagem
  "unidade_compra": null,     // ex. "fardo", "caixa", "kg" — p/ leitura de nota
  "fator_compra": null,       // quantas unidades_base em 1 unidade_compra (ex. 1 fardo = 24 un)
  "bom": []                   // composição (só para nível 1 e 2) — ver §4
}
```

### Regra do "contável" (resolve o caso misto)
- Se o item aparece na **planilha de estoque zero** → `item_de_estoque = true`.
  A venda baixa esse item e ele é reposto por produção/compra.
- Se **não** aparece → `item_de_estoque = false`. A venda explode a receita dele
  até chegar num item contável.

### Regra do CPD
- `origem = "cpd"` → o item entra no estoque **pronto** via nota (debita/credita o
  pacote porcionado, não a matéria-prima). Ex.: hambúrguer de costela, ancho,
  ossobuco, salmão porcionado em pacote.

## 4. BOM (composição / ficha técnica)

Cada item de nível 1 ou 2 tem uma composição. Produtos com versões de porção
(Executivo, À la carte, Kids, Rodízio, Meia porção) são **produtos distintos**,
cada um com seu BOM.

```jsonc
{
  "id": "moscow_mule",
  "nivel": 2,
  "rendimento": { "qtd": 1, "unidade": "porcao" },
  "bom": [
    { "item": "vodka_nacional",     "qtd": 50,  "unidade": "ml" },
    { "item": "reducao_gengibre",   "qtd": 30,  "unidade": "ml" },
    { "item": "sumo_limao",         "qtd": 15,  "unidade": "ml" },
    { "item": "chopp",              "qtd": 15,  "unidade": "ml" },
    { "item": "espuma_gengibre",    "qtd": 10,  "unidade": "ml" }
  ]
}
```

Produções em lote (reduções, arroz, feijão, purê...) têm `rendimento` no total
produzido — assim 1 batelada credita X do intermediário e debita os insumos do BOM.

## 5. Movimento de estoque e CMV

Equação por item, por período:

```
Estoque final = Estoque inicial + Entradas (compras/produção) − Saídas (vendas/perdas)
CMV (R$)      = (Estoque inicial + Compras − Estoque final) valorizado a custo
```

Fontes de cada componente:
- **Estoque inicial / final:** planilha de contagem (3–4x/semana).
- **Entradas:** notas do CPD/fornecedor em PDF (IA lê → conferência → grava).
- **Saídas:** relatório de vendas da semana, explodido pelas fichas (BOM).
- **Mínimo:** planilha de estoque zero.

Sugestão de compra (decisão do projeto):
```
piso        = estoque_minimo − contagem_atual
giro        = consumo_projetado_pela_venda − contagem_atual
sugestao    = max(piso, giro)   // mínimo como piso de segurança, venda ajusta p/ cima
```

## 6. Semáforo por item / setor

- 🟢 Verde: contagem ≥ mínimo.
- 🟡 Amarelo: contagem entre o mínimo e um limiar de alerta (a definir, ex. 1,2× mínimo... ou abaixo do mínimo mas acima de zero — a calibrar).
- 🔴 Vermelho: contagem < mínimo (ou zerado).

## 7. Lacunas conhecidas (a resolver com o Bruno)

1. **Gramatura das peças de sushi** — as fichas do Nazo são de *finalização*
   (montagem), não trazem g de insumo por peça. Solução v1: tabela de
   gramaturas-padrão marcadas como **"a validar"**, editável no app. CMV de sushi
   fica em "modo estimado" até validação.
2. **Itens recebidos do CPD** — confirmar item a item o que chega pronto
   (`origem = cpd`) vs. produzido na loja. Reconciliação com a nota.
3. **Rendimento final das reduções/xaropes** — só é necessário se algum
   intermediário NÃO for contável; nesse caso será levantado caso a caso.
4. **De-para nomes do PDV × fichas** — mapear o nome do produto no relatório de
   vendas para a ficha correspondente (ex. "Executivo 20 peças" → ficha X).

## 8. Fontes (arquivos de origem)

- `Almanaque do Bar 2026` — 39 drinks + produções (reduções/xaropes) + porcionamento + sucos.
- `Caminito FT Maio/2026` — entradas, sobremesas, saladas, parrilla, cozinha (versões Exec/À la carte/Kids).
- `Nazo FT 2025` — combinados de sushi + cozinha quente + pré-preparos + executivo.
