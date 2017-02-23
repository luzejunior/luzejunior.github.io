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


### DrawTriangle: