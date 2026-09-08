# Ficha técnica — Almanaque do Bar 2026 (estruturação)

Fonte: `almanaque_bar_2026.pdf` (camada de texto, extração direta — sem OCR).
Saída: `data/fichas_bar.json`.

## Cobertura

- **39 produtos** (coquetéis) extraídos das páginas 3 a 41.
- **197 linhas de composição**, todas classificadas (0 sem mapeamento).
- **1 linha** precisou de correção manual de parse (`MACA VERDE 15g (2 laminas)`).

| Situação da linha | Linhas | % |
|---|---|---|
| Pronta para calcular consumo e custo | 93 | 47% |
| Travada em **intermediário não cadastrado** | 54 | 27% |
| Travada em **conversão de unidade** | 50 | 25% |

## Bloqueio 1 — intermediários fora do catálogo (54 linhas)

Nenhum destes existe entre os 480 itens. Sem eles, 27% do bar não fecha:

| Intermediário | Usos nas fichas |
|---|---|
| Sumo de limão | 20 |
| Xarope simples | 9 |
| Redução de gengibre | 6 |
| Espuma de gengibre | 5 |
| Redução de hibisco | 3 |
| Redução de pimenta rosa | 3 |
| Redução de maracujá | 2 |
| Redução de amora | 2 |
| Redução de morango | 1 |
| Redução de chá mate | 1 |
| "Frutas da estação" (genérico) | 2 |

O almanaque traz os **ingredientes** de cada redução (páginas de Produções), mas **não o
rendimento final** (quanto sai da panela). Sem rendimento não há como converter
"30 ml de redução" em gramas de gengibre.

Dois caminhos: cadastrar os intermediários como itens contáveis (dispensa rendimento),
ou levantar o rendimento de cada produção. Ver decisão pendente.

## Bloqueio 2 — conversão de unidade (50 linhas)

A ficha mede em ml/g; o catálogo conta em UNID/KG/LITRO.

**Derivável dos próprios documentos do Bruno:**
- `HORT MORANGO BANDEJA` — 1 bandeja = **0,300 kg** (da NF MM, descrição "MORANGO - BJ 300").
- Suco de laranja — o almanaque (Sucos Naturais) dá 753 g de laranja → 300 ml de suco,
  ou seja **2,51 g de laranja por ml**.

**Padrão de mercado, a confirmar:** volume da garrafa de espumante, da lata de água tônica,
da lata de Red Bull, da cápsula Nespresso por dose, e gramas de clara por ovo.

**Só a operação sabe:** gramas por maço de hortelã, manjericão e alecrim; rendimento de sumo
por kg de limão; densidade do chantilly (ficha em g, catálogo em litro).

## Defeitos encontrados NAS FICHAS (para a equipe corrigir)

Auditoria de consistência apontou erros no próprio almanaque:

**Sólido medido em mililitro** (provável erro de digitação):

| Ficha | Ingrediente | Está | Deveria ser |
|---|---|---|---|
| Alfredo Spritz / Sasaki Spritz | Laranja Bahia | 50 ml | 50 g |
| Palácios / Hana | Laranja Bahia | 15 ml | 15 g |
| Clericot / Sangria (jarra) | Abacaxi | 150 ml | 150 g |
| Clericot / Sangria (taça) | Abacaxi | 50 ml | 50 g |
| Red Soda | Pepino japonês | 20 ml | 20 g |

**Mesmo ingrediente com unidade diferente entre fichas:**
- `LARANJA BAHIA` aparece em **g** (Aperol Spritz 30, Lillet Spritz 10, Negroni 10),
  em **ml** (Alfredo Spritz 50, Palácios 15) e em **fatia** (Caminito Tropical 1).
- `ESPUMA DE GENGIBRE` aparece em **ml** (4 fichas) e em **g** (Maradona Mule 10).

> Impacto: o mesmo insumo baixando em unidades diferentes gera CMV errado e impede
> padronização de custo por drink.

## Mapeamento de nomes (ficha → Everest)

Registrado em `data/fichas_bar.json`. Casos que exigiram decisão de equivalência:

| Ficha diz | Item do catálogo |
|---|---|
| GIN | `DEST GIN LARIOS` |
| VODKA / VODKA NACIONAL | `DEST VODKA SMIRNOFF` |
| CACHAÇA BRANCA | `DEST CACHACA SALINISSIMA PRATA` |
| LICOR DE LARANJA | `DEST LICOR STOCK CURACAU` |
| WHISKY BOURBON | `DEST WHISKY JIM BEAM BOURBON` |
| ESPUMANTE (e "ou vinho tinto") | `ESP 1913 SPARKLING BRANCO` |
| CHOPP / CERVEJA | `BARRIL CHOPP BRAHMA` |
| CAFÉ EXPRESSO | `CAFE NESPRESSO RISTRETTO` |
| CLARA DE OVO | `MP OVO` |

Onde havia mais de um item possível (5 cachaças, 7 whiskies, 2 gins, 2 vodkas), a escolha
acima é uma **proposta** — o Bruno confirma qual marca a ficha usa de fato.
