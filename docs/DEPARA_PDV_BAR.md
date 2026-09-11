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

---

# De-para PDV × ficha — Caminito

Fonte: índice do almanaque Caminito (52 fichas) contra `Venda Unid - Caminito`
(465 produtos, agosto/2026). Saída: `data/depara_pdv_caminito.json`.

## Cobertura

**49 das 52 fichas** casaram com produto do PDV, cobrindo **10.599 unidades/mês**.
Sem produto no PDV: `Creme azedo` (é pré-preparo, correto), `Sanduiche KIDS`, `KIDS CARNE`.

## O nome do PDV não é o nome da ficha

O casamento automático por texto acertou ~70% e deu **12 falsos negativos** — fichas que
existiam no PDV com nome comercial diferente:

| Ficha | Produto no PDV | un/mês |
|---|---|---|
| Sanduiche cheese burguer | `CHEESEBURGER` | 889 |
| Sanduiche Labaki burguer | `LABAKI BURGER` | 291 |
| Sanduiche ensalada | `ENSALADA BURGER` | 174 |
| Sanduiche Pastrami | `PASTRAMI BURGER` | 87 |
| Linguini rústico | `Linguine ao molho rústico` | 245 |
| Tábua de legumes | `LEGUMES GRELHADOS` (+ EXEC, FAM, COM) | 314 |
| Salada executivo e kids | `SALADINHA` (EXEC / DO DIA / KIDS) | 421 |
| Pudim | `PUDIM DE DOCE DE LEITE` | 108 |
| Churros | `CHURROS DE DOCE DE LEITE` | 80 |
| CROQUETA SUÍNA | `CROQUETE DE COSTELINHA` | 98 |
| Lomo em crosta | `LOMO COM CROSTA DE QUEIJO` (+ variantes) | 123 |
| Linguiças | `LINGUIÇA CAMINITO/IPA/FORMIGA 200G` | 209 |

Conclusão igual à das notas: **de-para de produto tem que ser aprovado item a item**;
depois fica determinístico.

## Fichas recuperadas das imagens do Classroom

7 páginas do almanaque eram apenas link do Google Classroom, sem texto. As imagens
estavam embutidas no PDF e foram **lidas e transcritas** (`data/fichas_caminito_salaaula.json`):

| Ficha | Composição |
|---|---|
| PÃO DE ALHO | pão de alho 1 un + manteiga com salsa 0,002 kg |
| PÃO DE LINGUIÇA | PROD pão de linguiça 1 un + PP chimichurri 0,040 + salsa 0,001 |
| PASTEL DE PASTRAMI | PROD pastel 4 un + PROD geleia de abacaxi 0,040 |
| SALTEÑAS (empanada) | empanada 1 un |
| CROQUETA SUÍNA | croqueta 6 un + BBQ 0,030 |
| BATATA COM BRISKET | batata palito 7x7 0,350 + PROD brisket 0,100 + ovo 1 un + maionese tabasco 0,025 + cebolete 0,002 + PP pó de bacon 0,012 + PROD picles de cebola 0,010 |

**Defeito encontrado:** a imagem nomeada `Coxinha de cupim.png` (página 40) contém a ficha
de **LINGUIÇAS**, duplicando a da página 21. Ou seja **`COXINHA DE CUPIM` (68 un/mês) não
tem ficha técnica** — a imagem foi anexada errada no almanaque.

## Produtos com venda e sem ficha — onde o CMV é cego

| Produto | un/mês | Situação |
|---|---|---|
| **BATATA FRITA** (+ EXEC + ADD SAND) | **3.136** | 2º mais vendido do Caminito, **sem ficha** |
| Parmegiana de frango 200g | 755 | a ficha "Parmegiana da chefe" é de carne |
| ARROZ BRANCO (+ EXEC) | 750 | sem ficha |
| CHOCOTORTA - WEEK | 273 | sobremesa sem ficha |
| COXINHA DE CUPIM | 68 | imagem trocada no almanaque |

**Proteínas porcionadas** (~2.500 un/mês: bombom de alcatra, chorizo, galeto, filé, ancho,
fraldinha, flat iron) **não precisam de ficha de composição** — precisam de de-para direto
produto do PDV → item `PO` do catálogo. É trabalho diferente e mais simples.

**Componentes vendidos no PDV** (3.461 un/mês, cadastrados em minúscula): Mix de queijos 817,
Crispy de alho poró 623, Batata palha 426, Queijo minas 293, Cenoura 289, Mix de folhas 276,
Mussarela 240, Brócolis 228. Parecem transferência interna ou buffet — o Bruno precisa
confirmar o que são antes de entrarem no cálculo.

## Cadastro sujo no PDV

Respostas de checklist cadastradas como produto **e registrando venda**:

| Produto | un/mês |
|---|---|
| `Sim, misturar os ingredientes` (Caminito) | 229 |
| `Não, não misturar` (Caminito) | 56 |
| `SIM, MISTURAR OS INGREDIENTES` (Nazo) | 7 |

Poluem relatório de venda e análise de mix. Vale limpar no PDV.
