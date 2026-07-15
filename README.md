# Segmentação de Áreas de Inundação

Este repositório contém o projeto de Machine Learning para segmentação de áreas inundadas em imagens aéreas/satélite. O objetivo é treinar e avaliar modelos de segmentação que identifiquem com precisão a região de água em imagens obtidas após eventos de enchente.

## Dataset

O conjunto de dados utilizado está disponível no Kaggle:

https://www.kaggle.com/datasets/faizalkarim/flood-area-segmentation

Descrição resumida:
- Contém 290 imagens e máscaras autoanotadas (cada máscara indica a região coberta por água).
- As máscaras foram geradas com o Label Studio (rotulador open-source).
- Imagens e máscaras são adequadas para treinar modelos de segmentação semântica.

## Objetivo do Projeto

Construir e comparar modelos de segmentação semântica para identificar áreas de enchente, com as seguintes metas:
- Treinar e avaliar diferentes arquiteturas de segmentação.
- Aplicar validação em conjunto de teste independente com métricas relevantes (IoU, Dice, F1-Score, precisão e recall por pixel).
- Propor e validar uma arquitetura própria (SE-UNet) como alternativa aos baselines da literatura.

## Notebooks / Execução

O notebook completo, com pré-processamento, treinamento e avaliação dos três modelos, está em:

`notebooks/squad_4_segmentacao_areas_inundacao.ipynb`

Instruções rápidas para reproduzir localmente ou no Colab:
1. Baixar o dataset do Kaggle.
2. Instalar dependências (veja `requirements.txt` ou use o ambiente do Colab).
3. Abrir o notebook e executar as células em ordem.

## Resultados

Foram treinadas e avaliadas três arquiteturas no conjunto de teste (42 imagens):

| Modelo | IoU Médio | IoU Mediano | Dice Médio | F1-Score | Acurácia |
|---|---|---|---|---|---|
| U-Net (baseline) | **0.7460** | **0.8207** | 0.8318 | **0.8875** | ~0.9038 |
| Attention U-Net (baseline) | 0.6062 | 0.6632 | 0.7210 | 0.7921 | 0.8294 |
| **SE-UNet (proposta própria)** | 0.7408 | 0.8007 | **0.8319** | 0.8743 | 0.8933 |

**Principais conclusões:**
1. A U-Net simples obteve o melhor IoU médio, confirmando que arquiteturas mais enxutas podem superar variantes mais complexas em datasets pequenos (~192 imagens de treino).
2. A **SE-UNet** (nossa contribuição, com blocos Squeeze-and-Excitation de atenção por canal) ficou estatisticamente empatada com a U-Net em IoU e Dice, com a vantagem de maior consistência entre predições (desvio padrão do IoU: ±0.1978 vs ±0.2113).
3. A Attention U-Net (atenção espacial nas skip connections) teve o pior desempenho — o mecanismo parece exigir mais dados de treino do que os disponíveis para convergir bem.

## Estrutura do Repositório

A estrutura esperada do projeto é:

```
.
├── data/
│   ├── raw/          # imagens e máscaras originais (diretório para colocar o dataset do Kaggle)
│   ├── processed/    # arquivos de dados processados (ex.: recortes, tamanhos padronizados)
│   └── external/     # outros datasets ou recursos externos
├── docs/              # documentação
├── models/            # modelos treinados e checkpoints
├── notebooks/         # notebooks de exploração e treinamento (inclui o Colab link)
├── src/               # código fonte (pré-processamento, datasets, treinadores, etc.)
├── app.py             # (opcional) script para inferência/visualização
├── requirements.txt   # dependências (para ambiente local)
└── README.md
```

## Métricas e Avaliação

Métricas calculadas no conjunto de teste, a partir da matriz de confusão pixel a pixel:
- Intersection over Union (IoU)
- Dice Coefficient (F1 para segmentação)
- Precisão/Recall por pixel

Validação:
- Conjunto de teste independente, avaliado com o melhor checkpoint de cada modelo (`ModelCheckpoint` monitorando `val_iou_metric`).

## Técnicas utilizadas

- Loss combinada Dice + Binary Crossentropy.
- `ReduceLROnPlateau` para redução adaptativa do learning rate.
- Dropout nos blocos convolucionais para mitigar overfitting em um dataset pequeno.
- Comparação entre atenção espacial (Attention U-Net) e atenção por canal (SE-UNet) como estratégias de melhoria sobre a U-Net baseline.

## Contribuidores

- [Alan de Oliveira Gonçalves](https://github.com/Alan-oliveir)
- [Albertina Costa Rodrigues](https://github.com/albiecr)
- [Carlos Victor Albuquerque Oliveira](https://github.com/Carlos-Vic)
- Eduardo Fabian de Oliveira
- Elzilene Montanha Machado

## Referências

- Dataset: https://www.kaggle.com/datasets/faizalkarim/flood-area-segmentation
