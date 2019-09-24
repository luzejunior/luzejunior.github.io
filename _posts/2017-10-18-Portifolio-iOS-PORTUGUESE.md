---
layout: post
title: PORTUGUESE - Portfolio de aplicativos iOS.
---

Este é o meu portfólio de Aplicações iOS que eu desenvolvi. Aqui você vai encontrar todos os projetos que eu participei, e o meu objetivo é atualizar essa página sempre que possível!

## Minha História como desenvolvedor iOS:

Olá, caso você ainda não me conheça, você pode saber um pouco sobre minha pessoa na sessão ["About"](https://luzejunior.github.io/about/) deste site.
Mas neste portfólio eu vou falar um pouco de como comecei a programar, e mais que isso, aprendi a gostar desta linguagem de programação chamada Swift e tudo que envolve desenvolvimento mobile para iOS.

Desde criança sempre fui curioso para saber como as coisas funcionavam, e no começo da era dos Smartphones eu sempre me perguntava como que funcionava a programação para dispositivos móveis. Então comecei a estudar por conta própria sobre Programação Android e iOS, mas infelizmente por não ter um iDevice, não tive muita oportunidade de me aprofundar tanto quanto gostaria no mundo do desenvolvimento Apple.

No fim de 2015, enquanto estudava na Universidade Federal da Paraíba, e trabalhava no LAViD (Laboratório de Aplicações de Video Digital), comecei a trabalhar em um projeto chamado ["Culturi"](http://www.culturi.com.br/), que era uma rede social de eventos em parceria com o Ministério da Cultura. Então, participando do projeto, fui escalado para fazer um aplicativo para iOS complementar ao Culturi completamente do zero, em no máximo dois meses. E é ai que começa a minha aventura de aprender a programar em uma linguagem relativamente simples, mas com uma arquitetura bem complexa em menos de um mês, para desenvolver o mais rápido possível um aplicativo em dois meses, utilizando um Macbook Pro late 2011 do meu Orientador na Universidade. O projeto saiu, e se chamava ["Meu olhar"](http://meuolhar.culturi.com.br/). Mas, infelizmente ele foi interrompido pelos novos ministros da cultura no final de 2016.

Como tomei gosto pelo desenvolvimento mobile, resolvi que continuaria nesta área, e ai, no começo de 2017 veio o segundo projeto. Dessa vez em parceria com o Ministério do Desenvolvimento Social. Desenvolvi um aplicativo do "Bolsa Família" para os beneficiários ficarem por dentro das noticias, novidades, datas de pagamento, etc.

Como freelancer, participei do desenvolvimento do aplicativo "Schoolastic", que é uma plataforma de gerenciamento de alunos, onde os pais podem ter total controle das atividades do seu filho na escola. Também desenvolvi o aplicativo do Wiit.live, uma plataforma de venda e transmissão de videos online, onde pude inclusive desenvolver um player de video totalmente em Swift. Converti também um template de app comprado em um site de venda de templates para um modelo de app onde os usuários poderiam postar vagas de empregos totalmente funcional. 

Atualmente trabalho como desenvolvedor iOS na Invillia numa equipe que trabalha para o PagSeguro (PagBank), e cuido da parte de Membership, desenvolvendo a parte mobile iOS da sessão "Indique e Ganhe" do aplicativo "PagBank".

Utilizo um Macbook Pro 13 2017 para programar, e disponho de 4 dispositivos para teste, sendo eles: iPhone 7 (iOS 12), iPhone 6 (iOS 12), iPhone XR (iOS 12) e dois iPad Mini 2 (um com iOS 9.3.5 e outro com iOS 8.4). Como sou Apple Entusiasta, possuo outros dispositivos como iPods e iPhones antigos, mas infelizmente eles não servem para teste, e são apenas itens de coleção.

Abaixo você pode ver alguns exemplos dos apps que eu desenvolvi:

## Meu Olhar:

 O "Meu olhar" é um aplicativo baseado no Instagram (quase uma cópia, só que diferente). Nele você não tira fotos diretamente dele e manda para o servidor. Na verdade todas as fotos que você tirar em uma rede social e marcar uma Hashtag previamente definida pelo administrador, vai parar nas galerias do "Meu olhar". Ele trabalha com o uma thread em PHP que busca por novas imagens de determinadas hashtags em todas as redes sociais e coloca no banco de dados da aplicação. Eu não lembro de detalhes da implementação do Back-End (Não fui eu que fiz), mas todas as informações são adquiridas através de REST com JSon. Basicamente utilizava-mos o AFNetworking para fazer requisições (GET) ao servidor e conseguir todas as hashtags cadastradas e fotos do banco de dados.

 O aplicativo utilizava as Api's do Facebook, Twitter, Instagram e Youtube para cadastro e login, além de ter um sistema de token próprio para usuários poderem curtir e compartilhar fotos direto da aplicação. Com esse sistema de token, fizemos uma versão para pessoas não cadastradas poderem apenas ver as fotos que estavam no aplicativo.

 A ideia é que este aplicativo fosse utilizado nas Olimpíadas do Rio 2015 para promover concursos culturais do governo, com premiação e tudo mais. Mas devido aos problemas que tivemos com a nossa antiga presidente, o projeto foi descontinuado na gestão do novo encarregado do cargo de gerência do nosso país.

#### Screenshots:

Estou procurando screenshots deste aplicativo. Como ele foi desativado da AppStore, eu tenho screens dele guardado em algum pen drive. O problema é que eu comprei um Macbook Pro 2017, e ele não tem entrada USB (Obrigado Apple). Pra resolver este problema, comprei um Dongle no Aliexpress... que infelizmente queimou com uma semana de uso (ninguém me avisou que não podia utilizar o carregador de 61W do Mac direto nele). Logo que conseguir passar todas as fotos pro meu mac, colocarei aqui.

## Bolsa Familia:

Este foi o meu segundo aplicativo nativo desenvolvido, desta vez em Swift 3. Este era em parceria com o Ministério do Desenvolvimento Social e Agrário (antigo MDS). A intensão é que fosse um aplicativo simples e intuitivo para que os beneficiários do Bolsa família pudessem utilizá-lo para obter informações sobre o seu cadastro no programa.

Este aplicativo era pra usar o mínimo de internet possível e persistir todos os dados diretamente na aplicação. Foram utilizadas a api do Google Maps para obter os "CRAIS" mais próximos da localização do beneficiário e Alamofire para obter eventuais dados que seriam enviados pelo servidor cada vez que o usuário conseguisse uma pontinha de internet. O Main Device Target era o iPhone 4S e o iOS 8.4, que eram os mais simples suportados pelo Swift 3.

Mais uma vez, para o meu azar, o ministro apesar de gostar da aplicação, decidiu que a parte móvel não era tão interessante assim, e simplesmente cancelou o projeto de aplicação móvel, ficando apenas com a versão para TV Digital, o que, obviamente me deixou no olho da rua. Infelizmente por isso, o aplicativo não chegou nem a ser publicado na AppStore. Mas com orgulho é um dos apps mais bonitos, responsivos e bem programados que eu fiz.

#### Screenshots:

| ![SplashScreen]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Splash.png) |
|:--:|
| *SplashScreen* |

| ![MainMenu]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-MainOff.png) |
|:--:|
| *Main Menu com usuário deslogado.* |

| ![Login]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-LoginScreen.png) |
|:--:|
| *Xib de Login* |

| ![MainMenu]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-MainOn.png) |
|:--:|
| *Main Menu com usuário Logado.* |

| ![Calendário]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Calendario.png) |
|:--:|
| *Calendário de Informações.* |

| ![Maps]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Maps.png) |
|:--:|
| *Maps com integração com API do google.* |

| ![Programas]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Programas.png)![Criança_Feliz]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-CFeliz.png) |
|:--:|
| *Tela com os principais programas do governo.* |

| ![Sobre]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Sobre.png) |
|:--:|
| *Tela de Sobre* |

| ![Social]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Social.png) |
|:--:|
| *Tela de Social* |

#### Código fonte:

Você pode encontrar o código fonte deste aplicativo no meu GitHub: [Repositório MDS-BolsaFamilia](https://github.com/luzejunior/MDS-BolsaFamilia)

## Wiit.Live:

| ![Main]({{ site.baseurl }}/images/WiitLive/Screen Shot 2018-10-03 at 10.40.20.png) |
|:--:|
| *Tela principal do app* |

| ![Chat]({{ site.baseurl }}/images/WiitLive/Screen Shot 2018-10-03 at 10.46.57.png) |
|:--:|
| *Tela de Chat* |

| ![Questions]({{ site.baseurl }}/images/WiitLive/Screen Shot 2018-10-03 at 10.47.14.png) |
|:--:|
| *Tela de Perguntas e respostas* |

| ![Quizz]({{ site.baseurl }}/images/WiitLive/Screen Shot 2018-10-03 at 10.47.31.png) |
|:--:|
| *Tela de Quizz* |

| ![Callendar]({{ site.baseurl }}/images/WiitLive/Screen Shot 2018-10-03 at 10.47.38.png) |
|:--:|
| *Tela de calendário* |

| ![VideoPlayer]({{ site.baseurl }}/images/WiitLive/Screen Shot 2018-10-03 at 10.47.56.png) |
|:--:|
| *Tela de calendário com o player de video* |

## VNA App:

| ![Main]({{ site.baseurl }}/images/VNA/Simulator Screen Shot - iPhone 6s Plus - 2018-12-03 at 13.14.04.png) |
|:--:|
| *Tela principal do app* |

| ![Menu]({{ site.baseurl }}/images/VNA/Simulator Screen Shot - iPhone 6s Plus - 2018-12-03 at 13.14.10.png) |
|:--:|
| *Tela de Menu* |

| ![Perfil]({{ site.baseurl }}/images/VNA/Simulator Screen Shot - iPhone 6s Plus - 2018-12-03 at 13.15.00.png) |
|:--:|
| *Tela de Perfil* |

| ![Vaga]({{ site.baseurl }}/images/VNA/Simulator Screen Shot - iPhone 6s Plus - 2018-12-03 at 13.19.14.png) |
|:--:|
| *Tela de Descrição da vaga* |

## Trabalhando como Freelancer:

Atualmente estou trabalhando como freelancer desenvolvendo aplicativos iOS para Clientes. Tenho 4 anos de experiência com Swift, e estou sempre estudando pra aprender mais sobre a linguagem para melhorar o meu trabalho.

Sou um cara metódico, e estabeleci que ao menos no começo estarei cobrando R$60 por hora de trabalho.

Como trabalho o dia inteiro (das 9 até as 18), costumo deixar as noites e finais de semana para trabalhar nos projetos, dispondo das 19-23 de segunda a sexta e das 10-18 de sábado e domingo.

Gosto de gerar relatório de atividades, então caso o cliente queira, posso gerar um breve relatório contendo horário de inicio dos trabalhos, horário de término, modificações feitas, duvidas, etc. Para gerenciar os projetos eu gosto muito de utilizar o GIT, portanto tenho conhecimento e certa facilidade com este tipo de ferramenta.

Caso queira me contratar, você pode me encontrar no [Workana](https://www.workana.com/freelancer/7577f01765503cb46109e759038a8009).
