# De-para PDV × ficha técnica — Bar

Fonte: índice do Almanaque do Bar (nomes canônicos com as variantes Caminito/Nazo)
contra `Venda Unid` de `CP_SG_VENDAS.xlsx` e `NZ_SG_VENDAS.xlsx` (agosto/2026, 31 dias).
Saída: `data/depara_pdv_bar.json`.

## Cobertura

**35 das 39 fichas** casaram com produto do PDV, somando **1.568 unidades no mês**.

Casos que exigiram inspeção manual (o casamento por texto errava):

| Ficha | Produto no PDV | Observação |
|---|---|---|
| CHÁ MATE | `CHÁ MATE GELADO` | nome comercial diferente |
| CLERICOT / SANGRIA — jarra | `CLERICOT 1,2L` · `SANGRIA 1,2L` | a ficha diz "jarra (2-3 pessoas)" |
| CLERICOT / SANGRIA — taça | `CLERICOT 500ML` · `SANGRIA 500ML` | a ficha diz "porção individual" |
| RED BERRY | `RED BERRY SPRITZ ZERO` | |
| COCO ABACAXI | `COCO COM ABACAXI ZERO` | o casamento automático errava para `SUCO ABACAXI` |

> Confirmar: jarra = 1,2 L e taça = 500 ml? A ficha da jarra soma ~900 ml e a individual ~450 ml.

## "Frutas da estação" resolvido pelo PDV

A ficha da Caipirinha e da Caipiroska pede "50 g de frutas da estação", sem dizer qual.
**O PDV diz** — cada sabor é um produto:

| Caipirinha (138 un/mês) | | Caipiroska (203 un/mês) | |
|---|---|---|---|
| Limão | 74 | Frutas vermelhas | 87 |
| Frutas vermelhas | 29 | Limão | 37 |
| Maracujá | 13 | Maracujá | 26 |
| Morango | 8 | Morango | 23 |
| Uva | 7 | Abacaxi | 13 |
| Abacaxi | 4 | Uva | 11 |
| Melancia | 3 | Melancia | 5 |

Logo o de-para é 1:N — uma ficha, vários produtos, cada um debitando a fruta do seu nome.
Existe também `CAIPICOCO` (20 un/mês) sem ficha correspondente no almanaque.

## Fichas órfãs — existem no almanaque e não no PDV

`IRISH COFFEE` · `LILLET SPRITZ` · `LILLET VIVE` · `WHISKY SOUR` (o PDV só tem
`REMON WHISKY SOUR`, que é a variante Nazo da ficha 28).

Ou saíram do cardápio, ou têm outro nome no PDV. Precisa de confirmação.

## Correção de cadastro — dois itens que eu inclui sem justificativa de venda

Ao varrer as fichas contra o catálogo, cadastrei 8 itens ausentes. Cruzando agora com a
venda real, **dois não se sustentam**:

| Item | Situação |
|---|---|
| `MP CHANTILLY` | existia só para o Irish Coffee, que não é vendido |
| `DEST LILLET` | existia só para Lillet Spritz/Vive, que não são vendidos |

Ambos marcados com `sem_venda_no_pdv: true` e `revisar_cadastro`. Decisão do Bruno:
manter (se voltarem ao cardápio) ou inativar.

Os outros seis se confirmaram pela venda:

| Item | Justificativa na venda |
|---|---|
| `MP PEIXE BRANCO` | Sashimi peixe branco 1.455 + Niguiri peixe branco 578 = **2.033 peças/mês** |
| `MP NUTELLA` | Hot Banana com Nutella: **1.151 un/mês** |
| `HORT ALFACE AMERICANA` / `ROXA` | 117 un/mês no PDV + uso nas fichas de salada |
| `DEST RUM` | Mojito, 21 un/mês |
| `MP CARVAO SACO 8KG` | insumo (não sai no PDV); justificado pela NF de R$ 1.400 |
