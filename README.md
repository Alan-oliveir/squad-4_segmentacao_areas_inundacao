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

Construir um pipeline de treinamento e inferência para segmentação de áreas de enchente com as seguintes metas:
- Treinar modelos de segmentação.
- Aplicar augmentations, validação cruzada e métricas relevantes (IoU, Dice, precisão por pixel).
- Gerar mapas binários e máscaras para análises e tomada de decisão em levantamentos de enchentes.

## Notebooks / Execução

Estamos desenvolvendo e experimentando no Google Colab:

https://colab.research.google.com/drive/10iCD-EJI5tYT9getGcrGlUMDgk8EKl8t?usp=sharing

Instruções rápidas para reproduzir localmente ou no Colab:
1. Baixar o dataset do Kaggle. 
2. Instalar dependências (veja `requirements.txt` ou use o ambiente do Colab).
3. Abrir o notebook no Colab e executar as células.

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

Métricas recomendadas:
- Intersection over Union (IoU)
- Dice Coefficient (F1 para segmentação)
- Precisão/Recall por pixel

Validação:
- Separar um conjunto de teste independente para avaliação final

## Boas práticas

- Usar backbones pré-treinados (ResNet, EfficientNet, etc.) para acelerar convergência.
- Aplicar aumento geométrico e fotométrico (flip, rotate, elastic transform, brilho/contraste).
- Monitorar overfitting e usar early stopping quando necessário.

## Contribuidores

- [Alan de Oliveira Gonçalves](https://github.com/Alan-oliveir)
- [Albertina Costa Rodrigues](https://github.com/albiecr)
- [Carlos Victor Albuquerque Oliveira](https://github.com/Carlos-Vic)
- Eduardo Fabian de Oliveira
- Elzilene Montanha Machado

## Referências

- Dataset: https://www.kaggle.com/datasets/faizalkarim/flood-area-segmentation
