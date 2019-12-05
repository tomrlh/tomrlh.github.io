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






Se está aqui, provavelmente você já viu esta mensagem:

`Access to XMLHttpRequest at 'destination domain' from origin 'origin domain' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.`

Esta mensagem ocorre quando se faz uma requisição HTTP para algum domínio que não está devidamente configurado para suportar o CORS. Vamos tirar os panos de cima desta política para na próxima vez que ver este erro, não se espantar e saber exatamente como resolver.

### O que é o CORS

Em uma frase: 
> é um mecanismo que permite requisições somente por _clientes_ identificados.

Entenda _clientes_ como o domínio de onde a requisição veio.


A requisição feita fora de um browser não irá lançar este erro, pois são os browsers que explicitamente implementam a [Same Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy).
Portanto, requisições via postman, curl ou outros clientes HTTP irão funcionar normalmente.

### Preflight

É uma requisição prévia, para saber o que está liberado para acesso para aquele domínio que fez a requisição.


![Conversa](/img/post-assets/understand-cors-2-minutes/conversation.jpg)


Esta é a conversa de origens que não implementam a política SOP:

> **HTTP client** - _servidor, me dê o recurso /index.html :pray:_

> **Servidor** - _claro, tome_ :package:

> **HTTP client** - _valeu_ :v:


Entre browser e servidor, a conversa haveria o Preflight:


> **Browser** - _servidor, **quais recursos você tem disponível, e quais posso obter**_ :eyes: ?

> **Servidor** - _bem, meus recursos são públicos, portanto todos_

> **Browser** - _sendo assim, me dê o recurso /index.html_ :thumbsup:

> **Servidor** - _claro, ai vai_ :package: :wave:

> **Browser** - _valeu_ :v:


Sendo assim, requisições de recursos para o mesmo servidor de onde a requisição partiu, a política CORS não é aplicada.
Porém, requisições de recursos de um servidor para outro, é preciso passar na validação do CORS para que o recurso seja entregue.

![Same Origin Policy](/img/post-assets/understand-cors-2-minutes/same-origin-policy.svg)

### Na prática

Para exemplificar esta comunicação, levantei um servidor [Javalin](https://javalin.io) simples que permite somente a origem 
**[localhost:8050](https://javalin-simple-service.herokuapp.com/)**, *quando implementando a política CORS*.


#### Servidor permitingo origem


{% highlight java %}
import io.javalin.Javalin;

public class Main {

	public static void main(String[] args) {
		Javalin app = Javalin.create(config -> {
			config.enableCorsForOrigin("http://localhost:8050");
		})
			.start(getHerokuAssignedPort()).get("/", ctx -> ctx.result("Hello! You have crossed CORS policy"));
	}

	private static int getHerokuAssignedPort() {
		String herokuPort = System.getenv("PORT");
		if (herokuPort != null) {
			return Integer.parseInt(herokuPort);
		}
		return 7000;
	}
}
{% endhighlight %}


#### Servidor permitingo origem

Para realizar a requisição ao servidor de uma origem diferente, o que de fato irá realizar o cruzamento de origens, foi usado o [Simple HTTP Server](https://docs.python.org/2/library/simplehttpserver.html):


{% highlight html %}
<html>
	<head>
		<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.19.0/axios.js"></script>		
	</head>

	<body>
		<button onclick="search()">Execute Request to other origin</button>

		<script>
		function search() {
			axios.get('https://javalin-simple-service.herokuapp.com/', {
				params: {},
				headers: {'randomHeader': 'randomValue'}
			})
			.then(function (response) {
				console.log(response);
			})
		}
		</script>
	</body>
</html>
{% endhighlight %}


### Por que usar o CORS?

A principal vantagem é **sergurança**, já que pode-se definir a permissão seletiva ao acessar o servidor e, através do CORS, definir qual recurso será liberado a requisições vindas de origens externas.


#### Referências

* [What is Cors](https://www.codecademy.com/articles/what-is-cors)
* [Same Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)