---
layout: post-sidebar
title: "Java Streams: As Collections Melhoradas"
date: 2019-01-14 08:50:28
categories: java coding
author_name : "Thiago Medeiros"
author_url : /author/tomrlh
author_avatar: tomrlh
show_avatar : true
read_time : 3
feature_image: feature-sea
show_related_posts: true
square_related: recommend-sea
i18n-link: java-streams-as-collections-melhoradas
lang: pt
---





Um Stream em Java é uma sequência de dados, que são produzidos e manuseados como em uma fábrica: um produto passa por um número de etapas finitas que dependem uma da outra. Quando o produto passa por uma etapa, não volta mais, ou seja, a operação é realizada e o fluxo segue.

Seguindo a analogia, o produto é o dado, e cada etapa da fabricação é uma _Operação Stream_. Os Streams possuem 3 partes:

![Partes do Stream](/img/post-assets/java-streams-the-improved-collections/stream-parts-pt.png)

<!-- ![The Stream Parts](/img/post-assets/java-streams-the-improved-collections/stream-parts-en.png) -->

1. **Fonte**: a fonte de dados;
2. **Operações Intermediárias**: métodos de manipulação/transformação dos dados;
3. **Operação Terminal**: entrega o resultado produzido.



### Fonte

Existem algumas maneiras de se criar um Stream:

{% highlight java %}
Stream<String> vazio = Stream.empty();

Stream<Integer> umElemento = Stream.of(1);
Stream<Integer> array = Stream.of(1, 2, 3);

// A partir de uma lista
Stream<String> lista = Arrays.asList("a", "b", "c");
Stream<String> deUmaLista = list.stream();
{% endhighlight %}





### Principais Operações Intermediárias



- **filter(Predicate)** -> retorna um stream contendo os elementos que atendam a condição
- **map(Function)** -> executa uma operação em cada elemento. Equivalente ao map() do javascript
- **flatMap(Function,** Stream) -> desctrincha diferentes collections e armazena em um novo stream
- **distinct()** -> remove os duplicados
- **sorted()** -> ordena o array
- **peek(Consumer)** -> permite debugar o Stream sem modificá-lo
- **limit(long)** -> limita o acesso ao stream até o limite especificado
- **skip(long)** -> retorna um stream pulando as posições indicadas no long




### Principais Operações Terminais



- **toArray()** -> converte stream para Array
- **count()** -> conta os elementos do stream
- **reduce(Consumer)** -> reduz o stream a um elemento em um Optional
- **forEach(Consumer)** -> equivalente ao for melhorado
- **min(Comparator)** -> retorna o valor mínimo do stream
- **max(Comparator)** -> retorna o valor máximo do stream
- **allMatch(Predicate)** -> verifica se todos elementos do stream atendem a condição
- **findFirst()** -> retorna o primeiro elemento do stream em um Optional



## Por que usar Streams?

A maioria do que se faz com listas, se faz com streams. Porém, Streams possui vantagens como:

- **legibilidade** -> o código é mais declarativo ->
- **menos código** -> reduz o uso de ifs e fors tradicionais
- **eficiência** -> os streams se prezam a fazer estritamente a função que o método propõe, da maneira mais eficiente
- **poder** -> possui métodos prontos para a maioria das interações com coleções




## Na prática


Digamos que se quer os 3 primeiros nomes de uma lista de strings, que comecem com a letra _"b"_ e que tenha tamanho _4_. Usando listas, teriamos o seguinte código:

{% highlight java %}
List<String> lista = Arrays.asList(
	"baba", "carro", "livro", "base", "carta", "bebê", "mesa"
);
List<String> listaFiltrada = new ArrayList<>();

for(String palavra: lista) {
	if(palavra.length() == 4 && palavra.startsWith("b"))
	    listaFiltrada.add(palavra);
}
Collections.sort(listaFiltrada);
Iterator<String> iter = listaFiltrada.iterator();

for(String palavra: listaFiltrada) {
	System.out.println(palavra);
}
{% endhighlight %}





Agora com Streams:

{% highlight java %}
Stream<String> stream = Stream.of(
	"baba", "carro", "livro", "base", "carta", "bebê", "mesa"
);
stream.filter(p -> p.length() == 4)
	.filter(p.startsWith("b"))
	.sorted()
	.forEach(System.out::println);
{% endhighlight %}

Perceba que os métodos podem ser encadeados e que a maioria dos métodos da interface Stream 
recebem _Interfaces Funcionais_. A [filter()](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#filter-java.util.function.Predicate-) 
recebe um [Predicate](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html). 
[sorted()](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#sorted--) 
possui duas versões, uma que não recebe nenhum parâmetro, outra que recebe um 
[Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html). O
[forEach()](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#forEach-java.util.function.Consumer-) é bem útil pois pode substituir o for tradicional e o melhorado, trazendo mais legibilidade. Este recebe um [Consumer](https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html).

Referências:

* [Stream Docs](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#filter-java.util.function.Predicate-)
* [Whats the difference between Streams and Collections in Java 8](https://stackoverflow.com/questions/39432699/what-is-the-difference-between-streams-and-collections-in-java-8)