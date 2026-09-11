# Relatório de vendas — estrutura e achados

Arquivos: `CP_SG_VENDAS.xlsx` (Caminito) e `NZ_SG_VENDAS.xlsx` (Nazo), unidade SG (Sudoeste).

## Estrutura

Três abas por arquivo: **Venda R$**, **Venda Unid**, **Dados Agosto**.
Layout: linhas = produtos do PDV, colunas = dias (agosto/2026, 31 dias) + coluna Total.
A aba que interessa para consumo é **Venda Unid**.

| | Produtos | Unidades no mês |
|---|---|---|
| Caminito | 465 | 40.964 |
| Nazo | 554 | 137.971 |

Há uma linha "Total Geral" dentro da planilha que precisa ser descartada na leitura.

## Achado principal — o rodízio do Nazo é registrado peça a peça

**88 produtos com sufixo `- R`** (rodízio) somam **95.281 peças no mês — 69% de todo o
volume do Nazo**. Exemplos: `SASHIMI SALMÃO 1un - R` (9.352), `BARRIGA SALMÃO TRUFADA 1un - R`
(5.234), `SASHIMI ATUM 1un - R` (3.709).

Isso resolve o problema que parecia estrutural: num rodízio normalmente se vende "1 rodízio"
e não se sabe o que o cliente comeu. **Aqui o PDV registra cada peça consumida.** Ou seja,
a explosão da venda pela ficha técnica é possível no nível da peça.

Com isso, a gramatura por peça deixa de ser um detalhe e vira **o gargalo número um do CMV do
Nazo**.

## Sensibilidade da gramatura

**78.894 peças com peixe cru** foram consumidas no mês.

| Gramatura média assumida | Peixe consumido no mês |
|---|---|
| 10 g/peça | 788,9 kg |
| 12 g/peça | 946,7 kg |
| 15 g/peça | 1.183,4 kg |

> **Cada 1 grama de erro na gramatura média = 78,9 kg de peixe por mês** de diferença no CMV.
> Multiplicado pelo preço do kg, é o tamanho do risco de estimar em vez de medir.

**Conclusão:** estimar gramatura com padrão de mercado aqui é inaceitável. Tem que pesar.

## A pesagem necessária (13 medições cobrem 96%)

| Família | Produtos | Peças/mês |
|---|---|---|
| Sashimi | 9 | 23.979 |
| Uramaki / Roll / Maki | 18 | 10.666 |
| Jô | 7 | 9.661 |
| Hot | 5 | 9.280 |
| Ceviche / Poke / Tataki | 8 | 6.353 |
| Ebi / Camarão | 7 | 6.289 |
| Niguiri | 7 | 6.013 |
| Salmão (outros) | 4 | 5.918 |
| Barriga | 1 | 5.234 |
| Sobremesas | 4 | 3.763 |
| Gyoza / Harumaki / Frituras | 2 | 2.045 |
| Shimeji | 2 | 1.767 |
| Robata | 4 | 916 |
| Não classificados | 10 | 3.397 |

Pesar **uma peça de cada família** (13 pesagens) cobre **96% do volume do rodízio**.
O ideal é pesar o **insumo cru por peça** (ex.: quantos gramas de salmão vão em 1 sashimi),
não o peso final montado.

## Caminito — versões são produtos distintos, confirmado pelo PDV

27 produtos com prefixo `EXEC` (5.231 unidades): `EXEC ARROZ CAMINITO` (794),
`EXEC BATATA FRITA` (597), `EXEC BOMBOM DE ALCATRA - 250g` (456), `EXEC FAROFA DE OVOS` (433).

Confirma o que a ficha já indicava: Executivo e À la carte têm composição diferente e precisam
ser tratados como produtos separados no de-para PDV × ficha.

## Próximos passos da integração

1. De-para **produto do PDV × ficha técnica** (465 + 554 nomes contra as fichas).
2. Gramatura das peças (pesagem acima).
3. Explosão: venda × ficha = consumo teórico.
4. Variância: consumo teórico × consumo real (contagem) = quebra/desvio.
