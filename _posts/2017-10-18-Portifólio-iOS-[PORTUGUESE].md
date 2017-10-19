---
layout: post
title: [PORTUGUESE] Portfólio de aplicativos iOS.
---

Este é o meu portfólio de Aplicações iOS que eu desenvolvi. Aqui você vai encontrar todos os projetos que eu participei, e o meu objetivo é atualizar essa página sempre que possível!

## Minha História com Desenvolvimento iOS:

Olá, caso você ainda não me conheça, você pode saber um pouco sobre minha pessoa na sessão ["About"](https://luzejunior.github.io/about/) deste site.
Mas neste portfólio eu vou falar um pouco de como comecei a programar e mais que isso, aprendi a gostar desta linguagem de programação chamada Swift e tudo que envolve desenvolvimento mobile para iOS.

Desde criança sempre fui curioso para saber como as coisas funcionavam, e no começo da hera dos Smartphones eu sempre me perguntava como que aquilo tudo funcionava, e como que era a programação a este tipo de dispositivo.

Um belo dia, no fim de 2015, muito tempo depois da minha infância, já de barba, na universidade e trabalhando no LAViD (Laboratório de Aplicações de Video Digital da Universidade Federal da Paraíba), apareceu a oportunidade de trabalhar em um projeto chamado de ("Culturi")[http://www.culturi.com.br/], que nada mais era que uma rede social de eventos em parceria com o Ministério da Cultura. Então, participando do projeto, fui escalado para fazer um aplicativo para iOS complementar ao Culturi completamente do zero, em no máximo dois meses. Problema nenhum, certo? ERRADO, pois eu não tinha NENHUMA experiência com Swift, muito menos um Mac.

E é ai que começa a minha aventura de aprender a programar em uma linguagem relativamente simples, mas com uma arquitetura bem complexa em menos de um mês para desenvolver o mais rápido possível um aplicativo em dois meses. O projeto saiu, e se chama ["Meu olhar"](http://meuolhar.culturi.com.br/). Mas, infelizmente ele foi interrompido pelos novos ministros da cultura do nosso novo presidente da república no final de 2016.

Como tomei gosto pelo desenvolvimento mobile, resolvi que continuaria nesta área, e ai, no começo de 2017 veio o segundo projeto. Dessa vez em parceria com o Ministério do Desenvolvimento Social, desenvolvi um aplicativo do "Bolsa Família" para os beneficiários ficarem por dentro das noticias, novidades, datas de pagamento, etc.

Hoje eu tento a sorte como freelancer, posso desenvolver aplicativos nativos do zero, ou fazer alterações pontuais em aplicativos. Tenho conhecimento em diversos frameworks como AFNetworking, Alamofire, todas as API's do facebook, twitter, google maps, e qualquer outra coisa que o cliente desejar. Caso eu não saiba, eu procuro, pesquiso e aprendo. Tento sempre dar o melhor da minha pessoa em tudo que faço e tenho mania de ser organizado com as minhas coisas. Qualquer dúvidas, você, caro leitor (ou cliente) pode me mandar uma mensagem no meu email pessoal, que você pode ver clicando [aqui](https://luzejunior.github.io/about/).

Atualmente utilizo um Macbook Pro 13 2017 para programar, e disponho de 4 dispositivos para teste, sendo eles: iPhone 7 (iOS 10.3.3), iPhone 6 (iOS 9.3.5), e dois iPad Mini 2 (um com iOS 9.3.5 e outro com iOS 8.4). Como sou Apple Entusiasta, possuo outros dispositivos como iPods e iPhones antigos, mas infelizmente eles não servem para teste, e são apenas itens de coleção.

Se você leu até aqui, muito obrigado! Vou deixar de besteira e ir logo ao que interessa: O portfólio.

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

![SplashScreen]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Splash.png)
*SplashScreen*

![MainMenu]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-MainOff.png)
*Main Menu com usuário deslogado.*

![Login]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-LoginScreen.png)
*Xib de Login*

![MainMenu]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-MainOn.png)
*Main Menu com usuário Logado.*

![Calendário]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Calendario.png)
*Calendário de Informações.*

![Maps]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Maps.png)
*Maps com integração com API do google.*

![Programas]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Programas.png)![Criança_Feliz]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-CFeliz.png)
*Tela com os principais programas do governo.*

![Sobre]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Sobre.png)
*Tela de Sobre*

![Social]({{ site.baseurl }}/images/BolsaFamilia/BolsaFamilia-Social.png)
*Tela de Social*

#### Código fonte:

Você pode encontrar o código fonte deste aplicativo no meu GitHub: [Repositório MDS-BolsaFamilia](https://github.com/luzejunior/MDS-BolsaFamilia)

## Planos Futuros:

Atualmente estou trabalhando como freelancer desenvolvendo aplicativos iOS para Clientes, mas estou disponível para ser contratado. Tenho 2 anos de experiência com Swift, e estou sempre estudando pra aprender mais sobre a linguagem para melhorar o meu trabalho.
Em breve colocarei o meu currículo aqui.
