---
layout: post-sidebar
title: "Entendendo CORS em 2 minutos"
date: 2019-11-19 02:10:50
categories: coding
author_name : "Thiago Medeiros"
author_url : /author/tomrlh
author_avatar: tomrlh
show_avatar : true
read_time : 2
feature_image: feature-cors
show_related_posts: true
square_related: recommend-cors
i18n-link: entendendo-cors-em-2-minutos
lang: pt
---





//TODO

0) explicar o CORS

1) mostrar rota sendo acessada pelo POSTMAN, mas não pelo Browser

2) iniciar servidor Python de arquivos e fazer requisição para mostrar a ORIGEM

3) permitir ORIGEM exemplificada

4) fazer requisição novamente



0)

Vocẽ já viu esta imagem alguma vez?

[imagem com a mensagem do CORS]

É um problema que ocorre quando se faz uma requisição HTTP para algum domínio que não está devidamente configurado para suportar o CORS. Provavelmente você soube resolvê-lo, mas vamos tirar os panos de cima desta requisição e entender como este erro acontece.

### O que é o CORS

Em uma frase: é um mecanismo que permite requisições somente por _clientes_ identificados.
Entenda _clientes_ por domínio ou seja, de onde a requisição veio.


A requisição feita fora de um browser não irá lançar este erro, pois não implementam a Same Origin Policy [https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy].
Ex: requisições via postman, curl ou outros clientes HTTP irão funcionar corretamente.

[imagem explicativa]

Esta é a conversa de origens que não implementam a política SOP:

> **HTTP client** - _servidor, me dê o recurso /index.html, please :pray:_

> **Servidor** - _claro, tendo em vista que meu serviço é público, tome_ :package:

> **HTTP client** - _valeu_ :v:


Os browsers, por padrão implementam esta política, então seria assim:


> **Browser** - _servidor, **quais recursos você tem disponível, e quais eu posso pedir**_ :eyes: ?

> **Servidor** - _bem, meus recursos são públicos, portanto todos, e vendo aqui na minha lista,
> 		   você está liberado para perdir o que quiser_

> **Browser** - _sendo assim, me dê o recurso /index.html por gentileza_ :thumbsup:

> **Servidor** - _claro, ai vai_ :package: :wave:

> **Browser** - _valeu_ :v:





.. em andamento

next:

- implementar e deployar spark java simple api (Heroku)

- implementar servidor de arquivos Python que será o cliente Http.
Este faz a requisição a API vai ajax

- fazer a requisição via Postman e mostrar o resultado.
fazer via Browser e mostrar o resultado

- adicionar os headers no cliente http e fazer de novo (continuará o bloqueio do CORS).
Permitir no cliente e fazer a request de novo (irá funcionar).
Para permitir (https://gist.github.com/saeidzebardast/e375b7d17be3e0f4dddf)




Eu preparei um simples cliente para exemplificar isto:

- código do cliente fazendo a consulta. JSFiddle mostra o erro CORS,
POSTMAN mostra a requisição funcionando

- código do serviço mais simples possível (Spring ou Python).
O cliente irá consumir o serviço



