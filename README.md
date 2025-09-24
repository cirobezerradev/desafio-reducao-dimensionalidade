# Redução de Dimensionalidade em Imagens para Redes Neurais

### Descrição do Desafio
Seguindo o exemplo do algoritmo de binarização apresentado em nossa última aula, realize a implementação em Python para transformar uma imagem colorida para níveis de cinza (0 a 255) e para binarizada (0 e 255), preto e branco.  

 
Por meio da imagem é possível visualizar os dois casos esperados: 
<img width="602" height="219" alt="image" src="https://github.com/user-attachments/assets/18990430-a40e-41f2-b579-dd03e4719a82" />

Figura 1: Lena colorida (imagem de entrada), em níveis de cinza e preto e branca.

---
### Iniciando o Desafio:
> Primeiramente decidi realizar a redução de dimensionalidade a partir de uma imagem que baixei no google.
<img width="932" height="621" alt="imagem" src="https://github.com/user-attachments/assets/87409bdf-0cbb-4559-8a05-7982492e152f" />

Figura 2: Ciclista numa ciclovia (imagem tem originalmente largura 932px e altura 621px)

> Para desenvolver o desafio usei a plataforma do Google Colab https://colab.research.google.com/drive/18o2qXNf1PhRxzrhhdNiaVG0ULtIBK3Qb?usp=sharing

> E pesquisei pelo tutorial do OpenCV-Python https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html


### Passo 1: Importação das bibliotecas
```python
import cv2 as cv
from matplotlib import pyplot as plt
import sys
```

### Passo 2: Ler a imagem
> Para ler a imagem usa-se o método `imread` e o parâmetro com o path da imagem.
> Achei importante também verificar se foi possível ler a imagem, caso contrário encerra o processamento do sistema
```python
# Ler imagem usando a biblioteca opencv
img = cv.imread("images/image.png")

# Verifica se foi possivel ler a imagem caso contrário retorna a mensagem e encerra aplicação
if img is None:
    sys.exit("Não foi possível ler a imagem.")
```

### Passo 3: Tratar as imagens
> Primeiramente verifiquei que seria interessante redimensionar a imagem, nesse caso criei uma função para realizar esse redimencionamento de forma mais proporcional possível.
```python
# Criei uma função para reduzir o tamanho da imagem
def resize_image(new_width: int) -> None:
  factor = new_width / img.shape[1]
  new_height = int(img.shape[0] * factor)
  return new_width, new_height
```

> Em seguida redimensionei a imagem usando a função criada:
```python
# Uso da função para reduzir a imagem proporcionalmente para a largura 350px
img = cv.resize(img, resize_image(350)) # largura 350px
```

> E finalmente a parte do algoritmo que será responsável pela redução de dimensionalidade.
> Como decidi exibir as imagens usando matplotlib, foi necessário usar método `cv.cvtColor` com o parâmetro `cv.COLOR_BGR2RGB` para converter a imagem BGR para RGB. Pois o opencv carrega a imagem em BGR e o matplotlib espera uma imagem RGB, caso não faça a conversão a imagem original não fica na cor esperada. Como ilustrado abaixo:
<img width="350" height="233" alt="image" src="https://github.com/user-attachments/assets/1816a0c3-17be-4309-8cb4-f2c495b8d807" />

> E para a redução de dimencionalidade para escala de cinza usa também o método `cv.cvtCOLOR` convertendo a imagem em RBG para escala de cinza de 0 a 255.
> E para a binarização usa-se o método `cv.threshold` usando a imagem em escala de cinza, a faixa da escala de cinza,
> indicando também o metodo `cv.THRESH_BINARY + cv.THRESH_OTSU` para que o cálculo automatico do melhor threshold.
```python
# Para não haver alteração ao plotar a imagem pelo matplotlib, precisamos converter a imagem de BGR para RGB
img_rgb = cv.cvtColor(img, cv.COLOR_BGR2RGB)
# Reduz a dimensionalidade para escalas de cinza 0 a 255
img_gray = cv.cvtColor(img_rgb, cv.COLOR_RGB2GRAY)
# Reduz a dimensionalidade binarizando, sendo necessário usar o método OTSU para
# realizar cálculo automático do melhor threshold baseado no histograma da imagem.
_, img_bin = cv.threshold(img_gray, 0, 255, cv.THRESH_BINARY + cv.THRESH_OTSU)
```

### Passo 4: Exibição da imagem
```python
# Para otimizar a exibição, criei um dicionário com indice e valor
images = {"RBG": img_rgb, "Escala de Cinza": img_gray, "Binarizada": img_bin}

# Ajustar o tamanho para exibição
plt.figure(figsize=(12,8))

# Iteração do dicionário para colocar titulo adequado em cada imagem
for i, (title, image) in enumerate(images.items()):
    plt.subplot(1, 3, i + 1) # Determina a plotagem em uma linha e 3 colunas
    plt.title(title) # Determina a plotagem do titulo
    # Verifica se o title na iteração é um RGB 
    if title == "RGB":
      plt.imshow(image)
    else: # se não for RBG ele precisa determinar o cmap="gray", para garantir que apareçam em tons de cinza reais, não em cores estranhas.
      plt.imshow(image, cmap="gray")
    plt.axis("off") # para não plotar os eixos

plt.show()
```
<img width="1139" height="273" alt="image" src="https://github.com/user-attachments/assets/1e9e16e9-dfad-4698-8703-92f5515ab7aa" />

### Passo 5: Gravação das imagens
```python
cv.imwrite("images/image_rgb.png", img_rgb)
cv.imwrite("images/image_gray.png", img_gray)
cv.imwrite("images/image_bin.png", img_bin)
```

### Conclusão
> A partir de uma imagem obtida no Google, foi realizado um processo de redução da dimensionalidade utilizando o OpenCV, por meio da conversão de RGB para escala de cinza (RGB2Gray) e posterior binarização. Esse procedimento demonstrou de forma clara a eficiência da redução, preservando as principais características da imagem original com menor complexidade de representação, o que evidencia a utilidade dessas técnicas em tarefas de pré-processamento e análise de imagens.
<img width="648" height="143" alt="image" src="https://github.com/user-attachments/assets/2462a878-443e-452f-a41a-33574844bde1" />











