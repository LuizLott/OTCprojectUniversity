# ☕ Oeste Coffee Tech — Modelo Econométrico de Previsão de Preços (2021–2027)

Este repositório contém a modelagem econométrica quantitativa de séries temporais desenvolvida em Python para o projeto de viabilidade de uma indústria cafeeira fictícia.

O objetivo principal deste código é projetar o comportamento dos preços do café arábica beneficiado no mercado físico paranaense para os anos de 2026-2027, fundamentando a estrutura de custos de matéria-prima.

Para a modelagem preditiva do preço da saca de café beneficiado (60 kg) no Paraná, foi utilizado um modelo de regressão linear simples estimado por Mínimos Quadrados Ordinários (MQO/OLS), tendo como variável dependente o preço da saca (R$) e como variável explicativa o índice temporal (mês). A justificativa para a escolha desse modelo baseia-se na identificação de uma tendência de crescimento de longo prazo na série histórica (jan/2021 a dez/2025, n=60 observações mensais), o que torna a regressão linear uma abordagem adequada para capturar a direção e magnitude médias dessa tendência, servindo de base para a projeção de curto e médio prazo. Os dados utilizados têm como fonte a SEAB/DERAL (Preços Recebidos pelo Produtor).

## Fórmulas Utilizadas

| Métrica | Fórmula |
| :--- | :--- |
| **Equação da regressão** | $Y_t = \beta_0 + \beta_1 \cdot X_t + \epsilon_t$ |
| **Coeficiente de Determinação** | $R^2 = 1 - \frac{SQ_{res}}{SQ_{tot}}$ |
| **Erro Médio Absoluto (MAE)** | $MAE = \frac{1}{n} \cdot \sum \|Y_t - \hat{Y}_t\|$ |
| **Raiz do Erro Quadrático Médio (RMSE)** | $RMSE = \sqrt{\frac{1}{n} \cdot \sum (Y_t - \hat{Y}_t)^2}$ |
| **Teste t do coeficiente** | $t = \frac{\hat{\beta}_1}{EP(\hat{\beta}_1)}$, sob $H_0: \beta_1 = 0$ |
| **Teste F (significância global)** | $F = \frac{R^2 / k}{(1 - R^2) / (n - k - 1)}$ |

---

## Testes Estatísticos Realizados

Foram realizados três testes para validar a aderência e as premissas do modelo:

* **Teste t do coeficiente angular** ($H_0: \beta_1 = 0$): $t = 7,83$, $p\text{-valor} < 0,0001$. Rejeita-se $H_0$, comprovando que a tendência de alta identificada ($\beta_1 =$ R$ 21,40/mês) é estatisticamente significativa e não fruto do acaso.
* **Teste F de significância global da regressão**: $F = 61,26$, $p\text{-valor} < 0,0001$, confirmando que o modelo como um todo é estatisticamente significativo.
* **Teste de Shapiro-Wilk** (normalidade dos resíduos): $W = 0,9685$, $p\text{-valor} = 0,123$. Como $p > 0,05$, não se rejeita a hipótese de normalidade dos resíduos, sustentando a premissa de erro gaussiano do modelo.

---

## Resultados e Interpretação

| Métrica | Valor | Interpretação |
| :--- | :--- | :--- |
| **$R^2$** | `0,5137` | **51,37%** da variação do preço da saca é explicada pela tendência linear de tempo. |
| **MAE** | `R$ 300,67` | Em média, as estimativas do modelo se desviam R$ 300,67 do valor real observado. |
| **RMSE** | `R$ 360,63` | O desvio típico dos erros é de R$ 360,63; por ser sempre $\ge MAE$, sua proximidade indica ausência de erros extremos isolados (*outliers*). |
| **$\beta_0$ (intercepto)** | `R$ 574,97` | Preço estimado do modelo no mês inicial da série (jan/2021). |
| **$\beta_1$ (tendência)** | `R$ 21,40/mês` | Crescimento médio estrutural do preço por mês, estatisticamente significativo. |

O $R^2$ de 51% é coerente com a natureza volátil de uma série de preços de *commodities* agrícolas; não se espera que uma tendência linear simples explique a maior parte da variação em um mercado sujeito a choques de oferta e demanda, como o observado em 2024–2025.

---

## Limitações Identificadas

A estatística de Durbin-Watson ($DW = 0,21$) indica forte autocorrelação positiva nos resíduos, o que viola a premissa de independência dos erros do modelo MQO. Isso sugere que os testes de significância ($t$ e $F$) podem estar superestimados e que modelos capazes de capturar a dependência temporal da série (como ARIMA) poderiam refinar essa estimativa em trabalhos futuros. Adicionalmente, a validação da projeção contra a cotação de mercado (Maringá/PR, R$ 1.514,00/saca) evidenciou que o modelo linear tende a superestimar o preço no curto prazo, por não capturar a reversão à média característica de *commodities* agrícolas após choques de oferta.
