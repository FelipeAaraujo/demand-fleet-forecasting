# Demand & Fleet Sizing Forecasting

**Python (regressão com tendência + sazonalidade) — dimensionando frota sem comprar veículo à toa**

## Contexto
A diretoria da RotaViva não sabe se deve comprar mais caminhões, reduzir a frota ou só realocar
veículos entre as 4 regiões de operação para o próximo trimestre.

## Problema de negócio
Prever a demanda de transporte (volume em m³) por região para os próximos 3 meses e traduzir isso
em necessidade real de frota, evitando decisão no "achismo".

## Base de dados (gerada para este projeto)
- Série mensal de **36 meses** (jan/2023–dez/2025) por região (4 regiões), com tendência de
  crescimento e sazonalidade distintas por região (ex.: Sul e Centro-Oeste com pico ligado a safra
  agrícola).
- Arquivo: `demanda_mensal_regiao.csv`.

## Modelagem (`generate_and_analyze.py`)
- Modelo de regressão linear com **tendência (t) + sazonalidade mensal (dummies de mês)**, um
  modelo por região.
- **Backtest**: treino nos primeiros 30 meses, teste nos últimos 6 meses reais, validando o
  modelo antes de confiar na previsão futura.
- Previsão para Q1/2026 (jan–mar) a partir do modelo re-treinado com o histórico completo.

## Resultado (calculado sobre a base e o backtest reais)
- **MAPE no backtest**: 0,9% (Sudeste), 1,5% (Sul), 0,9% (Nordeste), 1,4% (Centro-Oeste) — o
  modelo captura bem o padrão de tendência e sazonalidade de cada região.
- Traduzindo a previsão de Q1/2026 em frota necessária (capacidade média por veículo/mês):
  - **Sudeste**: frota atual 22, necessária ~18,9 → **excesso de ~3 veículos**
  - **Sul**: frota atual 13, necessária ~13,3 → equilibrado
  - **Nordeste**: frota atual 14, necessária ~11,8 → **excesso de ~2 veículos**
  - **Centro-Oeste**: frota atual 11, necessária ~11,2 → equilibrado
- Conclusão: **não é preciso comprar veículo nenhum** para o próximo trimestre — o mesmo
  resultado vem de realocar ~5 veículos de Sudeste/Nordeste (onde sobra capacidade) para reforçar
  picos sazonais pontuais em Sul/Centro-Oeste, se necessário.

## Recomendação
Plano de realocação sazonal trimestral entre regiões, revisado a cada novo backtest, em vez de
decisão de frota fixa por calendário.

## Ferramentas
Python — Pandas, NumPy, Scikit-learn (regressão com variáveis de tendência e sazonalidade).

## Competências demonstradas
Modelagem de série temporal sem depender de bibliotecas especializadas, backtest antes de
confiar em previsão, tradução de previsão estatística em decisão de capex/opex.
