---
layout: post
title: Trabalho 1 - Brensenham Algorithm.
---

Trabalho de implementação em C++ do algoritmo de Brensenham utilizando OpenGL.

### Introdução:

Nesta atividade da disciplina de Computação Gráfica da Universidade Federal da Paraiba que tem como tutor o Doutor Christian Azambuja Pagot, foi proposto que o aluno deveria escrever três funções em OpenGL utilizando o framework de criação de janelas desenvolvido pelo professor.

As funções principais são: 
* PutPixel: Que tem como objetivo principal pintar um pixel dentro da janela do OpenGL.
* DrawLine: Que tem como objetivo desenhar uma linha na janela utilizando o algoritmo de Brensenhan generalizando para todos os octantes.
* DrawTriangle: Que tem como objetivo desenhar um triangulo utilizando a função DrawLine.

OBS: Nas funções DrawLine e DrawTriangle, as linhas tem que conter interpolação linear das cores.

### PutPixel:

Para entendermos como essa implementação foi feita, nós temos primeiro que entender como funciona o array da memória de video.

Como visto em sala de aula, temos que todos os pixels da tela são uma sequencia de elementos RGBA, ou seja, cada pixel da tela é um array de 4 elementos que contém os valores de R, G, B e A que são Vermelhos, Verdes, Azuis, e Transparencia.

A função PutPixel recebe como parametro as coordenadas X e Y em que o ponto deverá ser pintado além da cor a ser pintada. Tomando isso como base, podemos dizer que o X corresponde a coluna e o Y a linha em que o pixel vai aparecer.

Os elementos são dispostos da seguinte maneira na memória de video: {R}{G}{B}{A}{R}{G}{B}{A} -> Array Linear com 2 pixels.

No OpenGL, temos que os elementos 0 até 3 do array acima são o primeiro pixel na janela, ou seja, se caso queiramos pintar o ponto X=0 e Y=0, o pixel pintado será o primeiro ponto do lado esquerdo superior da janela.

![Pixel na parte Superior esquerda da janela]({{ site.baseurl }}/images/PixelPonto0.png)

A minha implementação do PutPixel toma como base que a origem do sistema de coordenadas é o centro da janela, portanto, temos que a posição do X deverá ser a metade da largura da tela somado com o valor X recebido como parâmetro pela função PutPixel:

	pixel_na_metade_da_largura = x + (LARGURA_DA_JANELA/2);

Logo, se o usuário setar X=0, a posição em memória que o pixel será pintado será a partir da metade do tamanho da janela, como exemplificado na imagem a seguir:

![Pixel na metade da largura da tela]({{ site.baseurl }}/images/pixel0x.png)

Tomando o mesmo pensamento para Y, temos que para que o pixel seja pintado na metade da tela do eixo Y, temos que subtrair da metade da altura da tela o valor de Y passado como parâmetro.

	pixel_na_metade_da_altura = (ALTURA_DA_JANELA/2) - y;

![Pixel na metade da altura da tela]({{ site.baseurl }}/images/pixel0Y.png)

Portanto, juntando os dois, para pintarmos o pixel no centro da janela, temos que: 

	pixel_no_centro_da_janela = x + (LARGURA_DA_JANELA/2) + ((ALTURA_DA_JANELA/2) - y);

![Pixel na metade da tela]({{ site.baseurl }}/images/pixel0X0Y.png)

Como o nosso array da memoria de video é linear, temos que a posição REAL no array, o elemento Y deverá ser multiplicado pela quantidade de colunas da imagem, e tudo isso deverá ser multiplicado pela quantidade de elementos do pixel: Neste caso, 4 elemento (RGBA).

Logo, o nosso codigo final ficará:

	void putPixel(int x, int y, int RGBA[4]){
		int print_position = ((x+(IMAGE_WIDTH/2)) + (((IMAGE_HEIGHT/2)-y) * IMAGE_WIDTH)) * 4; //Posição do Array da memória onde o Pixel vai ser pintado.
		//Definição do array dos pixels na tela.
		for(int i = 0; i<4; i++){
			FBptr[print_position+i] = RGBA[i];
		}
	}

### DrawLine:

Para desenharmos uma linha na tela, nós temos que saber quais pixels nós temos que pintar na janela. A função drawLinr() recebe como paramêtro um X0, um Y0 e uma cor para essa coordenada, e um X1, um Y1 e uma cor para essa coordenada. O que essa função faz é pintar vários pixels da coordenada (X0,Y0), até a coordenada (X1, Y1). Para isso, nós utilizamos o chamado Algoritmo de Brensenham, que é um algoritmo utilizado para desenhar linhas em paineis digitais (ou telas de LCD que utilizam Pixels).

Abaixo temos um exemplo simplificado de um código de Brensenham para o primeiro octante:

	int deltaX = x1 - x0; //Valor de DeltaX
	int deltaY = y1 - y0; //Valor de DeltaY
	int j = y0; //Inicializando variável J, que corresponde ao Y do plano cartesiano.
	int maxNumber = deltaX; //Numero máximo de pixels que vai ser pintado.
	int error = (maxNumber)/2;
	int i = x0;
	for(int k=0; k<=maxNumber; k++){	
		putPixel(i, j, RGBA);
		error += deltaY;
		if(error >= deltaX){
			j += 1;
			error -= deltaX;
		}else{
			i += 1;
		}	
	}

O problema é que o algoritmo só funciona para o primeiro octante de uma esfera. Então existem adaptações do código para que ele funcione de forma geral, ou seja, em todos os octantes.

Nesta implementação, foram utilizadas 4 váriaveis de controle para incrementar os valores das coordenadas de X e Y a serem pintadas na tela. Todas elas foram inicializadas com 0, e são modificadas para 1 ou -1 caso seja necessário.

	int incrementX = 0; //Variável que controla o incremento do X no plano cartesiano.
	int incrementX1 = 0; //Variável auxiliar que controla o incremento do X no plano cartesiano.
	int incrementY = 0; //Variável que controla o incremento do Y no plano cartesiano.
	int incrementY1 = 0; //Variável auxiliar que controla o incremento do Y no plano cartesiano.

De maneira geral, para modificar essas variáveis de controle, foram criadas algumas regras para elas, são elas:
* Caso DeltaX seja menor que 0, inverte-se o sinal de DeltaX e faz com que as variáveis incrementX e incrementX1 sejam igual a -1.
* Caso DeltaX seja maior que 0, faz com que as variáveis incrementX e incrementX1 sejam igual a -1.
* Caso DeltaY seja menor que 0, inverte-se o sinal de DeltaY e faz com que a variável incrementY seja igual a -1.
* Caso DeltaY seja maior que 0, faz com que a variável incrementY seja igual a 1.
* Caso DeltaY seja maior que Delta X, inverte-se os deltas (DeltaY passa a ter valor de DeltaX e vice-versa) e:
	* Testa se o incrementoY é menor que 0, pois caso essa condição seja verdadeira, isso quer dizer que DeltaY era menor do que 0, logo, DeltaX será igual a -DeltaX.
	* Caso DeltaX seja menor que 0, inverte-se o sinal de DeltaX e faz com que a variável incrementY1 seja igual a -1.
	* Caso DeltaX seja maior que 0, faz com que a variável incrementY1 seja igual a 1.
	* Faz com que a variável incrementX1 seja igual a 0.

Um exemplo de implementação dessas regras pode ser visto a seguir:

	if (deltaX < 0){
		deltaX = -deltaX;
		incrementX = -1;
		incrementX1 = -1;
	}else if(deltaX > 0){
		incrementX = 1;
		incrementX1 = 1;
	}
	if(deltaY < 0){
		deltaY = -deltaY;
		incrementY = -1;
	}else if(deltaY > 0){
		incrementY = 1;
	}
	if(deltaY > deltaX){
		int tmp1 = deltaX;
		deltaX = deltaY;
		deltaY = tmp1;
		if (incrementY<0){
			deltaX = -deltaX;
		}
		if(deltaX<0){
			incrementY1 = -1;
			deltaX = -deltaX;
		}
		else if(deltaX>0){
			incrementY1 = 1;
		}
		incrementX1 = 0;
	}

Com o uso dessas variáveis de incremento, temos agora que adaptar o nosso algoritmo. Logo, como o algoritmo funciona calculando o "erro" para saber em qual coordenada X e Y devemos pintar na janela:
* O algoritmo utiliza o maior delta para saber quantos pixels serão pintados na tela.
* Primeiro pinta-se o ponto na primeira Coordenada X, e Y passada por parâmetro.
* Incrementa o erro com deltaY.
* Se o erro for menor ou igual ao DeltaX, deve-se incrementar X com o valor de incrementX, Y com o valor de incrementY e deve-se decrementar do erro o valor de DeltaX.
* Se o erro for maior que DeltaX, deve-se incrementar X com o valor de incrementX1 e Y com o valor de incrementY1.
* Repetir o procedimento para todos os pixels que devem ser pintados na tela.

Um exemplo do algoritmo com os incrementos:

	int maior_Distancia = deltaX; //Numero máximo de pixels que vai ser pintado.
	if(deltaX == 0){
		maior_Distancia = deltaY;
	}
	int i = x0;
	int j = y0;
	for(int k=0; k<=maior_Distancia; k++){
		putPixel(i, j, RGBA2);
		error += deltaY;
		if(error >= deltaX){
			j += incrementY;
			i += incrementX;
			error -= deltaX;
		}else{
			j += incrementY1;
			i += incrementX1;
		}	
	}

##### Para o primeiro e quinto Octante:

Digamos que queremos desenhar uma linha no primeiro octante, iremos utilizar a seguinte chamada da função drawLine:

	//drawLine recebe como parâmetro: x0, y0, primeira cor, x1, y1, segunda cor.
	drawLine(0,0, colorRed, 200, 100, colorGreen); //1 octante

Temos neste caso que:
* DeltaX é maior que 0, logo:
	* incrementX = 1;
	* incrementX1 = 1;
* DeltaY também é maior que 0, logo:
	* incrementY = 1;
	* incrementY1 = 0;

Com isso já definido, temos que, para cada vez que o erro for menor que deltaX, só devemos incrementar a coordenada X, enquanto que quando o "erro" é maior ou igual a DeltaX, devemos incrementar as coordenadas X e Y.

![Exemplo de linha no primeiro octante]({{ site.baseurl }}/images/octant1.png)

O quinto octante é o inverso do primeiro octante, ou seja, DeltaX e DeltaY são menores que 0, logo:

	//drawLine recebe como parâmetro: x0, y0, primeira cor, x1, y1, segunda cor.
	drawLine(0,0, colorRed, -200, -100, colorGreen); //5 octante

Portanto temos que:
* incrementX = -1;
* incrementX1 = -1;
* incrementY = -1;
* incrementY1 = 0;

Com isso já definido, temos que, para cada vez que o erro for menor que deltaX, só devemos decrementar a coordenada X, enquanto que quando o "erro" é maior ou igual a DeltaX, devemos decrementar as coordenadas X e Y.

![Exemplo de linha no primeiro octante]({{ site.baseurl }}/images/octant5.png)

##### Para o segundo e sexto Octante:

Digamos que queremos desenhar uma linha no segundo octante, iremos utilizar a seguinte chamada da função drawLine:

	//drawLine recebe como parâmetro: x0, y0, primeira cor, x1, y1, segunda cor.
	drawLine(0,0, colorRed, 100, 200, colorGreen); //2 octante

Temos neste caso que:
* DeltaX é maior que 0, logo:
	* incrementX = 1;
	* incrementX1 = 1;
* DeltaY também é maior que 0, logo:
	* incrementY = 1;
	* incrementY1 = 0;

O problema neste caso, é que DeltaY é maior que deltaX, ou seja, deve-se inverter essas coordenadas e:
* Tornar o incrementY1 = 1;
* Tornar o incrementX1 = 0;

Com isso, temos que: 
* incrementX = 1;
* incrementX1 = 0;
* incrementY = 1;
* incrementY1 = 1;

Por isso, podemos então dizer que para cada vez que o erro for menor que deltaX, devemos incrementar apenas a coordenada Y, enquanto que quando o "erro" é maior ou igual a DeltaX, devemos incrementar as coordenadas X e Y.

![Exemplo de linha no segundo octante]({{ site.baseurl }}/images/octant2.png)

O sexto octante é o inverso do segundo octante, ou seja, DeltaX e DeltaY são menores que 0, logo:

	//drawLine recebe como parâmetro: x0, y0, primeira cor, x1, y1, segunda cor.
	drawLine(0,0, colorRed, -100, -200, colorGreen); //6 octante

Portanto temos que:
* incrementX = -1;
* incrementX1 = 0;
* incrementY = -1;
* incrementY1 = -1;

Com isso já definido, temos que, para cada vez que o erro for menor que deltaX, devemos decrementar apenas a coordenada Y, enquanto que quando o "erro" é maior ou igual a DeltaX, devemos decrementar as coordenadas X e Y.

![Exemplo de linha no sexto octante]({{ site.baseurl }}/images/octant6.png)

##### Para o terceiro e sétimo Octante:

Digamos que queremos desenhar uma linha no terceiro octante, iremos utilizar a seguinte chamada da função drawLine:

	//drawLine recebe como parâmetro: x0, y0, primeira cor, x1, y1, segunda cor.
	drawLine(0,0, colorRed, -100, 200, colorGreen); //3 octante

Temos neste caso que:
* DeltaX é menor que 0, logo:
	* incrementX = -1;
	* incrementX1 = -1;
* DeltaY é maior que 0, logo:
	* incrementY = 1;
	* incrementY1 = 0;

Neste caso, é DeltaY também é maior que deltaX, ou seja, deve-se inverter essas coordenadas e:
* Tornar o incrementY1 = 1;
* Tornar o incrementX1 = 0;

Com isso, temos que: 
* incrementX = -1;
* incrementX1 = 0;
* incrementY = 1;
* incrementY1 = 1;

Por isso, podemos então dizer que para cada vez que o erro for menor que deltaX, devemos incrementar apenas a coordenada Y, enquanto que quando o "erro" é maior ou igual a DeltaX, devemos decrementar a coordenada X e incrementar a coordenada Y.

![Exemplo de linha no terceiro octante]({{ site.baseurl }}/images/octant3.png)

O sétimo octante é o inverso do terceiro octante, ou seja, DeltaX é maior que 0 e DeltaY é menores que 0, logo:

	//drawLine recebe como parâmetro: x0, y0, primeira cor, x1, y1, segunda cor.
	drawLine(0,0, colorRed, 100, -200, colorGreen); //7 octante

Portanto temos que:
* incrementX = 1;
* incrementX1 = 0;
* incrementY = -1;
* incrementY1 = -1;

Com isso já definido, temos que, para cada vez que o erro for menor que deltaX, devemos decrementar apenas a coordenada Y, enquanto que quando o "erro" é maior ou igual a DeltaX, devemos incrementar a coordenada X e decrementar a coordenada Y.

![Exemplo de linha no sétimo octante]({{ site.baseurl }}/images/octant7.png)

##### Para o quarto e oitavo Octante:

Digamos que queremos desenhar uma linha no quarto octante, iremos utilizar a seguinte chamada da função drawLine:

	//drawLine recebe como parâmetro: x0, y0, primeira cor, x1, y1, segunda cor.
	drawLine(0,0, colorRed, -200, 100, colorGreen); //3 octante

Temos neste caso que:
* DeltaX é menor que 0, logo:
	* incrementX = -1;
	* incrementX1 = -1;
* DeltaY é maior que 0, logo:
	* incrementY = 1;
	* incrementY1 = 0;

Por isso, podemos então dizer que para cada vez que o erro for menor que deltaX, devemos decrementar apenas a coordenada X, enquanto que quando o "erro" é maior ou igual a DeltaX, devemos decrementar a coordenada X e incrementar a coordenada Y.

![Exemplo de linha no quarto octante]({{ site.baseurl }}/images/octant4.png)

O oitavo octante é o inverso do quarto octante, ou seja, DeltaX é maior que 0 e DeltaY é menor que 0, logo:

	//drawLine recebe como parâmetro: x0, y0, primeira cor, x1, y1, segunda cor.
	drawLine(0,0, colorRed, 200, -100, colorGreen); //8 octante

Portanto temos que:
* incrementX = 1;
* incrementX1 = 1;
* incrementY = -1;
* incrementY1 = 0;

Com isso já definido, temos que, para cada vez que o erro for menor que deltaX, devemos incrementar apenas a coordenada X, enquanto que quando o "erro" é maior ou igual a DeltaX, devemos incrementar a coordenada X e decrementar a coordenada Y.

![Exemplo de linha no oitavo octante]({{ site.baseurl }}/images/octant8.png)

Juntando todos os octantes, podemos desenhar na tela a seguinte figura:

![Exemplo de todos os octantes]({{ site.baseurl }}/images/alloctants.png)

##### Interpolação de Cores.

Para interpolar as cores das linhas, devemos primeiro pegar o numero de cores para cada RGB do pixel e dividir pela maior distancia dos pixels:

	colorGradientRatio = 256/maior_Distancia;

Depois deveremos criar um RGBA2 que deverá ser igual ao RGBA do ponto (X0, Y0).

Então, com um aninho de ifs, devemos pegar a primeira Cor RGBA do ponto (X0, Y0) e verificar qual das cores é maior que 0, então para cada interação do laço for, devemos decrementar do valor de RGBA2 correspondente a cor maior que 0 o valor de colorGradientRatio.

De modo semelhante, com a segunda cor RGBA do ponto (X1, Y1), devemos verificar qual a cor que é maior que 0, e para cada interação do laço for, devemos incrementar do valor de RGBA2 correspondente a cor maior que 0 o valor de colorGradientRatio.

Um exemplo está descrito a seguir:

	if(RGBA0[0] > 0 && RGBA2[0] <= 255){
		RGBA2[0] -= colorGradientRatio;
	}
	else if(RGBA0[1] > 0 && RGBA2[1] <= 255){
		RGBA2[1] -= colorGradientRatio;
	}
	else if(RGBA0[2] > 0 && RGBA2[2] <= 255){
		RGBA2[2] -= colorGradientRatio;
	}
	if(RGBA1[0] > 0 && RGBA2[0] <= 255){
		RGBA2[0] += colorGradientRatio;
	}
	if(RGBA1[1] > 0 && RGBA2[1] <= 255){
		RGBA2[1] += colorGradientRatio;
	}
	if(RGBA1[2] > 0 && RGBA2[2] <= 255){
		RGBA2[2] += colorGradientRatio;
	}

Então devemos sempre pintar o pixel com o resultado do RGBA2 criado, fazendo assim com que as cores sejam interpoladas.

### DrawTriangle:

A função drawTriangle tem como objetivo pegar três coordenadas (X0, Y0), (X1, Y1), (X2, Y2) e formar um triângulo com elas.

Essa função tem uma implementação muito simples, pois pode-se utilizar a função já feita de desenhar linhas para todos os octantes. Logo, para que o triangulo esteja correto, devemos criar três chamadas da função drawLine como descrito a seguir:

	drawLine(x0, y0, RGBA0, x1, y1, RGBA1);
	drawLine(x0, y0, RGBA0, x2, y2, RGBA2);
	drawLine(x1, y1, RGBA1, x2, y2, RGBA2);

Para testar a nossa função, iremos definir uma chamada para drawTriangle como descrita a seguir:

	drawTriangle(-100, 0, colorRed, -100, 90, colorGreen, 100, 0, colorBlue);

O triangulo a seguir deve aparecer na tela:

![Exemplo da função drawTriangle]({{ site.baseurl }}/images/drawTriangle.png)

### Resultados, Melhoras e Dificuldades:

Com essa implementação do algoritmo de Brensenham o usuário é capaz de desenhar linhas em qualquer um dos octantes da janela do OpenGL apenas passando as coordenadas (X0, Y0) e (X1, Y1) desejadas. Também é possivel desenhar um trinangulo definindo na função drawTriangle as coordenadas dos pontos (X0, Y0), (X1, Y1) e (X2, Y2). Todos os testes em todos os octantes foram feitos, a fim de testar a eficiencia do código como exemplificado na seção DrawLines.

Para melhorar essa implementação, deveria ser feito uma classe em C++ para tratar exclusivamente do pixel, com metodos de Get e Set das coordenadas. Assim, o código ficaria mais enxuto e mais organizado, pois você definiria apenas o pixel e as funções drawLine e drawTriangle receberiam esses pixels como parâmetro. Outra melhoria seria utilizar uma classe ou uma função para fazer a interpolação das cores, deixando o código mais organizado.

As dificuldades que eu tive foram para entender o funcionamento do Brensenham geral para todos os octantes, foram feitos muitos calculos no caderno para entender qual incremento e qual decremento deveria utilizar e porquê. Alguns materiais foram muito importantes para ajudar a entender o algoritmo - Eles estarão descritos na seção de referencias. 

### Referencias:

*[BRESHENHAM’S ALGORITHM, Kenneth I. Joy. Visualization and Graphics Research Group, Department of Computer Science, University of California, Davis.](http://graphics.idav.ucdavis.edu/education/GraphicsNotes/Bresenhams-Algorithm.pdf)
*[Bresenham's line algorithm article on Wikipedia.](https://en.wikipedia.org/wiki/Bresenham's_line_algorithm)
*[Drawing Line Using Bresenham Algorithm](http://tech-algorithm.com/articles/drawing-line-using-bresenham-algorithm/)
*[Bresenham's Line Drawing Algorithm in Computer Graphics - Part 1 What is Bresenham's Algorithm Youtube Video.](https://www.youtube.com/watch?v=5NV7HDI4xWk)
