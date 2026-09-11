# Nazo — explosão dos combinados e demanda real por peça

Fonte: fichas de finalização da Seção I do almanaque Nazo (transcritas) × `Venda Unid - Nazo`
(agosto/2026). Saídas: `data/nazo_demanda_pecas.json` e `data/depara_pdv_nazo.json`.

## Conferência das fichas de combinado — todas fecham

| Combinado | Vendas/mês | Peças na ficha | Confere |
|---|---|---|---|
| 12 peças | 354 | 12 | ✓ |
| 18 peças (premium) | 41 | 18 | ✓ |
| 20 peças | 158 | 20 | ✓ |
| 20 peças vegetariano | 11 | 20 | ✓ |
| 22 peças (premium) | 81 | 22 | ✓ |
| 24 peças | 57 | 24 | ✓ |
| 30 peças (premium) | 62 | 25 + carpaccio | ✓ |
| 32 peças | 74 | 32 | ✓ |
| 42 peças (premium) | 133 | 37 + carpaccio | ✓ |

Nos combinados de 30 e 42 peças o carpaccio entra como `0,050 kg` com medida caseira
"5 fatias" — conta como 5 peças. 25+5 = 30 e 37+5 = 42. As nove fichas estão consistentes.

## Demanda total por peça = rodízio avulso + peças dentro dos combinados

Os combinados geram **20.355 peças/mês**; o rodízio avulso registra **90.818**.
Total: **111.183 peças/mês**, em 83 tipos distintos. **76.348** levam peixe ou proteína crua.

| Peça | De combinado | Avulso rodízio | **Total/mês** |
|---|---|---|---|
| Sashimi salmão | 4.035 | 11.433 | **15.468** |
| Barriga de salmão | 596 | 6.604 | **7.200** |
| Sashimi atum | 1.750 | 3.709 | **5.459** |
| Uramaki filadélfia | 3.430 | 1.442 | **4.872** |
| Niguiri salmão | 2.408 | 1.823 | **4.231** |
| Hot filadélfia | — | 3.572 | **3.572** |
| Salmão thai | 1.104 | 2.195 | **3.299** |
| Sashimi peixe branco | 1.372 | 1.455 | **2.827** |
| Jô salmão | 1.306 | 1.501 | **2.807** |
| Salmão maracujá | — | 2.782 | **2.782** |
| Jô salmão maçaricado | 330 | 2.002 | **2.332** |
| Sashimi anchova defumada | — | 2.204 | **2.204** |
| Hot poró | — | 2.008 | **2.008** |
| Jô salmão q. coalho | — | 1.921 | **1.921** |
| Ebi king | — | 1.833 | **1.833** |

## Exposição por proteína

| Proteína | Peças/mês |
|---|---|
| Salmão (inclui barriga, filadélfia, jô, tataki, thai, maracujá) | **64.884** |
| Atum | 8.492 |
| Anchova | 4.668 |

## Sensibilidade — o que 1 grama de erro custa

| Item | Peças/mês | Impacto de 1 g de erro |
|---|---|---|
| Sashimi salmão | 15.468 | **15,5 kg/mês** |
| Barriga de salmão | 7.200 | 7,2 kg/mês |
| **Todo o salmão** | 64.884 | **64,9 kg/mês** |

## Curva de cobertura da pesagem

| Pesagens | Volume coberto |
|---|---|
| 5 | 33% |
| 10 | 47% |
| 15 | 56% |
| 20 | 64% |
| **25** | **71%** |
| 30 | 76% |

A cauda é longa (83 tipos de peça), então não existe atalho de 5 pesagens. O ponto de
equilíbrio razoável é **as 10 primeiras** (47% do volume, meia hora de trabalho) para
destravar o cálculo, evoluindo para 25 depois.

**Ordem de pesagem, por volume real:**
1. Sashimi salmão · 2. Barriga de salmão · 3. Sashimi atum · 4. Uramaki filadélfia ·
5. Niguiri salmão · 6. Hot filadélfia · 7. Salmão thai · 8. Sashimi peixe branco ·
9. Jô salmão · 10. Salmão maracujá

O que pesar: **o insumo cru por peça** (gramas de salmão que entram em 1 sashimi), não o
peso final montado.

## Observação sobre o rodízio

O rodízio é 69% do volume do Nazo e está registrado peça a peça no PDV. Isso significa que
o consumo teórico do Nazo pode ser calculado com a mesma precisão do à la carte — algo raro
em operação de rodízio. O único dado que falta é a gramatura.
