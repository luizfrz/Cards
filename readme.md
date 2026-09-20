# Diamond — Cartas de Baralho

Mini dataset próprio de fotos de cartas de baralho (**Copas** × **Espadas**) e um pipeline de
pré-processamento de imagens em Python/OpenCV. O projeto simula as etapas iniciais de um projeto real
de visão computacional: construir o dataset, controlar a aquisição, decidir formato e resolução e
analisar o impacto técnico dessas escolhas **antes** de treinar qualquer modelo.

## Objetivos

- Construir um dataset próprio, documentando as condições de captura (iluminação e distância).
- Entender o impacto de resolução, profundidade de cor, espaço de cor e formato de arquivo.
- Aplicar filtragem espacial, remoção de ruído e detecção de bordas, justificando tecnicamente cada escolha.
- Desenvolver mentalidade de engenharia, não apenas execução técnica: em aplicações industriais,
  médicas ou científicas, essas decisões definem o sucesso ou o fracasso do sistema.

## Estrutura do repositório

```
Cards/
├── data/
│   ├── hearts/      # 5 fotos de Copas   (2, 4, 7, 8, A)
│   └── spades/      # 5 fotos de Espadas (5, 6, A, J, K)
├── notebook/
│   ├── settingData.ipynb      # Dataset, resolução, espaços de cor, quantização e formatos
│   └── filteringEdges.ipynb   # Suavização, ruído e detecção de bordas
├── requirements.txt
└── readme.md
```

## Dataset

| Item | Valor |
|---|---|
| Classes | Copas (*hearts*) e Espadas (*spades*) |
| Imagens | 10 (5 por classe) |
| Formato | JPEG, 4000 × 3000 px |
| Dispositivo | Samsung Galaxy A33 |
| Nomenclatura | `<valor>_hearts.jpeg` / `<valor>_spade.jpeg` |

As fotos foram tiradas de propósito em condições variadas — luz artificial, luz natural difusa,
contraluz, cena escura, close-up (~15 cm) e distância média (~50 cm) — para que seja possível analisar
como cada condição de aquisição afeta a qualidade da imagem. Os detalhes de cada foto estão em
`notebook/settingData.ipynb`.

## Notebooks

### 1. `settingData.ipynb` — Aquisição e representação da imagem

- **Resolução:** comparação entre a versão original e as reduzidas a 50% e 20%, com discussão da perda de detalhes finos.
- **Espaços de cor:** RGB, HSV e escala de cinza, e quando usar cada um.
- **Quantização:** versões com 256, 64, 32 e 2 níveis de cinza.
- **Formatos:** JPEG (com perdas) × PNG (sem perdas), comparando o tamanho dos arquivos.

### 2. `filteringEdges.ipynb` — Filtragem espacial e bordas

Trabalha com 4 imagens selecionadas por apresentarem ruído ou granulação (`a_hearts`, `4_hearts`,
`a_spade`, `6_spade`).

1. **Seleção das imagens:** degradação observada e hipótese sobre a origem do ruído.
2. **Suavização:** filtros da Média (3×3, 5×5), Gaussiano (σ = 1, 3) e Mediana (3×3, 5×5), avaliados
   por variância dos níveis de cinza e energia de borda (variância do Laplaciano).
3. **Detecção de bordas sem suavização:** Sobel, Prewitt (implementado manualmente) e Canny com dois
   pares de limiares, comparados pela densidade de bordas.
4. **Detecção de bordas após suavização:** o melhor filtro é escolhido automaticamente por um score
   que combina redução de ruído e preservação de bordas, e os operadores são reaplicados e comparados.
5. **Conclusão:** relatório consolidado do pipeline.

## Como executar

Requisitos: Python 3.9+.

```bash
git clone <url-do-repositorio>
cd Cards

python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -r requirements.txt

jupyter notebook notebook/
```

> Os notebooks usam caminhos relativos (`../data/...`) e precisam ser executados **a partir da pasta
> `notebook/`**.

No **Google Colab**, clone o repositório ou envie a pasta `data/` mantendo a mesma estrutura relativa
aos notebooks.

## Tecnologias

- [OpenCV](https://opencv.org/) (`opencv-python`) — leitura, conversões e filtros
- [NumPy](https://numpy.org/) — operações sobre matrizes
- [Matplotlib](https://matplotlib.org/) — visualização
- [Jupyter](https://jupyter.org/) — notebooks
