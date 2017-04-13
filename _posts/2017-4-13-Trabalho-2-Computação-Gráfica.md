---
layout: post
title: Trabalho 2 - Pipeline Gráfico.
---

Trabalho de implementação em C++ de um Pipeline gráfico completo utilizando OpenGL para carregar e desenhar objetos na tela.

### Introdução:

Nesta atividade da disciplina de Computação Gráfica da Universidade Federal da Paraiba que tem como tutor o Doutor Christian Azambuja Pagot, foi proposto que o aluno deveria implementar um pipeline completo levando em consideração todas as transformações envolvidas neste processo.

Estas transformações são:
* Transformação: Espaço do Objeto → Espaço do Universo.
* Transformação: Espaço do Universo → Espaço da Câmera.
* Transformação: Espaço da Câmera → Espaço Projetivo ou de Recorte.
* Transformação: Espaço de Recorte → Espaço “Canônico”.
* Transformação: Espaço “Canônico” → Espaço de Tela.
* Rasterização na tela.

### Implementação:

Neste meu modelo de implementação, foi criada uma classe auxiliar "Matrix" para facilitar algumas transformações e criar algumas matrizes. As das funções desta classe são:
* Criar Matriz de rotação em X e em Y.
* Criar Matriz de translação.
* Criar Matriz de projeção.
* Criar Matriz transposta com 3 vetores.

Além disso, nesta implementação também foi utilizada a biblioteca GLM para vetores e matrizes e algumas operações, como Norma e Cross.
Ao fazer esta implementação, eu tomei como base o arquivo Pipeline.h do octave, comparando os resultados das matrizes implementadas em C++ com o Octave.

### Espaço do Objeto → Espaço do Universo:

A Matriz model consiste nas transformações a serem feitas no Espaço do Objeto. Neste caso, foram feitas duas matrizes de Rotação: Uma em X e outra em Y, com valor de 30 graus cada. Depois deve-se multiplicar as matrizes de transformação para criarmos a Matriz_Model.

	glm::mat4 R1 = matrixRotationX(30.0f);
	glm::mat4 R2 = matrixRotationX(30.0f);

	glm::mat4 M_Model = R1 * R2;

Transformações no modelo do objeto devem ficar nesta matriz, logo, para fazer o objeto rotacionar a cada frame, eu modifiquei transformei a matriz R2 em global e apliquei a rotação da biblioteca GLM nesta matriz, logo:

	R2 = glm::rotate(R2, 0.005f, glm::vec3(0.0f, 1.0f, 0.0f));
	//R2 recebe uma matriz rotação com variância de 0.005f no eixo Y.

### Espaço do Universo → Espaço da Câmera:

Para o espaço da câmera, primeiro devemos criar os vetores de posicionamento da mesma, eles são: 
* Camera_Pos - Define a posição da camera no espaço.
* Camera_Look_At - Define o ponto para o qual a camera vai apontar.
* Camera_Look_Up - Define a posição up da camera.

Na minha implementação, eu segui o exemplo do octave, portanto a camera está no eixo Z = 3, olhando para a origem e com up no eixo Y, como pode ser visto abaixo:
	
	glm::vec3 camera_pos = glm::vec3(0.0f, 0.0f, 3.0f);
	glm::vec3 camera_look_at = glm::vec3(0.0f, 0.0f, 0.0f);
	glm::vec3 camera_look_up = glm::vec3(0.0f, 1.0f, 0.0f);

Para criarmos a matriz da camera, temos que definir os vetores X, Y e Z da camera, podemos ver a implementação no trecho de código abaixo:

	glm::vec3 z_camera = -(camera_look_at - camera_pos) / glm::l1Norm(camera_look_at - camera_pos);
	glm::vec3 x_camera = glm::cross(camera_look_up, z_camera) / glm::l1Norm(glm::cross(camera_look_up, z_camera));
	glm::vec3 y_camera = glm::cross(z_camera, x_camera);



### Referências:

[BRESHENHAM’S ALGORITHM, Kenneth I. Joy. Visualization and Graphics Research Group, Department of Computer Science, University of California, Davis.](http://graphics.idav.ucdavis.edu/education/GraphicsNotes/Bresenhams-Algorithm.pdf)

[Bresenham's line algorithm article on Wikipedia.](https://en.wikipedia.org/wiki/Bresenham's_line_algorithm)

[Drawing Line Using Bresenham Algorithm](http://tech-algorithm.com/articles/drawing-line-using-bresenham-algorithm/)

[Bresenham's Line Drawing Algorithm in Computer Graphics - Part 1 What is Bresenham's Algorithm Youtube Video.](https://www.youtube.com/watch?v=5NV7HDI4xWk)

### GitHub do código.

Para baixar o código fonte deste trabalho, você pode utilizar o meu repositório no gitHub: [luzejunior/OpenGL-Graphic-Pipeline](https://github.com/luzejunior/OpenGL-Graphic-Pipeline)
