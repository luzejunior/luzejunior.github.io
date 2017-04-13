---
layout: post
title: Trabalho 2 - Pipeline Gráfico.
---

Trabalho de implementação em C++ de um Pipeline gráfico completo utilizando OpenGL para carregar e desenhar objetos na tela.

### Introdução:

Nesta atividade da disciplina de Computação Gráfica da Universidade Federal da Paraiba que tem como tutor o Doutor Christian Azambuja Pagot, foi proposto que o aluno deveria implementar um pipeline completo levando em consideração todas as transformações envolvidas neste processo.

Estas transformações são:
* 1. Transformação: Espaço do Objeto → Espaço do Universo.
* 2. Transformação: Espaço do Universo → Espaço da Câmera.
* 3. Transformação: Espaço da Câmera → Espaço Projetivo ou de Recorte.
* 4. Transformação: Espaço de Recorte → Espaço “Canônico”.
* 5. Transformação: Espaço “Canônico” → Espaço de Tela.
* 6. Rasterização na tela.

### Implementação:

Neste meu modelo de implementação, foi criada uma classe auxiliar "Matrix" para facilitar algumas transformações e criar algumas matrizes. As das funções desta classe são:
* Criar Matriz de rotação em X e em Y.
* Criar Matriz de translação.
* Criar Matriz de projeção.
* Criar Matriz transposta com 3 vetores.
Além disso, nesta implementação também foi utilizada a biblioteca GLM para vetores e matrizes e algumas operações, como Norma e Cross.
Ao fazer esta implementação, eu tomei como base o arquivo Pipeline.h do octave, comparando os resultados das matrizes implementadas em C++ com o Octave.

### Matriz Model:



### Referências:

[BRESHENHAM’S ALGORITHM, Kenneth I. Joy. Visualization and Graphics Research Group, Department of Computer Science, University of California, Davis.](http://graphics.idav.ucdavis.edu/education/GraphicsNotes/Bresenhams-Algorithm.pdf)

[Bresenham's line algorithm article on Wikipedia.](https://en.wikipedia.org/wiki/Bresenham's_line_algorithm)

[Drawing Line Using Bresenham Algorithm](http://tech-algorithm.com/articles/drawing-line-using-bresenham-algorithm/)

[Bresenham's Line Drawing Algorithm in Computer Graphics - Part 1 What is Bresenham's Algorithm Youtube Video.](https://www.youtube.com/watch?v=5NV7HDI4xWk)

### GitHub do código.

Para baixar o código fonte deste trabalho, você pode utilizar o meu repositório no gitHub: [luzejunior/OpenGL-Graphic-Pipeline](https://github.com/luzejunior/OpenGL-Graphic-Pipeline)
