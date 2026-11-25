
# Classificador de Lixo Reciclável com Redes Neurais

Trabalho Final da disciplina de **Redes Neurais** | Turma: 6º Período | Data: 02/12/2026

## Integrantes

- Wlademir Pontes Xavier
- Celso Vinícius Souza
- João Gabriel da Silva Nemezio
- Daniel Paulino da Silva Neto

## Sobre o Projeto
Separar o lixo corretamente ainda é um gargalo grande na reciclagem. A triagem manual costuma ser lenta, cara e sujeita a erros. A ideia deste projeto é mostrar como modelos de Deep Learning conseguem reconhecer padrões visuais (forma, textura, transparência e cor) para classificar resíduos de forma automática.

Todo o desenvolvimento foi feito em Python dentro de um Jupyter Notebook (Trabalho_Final_Lixo_Reciclavel.ipynb), cobrindo desde a exploração e limpeza das imagens até o treinamento do modelo, ajustes para evitar overfitting e análise dos resultados.

## Divisão dos Dados

Treino  [ 70% ]: Usado efetivamente para o modelo aprender os padrões.

Validação [ 15% ]: Usado a cada época para acompanhar se o modelo está generalizando bem.

Teste [ 15% ]): Imagens totalmente inéditas para testar o desempenho real no final.
## Como executar

1. Abra o notebook `Trabalho_Final_Lixo_Reciclavel.ipynb` no Google Colab (Arquivo > Fazer upload de notebook, ou abra pelo GitHub).
2. Ative a GPU: Ambiente de execução > Alterar tipo de ambiente de execução > GPU.
3. Execute as células em ordem. O dataset é baixado automaticamente.
4. Na última etapa, envie uma foto nova para ver a previsão.

## Opção 2: Rodando Localmente

Clone o repositório:

git clone https://github.com/Wlademirpx/classificador-lixo-reciclavel.git
cd classificador-lixo-reciclavel


Crie e ative um ambiente virtual:

# Linux / macOS
python3 -m venv venv
source venv/bin/activate

# Windows (PowerShell)
python -m venv venv
.\venv\Scripts\Activate.ps1


Instale as dependências:

pip install --upgrade pip
pip install numpy pandas matplotlib seaborn scikit-learn opencv-python pillow tensorflow jupyter


Abra o notebook:

jupyter notebook Trabalho_Final_Lixo_Reciclavel.ipynb


## Avaliação e Resultados
Para medir a qualidade do classificador, foram analisadas quatro métricas principais:Acurácia: Percentual geral de acertos do modelo.Precisão : Quando o modelo diz que um item é "Plástico", quantas vezes ele realmente acertou? Recall: De todos os "Plásticos" que existiam no teste, quantos o modelo conseguiu encontrar?$F_1\text{-Score}$: Equilíbrio (média harmônica) entre Precisão e Revocação.

## Principais Observações

Curvas de Aprendizado: Os gráficos de Loss e Accuracy mostram a evolução do aprendizado ao longo das épocas, ajudando a confirmar que não houve overfitting severo.

Onde o modelo mais confunde: Na Matriz de Confusão, erros pontuais costumam acontecer entre materiais visualmente parecidos sob reflexo de luz, como plástico transparente e vidro, ou papel amassado e metal.

 ## Tecnologias Utilizadas

Linguagem: Python 3.9+

Deep Learning: TensorFlow / Keras (ou PyTorch)

Processamento de Imagens: OpenCV, Pillow

Manipulação de Dados e Métricas: NumPy, Pandas, Scikit-learn

Visualização: Matplotlib, Seaborn

Ambiente: Jupyter Notebook / Google Colab

## Declaração de uso de inteligência artificial

A estrutura inicial do notebook, do README e do roteiro da apresentação foi gerada com apoio do Claude (Anthropic). PREENCHER: descrever o que a equipe fez, revisou e alterou. Todos os integrantes compreendem e sabem explicar o código entregue.
