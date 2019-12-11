---
layout: post-sidebar
title: "Understanding CORS Policy in 2 minutes"
date: 2019-11-19 02:10:50
categories: coding
author_name : "Thiago Medeiros"
author_url : /author/tomrlh
author_avatar: tomrlh
show_avatar : true
read_time : 3
feature_image: feature-cors
show_related_posts: true
square_related: recommend-cors
i18n-link: understanding-cors-policy-2-minutes
---




If you are here, probably you have seen this message before:

`Access to XMLHttpRequest at 'destination domain' from origin 'origin domain' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.`

This message occurs when a HTTP request is done to a domain that is not properly configured to support CORS.
Let's undercover this initials and understand this error, to not get surprised next time you see it, and know how to deal with it.


### What is CORS

In one phrase:
> a mechanism that only allow identified _clients_ requests. 

Read _clients_ as domains that where the request came from.


The request made out of a browser will not throw this error, cause are the browsers that explicitily implements the [Same Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy).
So requests by POSTMAN, curl or other HTTP clients will work properly.


### Preflight

Is a pre-request to knows what can be accessed for the requesting domain.

![Conversation](/img/post-assets/understand-cors-2-minutes/conversation.jpg)


The conversation below ilustrates origins that not implements the SOP (Same Origin Policy):

> **HTTP client** - _hey server, give me the /index.html resource please :pray:_

> **Server** - _sure_ :package: :wave:

> **HTTP client** - _thanks_ :v:


Therefore, a conversation between browser and server will have the Preflight request:


> **Browser** - _server, **which are the resources do you have available for me?**_ :eyes: ?

> **Server** - _well, my resources are public, so all_

> **Browser** - _give me the /index.html resource then_ :thumbsup:

> **Server** - _sure, there it goes_ :package: :wave:

> **Browser** - _thanks_ :v:


Considering the above mentioned, requests from the same server to the same server, the CORS policy is not applied.
Otherwise, requests from a server to another, it needs the CORS validation to deliver the resource.


![Same Origin Policy](/img/post-assets/understand-cors-2-minutes/same-origin-policy.svg)

### In Practice

To exemplify this communication, I've deployed a simple [Javalin](https://javalin.io) server that allows only the
**[localhost:8050](https://javalin-simple-service.herokuapp.com/)** to request (in a cross-origin scenario).

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

To make the crossed request in fact, I'll need a different server, so I've used the Simple HTTP Server](https://docs.python.org/2/library/simplehttpserver.html):


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


With all up, with the SimpleHTTPServer serving at port 8050, all goes ok:

![Cors OK](/img/post-assets/understand-cors-2-minutes/permitted-cors-request.jpg)

But changing the port to a different one (8060), CORS goes into action:

![Cors not OK](/img/post-assets/understand-cors-2-minutes/not-permitted-cors-request.jpg)


### Why to use CORS?

Security, cause you can define a selective permission to server resources and, through CORS, define which resource
can be delivered to which origin.

#### Rerefences

* [What is Cors](https://www.codecademy.com/articles/what-is-cors)
* [Same Origin Policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)