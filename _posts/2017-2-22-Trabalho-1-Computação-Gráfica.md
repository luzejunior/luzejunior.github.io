---
layout: post
title: Trabalho 1 - Brensehan Algorithm.
---

Trabalho de implementação em C++ do algoritmo de Brensehan utilizando OpenGL.

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

//Imagem do pixel sendo pintado no ponto superior.

A minha implementação do PutPixel toma como base que a origem do sistema de coordenadas é o centro da janela, portanto, temos que a posição do X deverá ser a metade da largura da tela somado com o valor X recebido como parâmetro pela função PutPixel. 
	pixel_na_metade_da_largura = x + (LARGURA_DA_JANELA/2);
Logo, se o usuário setar X=0, a posição em memória que o pixel será pintado será a partir da metade do tamanho da janela, como exemplificado na imagem a seguir:

//Imagem do pixel sendo pintado no centro da tela.

Tomando o mesmo pensamento para Y, temos que para que o pixel seja pintado na metade da tela do eixo Y, temos que subtrair da metade da altura da tela o valor de Y passado como parâmetro.


![_config.yml]({{ site.baseurl }}/images/config.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.