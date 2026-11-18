
# Classificador de Lixo Reciclável com Redes Neurais

Trabalho Final da disciplina de **Redes Neurais** | Turma: 6º Período | Data: 02/12/2026

## Integrantes

- Wlademir Pontes Xavier
- Celso Vinícius Souza
- João Gabriel da Silva Nemezio
- Daniel Paulino da Silva Neto

## Descrição

O sistema recebe a **foto de um objeto** e responde de qual material ele é: papelão, vidro, metal, papel, plástico ou rejeito (lixo comum). O objetivo é apoiar a separação correta do lixo para reciclagem.

- **Área:** visão computacional
- **Tarefa:** classificação multiclasse (6 classes)
- **Entrada:** imagem colorida
- **Saída:** classe prevista e a confiança do modelo

**Relevância:** separar o lixo corretamente é o primeiro passo da reciclagem, e muita gente erra a categoria do material. Descartar errado contamina o material reciclável.

**Quem poderia usar:** aplicativos de educação ambiental, escolas, lixeiras inteligentes e cooperativas de reciclagem, sempre como apoio e não como decisão final.

## Dados

- **Conjunto:** TrashNet (Thung e Yang, Stanford, 2016), versão redimensionada.
- **Origem:** https://github.com/garythung/trashnet e https://huggingface.co/datasets/garythung/trashnet (licença MIT).
- **Conteúdo:** 2.527 fotos originais em 6 classes. Após remover 3 duplicatas, restaram 2.524 (nenhuma imagem corrompida).
- **Formato:** imagens coloridas JPG de 512x384 pixels, redimensionadas para 224x224 na entrada da rede.
- **Classes desbalanceadas:** `paper` 594, `glass` 501, `plastic` 482, `metal` 410, `cardboard` 403 e `trash` 137.
- **Separação:** 70% treino (1.766), 15% validação (379), 15% teste (379), estratificada por classe, com semente fixa (42).
- **Cuidados contra vazamento de dados:** duplicatas removidas por hash, separação feita antes de qualquer ajuste, normalização do baseline ajustada só no treino, augmentação só no treino, teste usado uma única vez no final.

Citação: Thung, G. e Yang, M. (2016). *Classification of Trash for Recyclability Status*. Stanford CS229.

## Ferramentas

- Python, Google Colab (GPU)
- TensorFlow / Keras (rede neural, MobileNetV2 pré-treinada no ImageNet)
- scikit-learn (baseline, divisão dos dados, métricas)
- pandas, NumPy, Matplotlib, Pillow
- huggingface_hub (download do dataset)

## Etapas

1. Definição do problema e do objetivo
2. Exploração dos dados (distribuição das classes, exemplos)
3. Limpeza (duplicatas, imagens corrompidas) e separação treino/validação/teste
4. **Baseline:** (a) classe mais frequente; (b) regressão logística sobre pixels 32x32
5. **Representação:** imagem 224x224x3 normalizada para [-1, 1]; a MobileNetV2 gera um embedding de 1280 números
6. **Modelo:** MobileNetV2 congelada + GlobalAveragePooling + Dropout(0,3) + Dense(128, ReLU) + Dense(6, Softmax). Total de 2.422.726 parâmetros, dos quais 164.742 são treinados
7. **Treino:** Adam (lr 1e-3), perda Sparse Categorical Cross-Entropy, lotes de 32, até 15 épocas com early stopping, pesos de classe e augmentação. Rodou 8 épocas em cerca de 0,8 minuto na GPU
8. Avaliação no teste, comparação com a baseline, análise de acertos e erros
9. Limitações e questões éticas

## Resultados

Resultados no conjunto de teste (379 imagens):

| Modelo | Métrica principal | F1 macro | Acurácia |
|---|---|---|---|
| Baseline 1: classe mais frequente | F1 macro | 0,063 | 0,235 |
| Baseline 2: regressão logística (pixels 32x32) | F1 macro | 0,370 | 0,401 |
| **Rede neural (MobileNetV2)** | **F1 macro** | **0,797** | **0,831** |

Desempenho da rede por classe:

| Classe | Precisão | Recall | F1 | Imagens no teste |
|---|---|---|---|---|
| cardboard | 0,898 | 0,883 | 0,891 | 60 |
| glass | 0,773 | 0,895 | 0,829 | 76 |
| metal | 0,845 | 0,803 | 0,824 | 61 |
| paper | 0,907 | 0,876 | 0,891 | 89 |
| plastic | 0,812 | 0,778 | 0,794 | 72 |
| trash | 0,579 | 0,524 | 0,550 | 21 |

**Análise:** a rede superou com folga as duas baselines: o F1 macro foi mais que o dobro do da regressão logística. O modelo errou 64 das 379 imagens (16,9%). As confusões mais comuns foram metal classificado como vidro (9 vezes), plástico e vidro trocados entre si (7 vezes em cada sentido) e papel e papelão trocados entre si (5 vezes em cada sentido). Materiais brilhantes ou transparentes tendem a parecer iguais em foto, e papel e papelão têm cor e textura parecidas. A classe `trash` foi a mais fraca, o que provavelmente se deve ao número pequeno de exemplos (137 no dataset, 21 no teste). Como o teste é pequeno, os números têm margem de erro e podem variar alguns pontos com outra semente.

Na demonstração, uma foto fora do padrão do dataset (pintura sobre papel amassado) ficou dividida entre papel (49,7%) e papelão (43,2%), mostrando que o modelo não diz "não sei", apenas mostra probabilidades.

## Limitações

- **Dados:** dataset pequeno e desbalanceado, com fotos em condições controladas, diferentes de objetos sujos, amassados ou em uma lixeira real.
- **Modelo:** reconhece só 6 categorias, não tem opção "não sei" e confunde materiais parecidos e objetos feitos de vários materiais.
- **Vieses:** os objetos vêm de uma coleta específica e podem não representar os produtos brasileiros.
- **Riscos de uso:** uma classificação errada pode levar ao descarte incorreto e contaminar a reciclagem. A decisão final deve ser humana.
- **Privacidade:** fotos enviadas por usuários podem conter pessoas ou ambientes pessoais ao fundo.
- **Melhorias futuras:** mais dados (inclusive brasileiros e fotos reais de celular), fine-tuning, mais augmentação e uma classe "desconhecido".

O protótipo não é uma solução pronta para uso comercial.

## Como executar

1. Abra o notebook `Trabalho_Final_Lixo_Reciclavel.ipynb` no Google Colab (Arquivo > Fazer upload de notebook, ou abra pelo GitHub).
2. Ative a GPU: Ambiente de execução > Alterar tipo de ambiente de execução > GPU.
3. Execute as células em ordem. O dataset é baixado automaticamente.
4. Na última etapa, envie uma foto nova para ver a previsão.

## Declaração de uso de inteligência artificial

A estrutura inicial do notebook, do README e do roteiro da apresentação foi gerada com apoio do Claude (Anthropic). PREENCHER: descrever o que a equipe fez, revisou e alterou. Todos os integrantes compreendem e sabem explicar o código entregue.
