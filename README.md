Projeto 03 - Modelo de Mapeamento de Comportamento Transacional + Detecção de Anomalias
Este repositório contém os arquivos resultantes do processo de modelagem de comportamento padrão de transações financeiras, utilizando Autoencoder, KMeans e XGBoost para detecção de padrões e identificação de comportamentos atípicos ou suspeitos.

O pipeline desenvolvido abrange desde o pré-processamento dos dados até a classificação final de risco, com base em múltiplas estratégias:

Reconstrução por Autoencoder

Distância ao cluster (KMeans)

Regras heurísticas

Classificação supervisionada com XGBoost

📁 Estrutura dos Modelos (modelos/)
Arquivo	Descrição
modelo_autoencoder.keras	Autoencoder treinado para reconstrução do comportamento
modelo_encoder.keras	Encoder que extrai representação latente de 3 dimensões
kmeans_auto.pkl	Modelo de clusterização treinado sobre os vetores latentes
scaler.pkl	RobustScaler usado para normalizar variáveis numéricas
colunas_scaler.pkl	Lista de colunas normalizadas pelo scaler
encoder_tipo_transacao.pkl	OneHotEncoder para o tipo de transação
encoder_semana.pkl	OneHotEncoder para o dia da semana
encoder_horario.pkl	OneHotEncoder para a faixa horária da transação
modelo_xgb.pkl	Classificador XGBoost treinado com SMOTE para detecção supervisionada

🗃️ Arquivos Gerados na Inferência (resultados/ ou raiz do projeto)
Arquivo	Descrição
transacoes_comportamento_por_conta.csv	Base agregada por conta_id, com métricas de comportamento padrão da conta
transacoes_analisadas.csv	Todas as transações com colunas de reconstrução, cluster, score e decisão final
transacoes_anomalas_log.csv	Subconjunto com transações rotuladas como anômalas + justificativas

📊 O que é transacoes_comportamento_por_conta.csv?
Esse arquivo contém o perfil médio de comportamento de cada conta, gerado a partir de:

Valor médio e desvio padrão das transações

Percentuais de tipos de transação (pix, transferência, etc.)

Frequência de transações no fim de semana

Distribuição por horário (manhã, tarde, etc.)

Percentual de mesma titularidade

Essas métricas são usadas como referência para detectar desvios individuais em novas transações, comparando com o comportamento histórico da conta.

🤖 Como funciona a Inferência?
O pipeline de inferência passa por 2 etapas principais:

1. Inferência de Comportamento
Usa o Autoencoder e o KMeans para gerar:

Erro de reconstrução

Cluster da transação

Desvio de comportamento da conta

2. Inferência de Anomalia
Com base nos resultados anteriores, aplica:

Regras heurísticas (valor extremo, madrugada, frequência alta)

XGBoost supervisionado, treinado com rótulos sintéticos (anomalia_confirmada)

Gera a coluna nivel_suspeita com os níveis: nenhuma, baixa, media, alta

🛠️ Como executar o pipeline completo
Você pode executar todas as etapas com o seguinte comando:

bash
Copiar
Editar
python rodar_pipeline.py
Esse script executa:

inferencia/inferencia_comportamento.py
➜ Gera comportamento, clusters, reconstrução

inferencia/inferencia_anomalia.py
➜ Aplica regras, XGBoost, e classifica as transações

Salva os arquivos gerados na pasta resultados/

🎯 Objetivos do Projeto
Representar o comportamento padrão de contas a partir de dados transacionais

Identificar grupos distintos de comportamento com Autoencoder + KMeans

Detectar transações suspeitas com base em:

Erro de reconstrução

Distância ao cluster

Regras heurísticas (valor, horário, frequência)

Classificação supervisionada (XGBoost)
