# Diagnóstico — Notas de entrada e lacunas do catálogo

Base: 4 notas reais (MM Hortifruti 517083 e 517251, Vegan 49017, Carvão Triângulo 5976)
testadas contra o catálogo de itens. Data do diagnóstico: 08/09/2026.

## 1. O que as notas são, tecnicamente

- **DANFE (NF-e)** de fornecedor — não romaneio interno do CPD.
- **Sem camada de texto**: PDFs são imagem (digitalizados no CamScanner) → exigem leitura por IA.
- **Layout estável** entre fornecedores diferentes (padrão DANFE).
- Todas têm **chave de acesso de 44 dígitos**; a nota do Carvão informa que o XML pode
  ser obtido em `olist.com/nfe`. O caminho XML elimina o OCR, se um dia estiver disponível.
- Colunas úteis da tabela de produtos: `CÓD. PROD.` · `DESCRIÇÃO` · `UN` · `QTD` · `V.UNIT` · `V.TOTAL`.

### Unidade (loja)
Destinatário: **MULT COMERCIO DE PRODUTOS ALIMENTICIOS LTDA — CNPJ 35.631.524/0003-27**,
nome fantasia *CAMINITO PARRILLA SETOR GRAFICO SIG*.
**SIG e Sudoeste são a mesma loja** (confirmado pelo Bruno). O CNPJ acima é o CNPJ da unidade
e deve ser aceito como destinatário válido.

## 2. Por que casamento automático por descrição NÃO pode gravar sozinho

Teste com 33 linhas de nota contra 471 itens: **82% de "acerto" automático — porém com
falsos positivos de alta confiança**, que são piores que uma falha:

| Nota | Casou (errado) | Correto |
|---|---|---|
| `ALHO` 3 kg | `PROD PAO DE ALHO` (unid) | `HORT ALHO DESCASCADO KG` |
| `CEBOLA` 10 kg | `HORT CEBOLA ROXA KG` | `HORT CEBOLA BRANCA KG` |

E falhas óbvias por grafia: `BERINJELA`→`BERINGELA` (score 0,22), `LIMAO TAITI`→`LIMAO TAHITI`.

**Decisão de arquitetura:** o de-para é chaveado por
**`CNPJ do fornecedor` + `CÓD. PROD.`** → item do catálogo + fator de conversão,
aprovado **uma vez** pelo Bruno. Depois disso a entrada é **determinística, sem IA**.
A IA só sugere (com nível de confiança) quando aparece um código novo, e **nunca grava sozinha**.
Ver `data/de_para_fornecedores.json` (32 mapeamentos verificados).

## 3. Lacunas confirmadas do catálogo (comprado/consumido, não estava no estoque)

Detectadas pela varredura das 3 fichas técnicas contra o catálogo + pelas notas.
Cadastradas com `pendente_validacao: true` e mínimo 0 (unidade assumida, a confirmar):

| Item cadastrado | Un. assumida | Evidência |
|---|---|---|
| `HORT ALFACE AMERICANA` | KG | 13 usos em fichas; 30 kg na NF Vegan 49017 |
| `HORT ALFACE ROXA` | KG | Salada Caminito/Saladinha; 4 kg na NF Vegan 49017 |
| `MP CARVAO SACO 8KG` | UNID | NF Carvão 5976: 50 sacos de 8 kg = R$ 1.400 |
| `MP PEIXE BRANCO` | KG | sashimi/niguiri de peixe branco nos combinados 20/24/32/42 |
| `DEST RUM` | LITRO | Mojito, 50 ml/drink |
| `DEST LILLET` | LITRO | Lillet Spritz e Lillet Vive, 50 ml/drink |
| `MP CHANTILLY` | LITRO | Irish Coffee, 20 g/drink |
| `MP NUTELLA` | KG | Hot Banana com Nutella (Nazo) |

> Impacto de gestão: item fora da contagem = custo entra pela nota e nunca sai pelo consumo.
> Isso distorce o CMV e a margem.

## 4. Divergência de unidade (36% das linhas testadas)

A nota fatura em **KG**; o catálogo conta em **MAÇO / BANDEJA / UNIDADE**. Item certo com
unidade errada gera quantidade errada. Ver `data/conversoes_pendentes.json`.

- **7 fatores derivados da própria nota** (o fornecedor declara o peso na descrição):
  morango BJ 300 = 0,300 kg · flor comestível = 0,070 · tomilho = 0,100 · nabo = 0,150 ·
  rúcula = 0,200 · salsa crespa = 0,200 · sálvia = 0,080. **A validar.**
- **4 fatores desconhecidos**, dependem da operação: `HORT ABACAXI`,
  `HORT CEBOLETE MACO`, `HORT CEBOLINHA MACO`, `HORT SALSA MACO`.

## 5. Pendências de decisão

1. `ABOBORA ITALIA` (nota MM, cód. 101) — catálogo só tem `HORT ABOBORA CABOTIA KG`.
   Variedade diferente: cadastrar item novo ou mapear para cabotiá?
2. **Salmão/atum de sushi**: catálogo tem `PO SALMAO 200G` (porção de grelha),
   `PROD SALMAO POKE` (mín. 0), `MP BIT SALMAO DEFUMADO`, `PROD ATUM POKE` (mín. 0).
   Não há item a granel com mínimo real para o sushi — maior custo do Nazo. Qual item recebe
   o salmão sashimi do CPD?
3. Confirmar as unidades assumidas dos 8 itens novos (§3) e definir seus mínimos.

## 6. Falsos "gaps" — não são lacunas

- **Intermediários por desenho** (explodem por receita, não são contáveis): reduções, xaropes
  caseiros, sumo de limão, arroz cozido, molhos, vinagrete, massa de hot, patês, farofas.
- **De-para de nome** (o item existe, o matcher falhou): Alcaparrone→`MP ALCAPARRONES`,
  Burrata→`MP QUEIJO BURRATA`, Salsinha→`HORT SALSA MACO`, Vermute Rosso→`DEST MARTINI ROSSO`,
  Azeite de oliva→`MP AZEITE EXTRA VIRGEM`, Castanhas→`MP CASTANHA DE CAJU XEREM`,
  Vodka nacional→`DEST VODKA SMIRNOFF`, Sorvete de creme→`MP SORVETE DE BAUNILHA`,
  Nori→`MP ALGA NORI`, Farinha de tempurá→`MP TEMPURA`.
- **Conversão, não ausência**: Espumante (ficha em ml ↔ `ESP …` em garrafa),
  Café expresso (ml ↔ cápsula Nespresso), Água tônica e Red Bull (ml ↔ unidade).
- **Peças de sushi** (niguiri, sashimi, uramaki, jô): não são itens de estoque — é a camada de
  ficha faltando (gramatura), já registrada como lacuna em `MODELO_DADOS.md §7`.
