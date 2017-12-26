# Localizador de QR-Code

## Descrição do problema

Para a realização do exercício programa foram disponibilizadas 16 imagens, sendo que em cada uma aparece um único QR-code. Nessas imagens, os QR-codes aparecem aproximadamente em pé, com pequenas distorções em perspectiva.

Dado esse material inicial, o objetivo desse trabalho consiste em fazer um programa que lê uma imagem com QR-code e gera uma outra imagem que identifica a localização das três marcas quadradas localizadoras do QR-code. 

Para ler a descrição completa do problema, acesse: [Enunciado.pdf](https://github.com/matheusrmorgado/QRCode/blob/master/Enunciado.pdf).

## Operação

Para executar o programa, o usuário deve acessar pelo prompt de comando a pasta em que está localizado o arquivo **QRCode.cpp**. Caso ainda não tenha compilado, o usuário deve compilar digitando no prompt o seguinte comando: 

```
compila QRCode.cpp -cek
```

Agora compilado, o usuário deve ter na mesma pasta que está localizado o arquivo **QRCode.cpp**, o arquivo **padrao.png** que será utilizado no programa. 

Com isso garantido, o usuário deve digitar 3 argumentos para a execução correta do programa: 

* QRCode
* Nome da imagem de QR-code
* Nome do arquivo da imagem do QR-code com identificação que será gerada após a execução do programa

Para facilitar a visualização, os 3 valores impressos na imagem que auxiliam na identificação do QR-code (MatchLoc) são também disponibilizados no prompt de comando.

<p align="left">
  <img src="https://github.com/matheusrmorgado/QRCode/blob/master/images/operation.png">
</p>

## Técnicas utilizadas para resolução do problema

Nesse programa foi utilizada a biblioteca cekeikon.h para que fosse possível aplicar funções do Cekeikon e do OpenCV. Para fazer download dessa biblioteca é necessário acessar: http://www.lps.usp.br/hae/software/cekeikon5.html.

Para resolver o problema proposto, foram criadas 3 funções a fim de organizar o código e deixar a lógica de programação mais fácil de ser compreendida. A seguir, essas funções serão explicadas com mais detalhes.

Os primeiros comandos se referem à leitura de duas imagens, uma imagem que possui um QR-code e uma imagem padrão que será utilizada para localizar marcas quadradas.

<p align="center">
  <img src="https://github.com/matheusrmorgado/QRCode/blob/master/padrao.png">
</p>

Após a leitura, foi necessário encontrar a dimensão da imagem padrão que melhor localizasse as marcas quadradas na imagem original do QR-code. Para a realização desse procedimento foi criada a função **achaTamanho**.

### achaTamanho

Para encontrar a melhor dimensão, essa função redimensiona a imagem padrão, representada pela matriz *templ*, para várias escalas por meio da função do OpenCV *resize*.

Essa imagem redimensionada é então comparada com a imagem original do QR-code, representada pela matriz *image*, por meio da função do OpenCV *matchTemplate*.

<p align="center">
  <img src="https://github.com/matheusrmorgado/QRCode/blob/master/qr/qr00.jpg">
</p>

O resultado dessa comparação é armazenado na matriz *result*, que é analisada com a função do OpenCV *minMaxLoc* para verificar qual o seu maior valor. A localização do ponto que possui esse maior valor, representa o ponto mais claro da matriz *result*.

<p align="center">
  <img src="https://github.com/matheusrmorgado/QRCode/blob/master/images/result.png">
</p>

Após esse procedimento, a função **achaTamanho** retorna a dimensão da imagem padrão que obteve o maior valor na análise da matriz *result*. Com esse valor, foi possível construir a matriz *result* mais adequada para a continuação da resolução do problema. 

### localizaQR

O procedimento descrito abaixo é repetido 3 vezes para que seja possível localizar as 3 marcas quadradas da imagem do QR-code. Por meio da função do OpenCV *minMaxLoc*, encontra-se a posição do ponto de maior valor da matriz *result* gerada após o procedimento da função **achaTamanho**.

Esse ponto representa o vértice superior esquerdo do retângulo que é desenhado para identificação da marca quadrada do QR-code. Além disso, a localização desse ponto é também utilizada para desenhar linhas e textos que auxiliam na identificação do QR-code.

<p align="center">
  <img src="https://github.com/matheusrmorgado/QRCode/blob/master/images/QRCode-localized.png">
</p>

Após a localização do primeiro quadrado, é necessário encontrar as outras duas marcas quadradas. Para facilitar esse processo, a lógica utilizada foi pintar a vizinhança e o ponto localizado anteriormente de preto para evitar que a repetição do procedimento encontre um quadrado errado.

### pinta

A criação dessa função se baseia na função *pintaAzul* vista em aula e disponível na apostila: http://www.lps.usp.br/hae/apostila/compcon.pdf

A lógica dessa função é utilizar fila para processar componentes conexos a um ponto escolhido. Nesse caso, o ponto escolhido foi o de maior valor e o procedimento realizado foi de pintar de preto o próprio ponto e uma região próxima.

<p align="center">
  <img src="https://github.com/matheusrmorgado/QRCode/blob/master/images/result-pinta.png">
</p>

Com as 3 marcas quadradas localizadas, o problema proposto é então solucionado, uma imagem do QR-code com identificação é salva e encerra-se todas as instruções.

<p align="center">
  <img src="https://github.com/matheusrmorgado/QRCode/blob/master/lo/lo00.png">
</p>

## Referências

* Template Matching Tutorial: http://docs.opencv.org/2.4/doc/tutorials/imgproc/histograms/template_matching/template_matching.html#template-matching

* Finds the global minimum and maximum in an array: http://docs.opencv.org/2.4/modules/core/doc/operations_on_arrays.html#minmaxloc

* Resize an image: http://docs.opencv.org/2.4/modules/imgproc/doc/geometric_transformations.html#resize

* Apostila Template Matching: http://www.lps.usp.br/hae/apostila/tmatch-simp.pdf

## Direitos autorais

O trabalho aqui publicado fez uso da biblioteca Cekeikon5 para processamento de imagens e visão computacional.

Mais informações sobre essa biblioteca pode ser encontrado em: http://www.lps.usp.br/~hae/software
