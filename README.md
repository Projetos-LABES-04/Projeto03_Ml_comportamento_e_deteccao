# Projeto 03 - Modelo de Mapeamento de Comportamento Transacional + Detecção de Anomalias

Este repositório contém os arquivos resultantes do processo de modelagem de comportamento padrão de transações financeiras, utilizando **Autoencoder**, **KMeans** e **XGBoost** para detecção de padrões e identificação de comportamentos atípicos ou suspeitos.

O pipeline desenvolvido abrange desde o **pré-processamento dos dados** até a **classificação final de risco**, com base em múltiplas estratégias:

- ✅ Reconstrução por Autoencoder  
- ✅ Distância ao cluster (KMeans)  
- ✅ Regras heurísticas  
- ✅ Classificação supervisionada com XGBoost

---

## 📁 Estrutura dos Modelos (`modelos/`)

| Arquivo                    | Descrição                                                                 |
|---------------------------|---------------------------------------------------------------------------|
| `modelo_autoencoder.keras`| Autoencoder treinado para reconstrução do comportamento                  |
| `modelo_encoder.keras`     | Encoder que extrai representação latente de 3 dimensões                  |
| `kmeans_auto.pkl`          | Modelo de clusterização treinado sobre os vetores latentes               |
| `scaler.pkl`               | RobustScaler usado para normalizar variáveis numéricas                   |
| `colunas_scaler.pkl`       | Lista de colunas normalizadas pelo scaler                                |
| `encoder_tipo_transacao.pkl`| OneHotEncoder para o tipo de transação                                  |
| `encoder_semana.pkl`       | OneHotEncoder para o dia da semana                                       |
| `encoder_horario.pkl`      | OneHotEncoder para a faixa horária da transação                          |
| `modelo_xgb.pkl`           | Classificador XGBoost treinado com SMOTE para detecção supervisionada    |

---

## 🗃️ Arquivos Gerados na Inferência (`resultados/` ou raiz do projeto)

| Arquivo                            | Descrição                                                                 |
|-----------------------------------|---------------------------------------------------------------------------|
| `transacoes_comportamento_por_conta.csv` | Base agregada por `conta_id`, com métricas de comportamento padrão da conta |
| `transacoes_analisadas.csv`       | Todas as transações com colunas de reconstrução, cluster, score e decisão final |
| `transacoes_anomalas_log.csv`     | Subconjunto com transações rotuladas como anômalas + justificativas      |

---

## 🤖 Como funciona a Inferência?

O pipeline de inferência passa por **2 etapas principais**:

### 1. Inferência de Comportamento
Utiliza o Autoencoder e o KMeans para gerar:
- Erro de reconstrução  
- Cluster da transação  
- Desvio em relação ao comportamento da conta

### 2. Inferência de Anomalia
Com base nos resultados anteriores, aplica:
- Regras heurísticas (valor extremo, madrugada, frequência alta)  
- XGBoost supervisionado, treinado com rótulos sintéticos (`anomalia_confirmada`)  
- Gera a coluna `nivel_suspeita` com os níveis: `nenhuma`, `baixa`, `media`, `alta`



Regras heurísticas (valor, horário, frequência)

Classificação supervisionada (XGBoost)


