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

Com esses vetores, nós podemos criar a Matriz Rotação da camera com a função createMatrixWithVec3, passando os 3 vetores como parametro, e multiplicar pela matriz Translação para obtermos a Matriz View. Logo após, podemos multiplicar a Matriz view com a Matriz Model para obtermos a nossa matriz Model View do espaço da Câmera:

	glm::mat4 Bt = createMatrixWithVec3(x_camera, y_camera, z_camera);
	glm::mat4 T = translateMatrix(-camera_pos[0], -camera_pos[1], -camera_pos[2]);
	glm::mat4 M_View = T * Bt;

	glm::mat4 M_Model_View = M_Model * M_View;


### Espaço da Câmera → Espaço Projetivo ou de Recorte:

Com a nossa matriz Model View, para transformarmos o espaço para o espaço projetivo, temos que primeiro criar a nossa matriz de projeção, passando um parâmetro distancia que irá posteriormente alterar a coordenada homogênea dos vertices. Para criar a matriz projeção, iremos utilizar uma função chamada de criacreateM_Projection() passando como parametro o valor inteiro de d, que neste exemplo é igual a 1.

	int d = 1;
	glm::mat4 M_Projection = createM_Projection(d);

Após esta etapa, devemos multiplicar a Matriz Model_View do espaço da câmera, pela matriz projeção, para irmos para o Espaço Projetivo:

	glm::mat4 M_Model_View_Projection = M_Model_View * M_Projection;

Depois de criada a matriz view projection, devemos multiplicar ela pelos vetores de cada vértice do objeto, logo, com o loader do objeto, criamos 3 vec4 que terão o X, o Y, e o Z de cada ponto de cada triangulo do objeto. Após, isso, multiplicamos a matriz Model View Projection por cada vetor:

	glm::vec4 v1 = glm::vec4(objData->vertexList[o->vertex_index[0]]->e[0], // Primeira linha
  							objData->vertexList[o->vertex_index[0]]->e[1],
  							objData->vertexList[o->vertex_index[0]]->e[2], 
	 						1.0f);
  	glm::vec4 v2 = glm::vec4(objData->vertexList[o->vertex_index[1]]->e[0], // Segunda linha
  							objData->vertexList[o->vertex_index[1]]->e[1],
  							objData->vertexList[o->vertex_index[1]]->e[2], 
 							1.0f);
 	glm::vec4 v3 = glm::vec4(objData->vertexList[o->vertex_index[2]]->e[0], // Terceira linha
 							objData->vertexList[o->vertex_index[2]]->e[1],
 							objData->vertexList[o->vertex_index[2]]->e[2], 
 							1.0f);

  	//M_V_Projection to vertices
  	v1 = v1 * M_Model_View_Projection;
  	v2 = v2 * M_Model_View_Projection;
  	v3 = v3 * M_Model_View_Projection;

  	Logo, nós temos todos os vértices no espaço projetivo, ou de Recorte.

### Espaço de Recorte → Espaço “Canônico”:

Com os todos os vértices no espaço projetivo, para transformarmos para o espaço canônico, devemos dividir todos os valores de X, Y e Z, pelo valor do espaço homogêneo W, ou o ultimo valor do vetor. Logo,

	//Canonic Space Homog.
  	v1 = v1 / v1[3];
  	v2 = v2 / v2[3];
  	v3 = v3 / v3[3];

Agora nós temos o nossos vétices no espaço canônico, ou seja, todos os nossos vértices estão dentro de um hexaedro de visualização (Frustum), e oderão ser projetados na tela.

### Transformação: Espaço “Canônico” → Espaço de Tela:

A nossa ultima etapa é a transformação do espaço canônico para o espaço em tela, onde poderemos pegar as coordenadas de tela em que os pixels deverão ser pintados.
Para isso, nós devemos criar uma matriz de Tela, que é composta por três transformações:
* Inversão das coordenadas no eixo Y.
* Translação para X = 0.9f e Y = 0.8f.
* Escala com X=(largura da tela-1)*0.6f e Y=(altura da tela-1)*0.6f.

Logo, temos:

	//Create Screen Space:
	glm::mat4 M_Invert = glm::mat4(1.0f);
	M_Invert[1][1] = -1;

	glm::mat4 M_Translate = translateMatrix(0.9f, 0.8f, 0.0f);

	glm::mat4 M_Scale = glm::mat4(1.0f);
	M_Scale[0][0] = (ViewPortWidth-1) * 0.6f;
	M_Scale[1][1] = (ViewPortHeight-1) * 0.6f;

	glm::mat4 M_Screen = M_Invert * M_Translate * M_Scale;

Devemos então multiplicar todos os vetores do espaço canônico pela matriz de tela:

	//Espaço de tela:
  	v1 = v1 * M_Screen;
  	v2 = v2 * M_Screen;
  	v3 = v3 * M_Screen;

Com isso podemos utilizar a nossa função DrawTriangle, passando como parametro o X e o Y de cada vetor dos vértices.

	drawTriangle((int)v1[0], (int)v1[1], colorWhite, (int)v2[0], (int)v2[1], colorWhite, (int)v3[0], (int)v3[1], colorWhite);

### Resultado:



### Referências:

[BRESHENHAM’S ALGORITHM, Kenneth I. Joy. Visualization and Graphics Research Group, Department of Computer Science, University of California, Davis.](http://graphics.idav.ucdavis.edu/education/GraphicsNotes/Bresenhams-Algorithm.pdf)

[Bresenham's line algorithm article on Wikipedia.](https://en.wikipedia.org/wiki/Bresenham's_line_algorithm)

[Drawing Line Using Bresenham Algorithm](http://tech-algorithm.com/articles/drawing-line-using-bresenham-algorithm/)

[Bresenham's Line Drawing Algorithm in Computer Graphics - Part 1 What is Bresenham's Algorithm Youtube Video.](https://www.youtube.com/watch?v=5NV7HDI4xWk)

### GitHub do código.

Para baixar o código fonte deste trabalho, você pode utilizar o meu repositório no gitHub: [luzejunior/OpenGL-Graphic-Pipeline](https://github.com/luzejunior/OpenGL-Graphic-Pipeline)
