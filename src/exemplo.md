Algoritimo De Needleman-Wunsch
======

!!! Aviso
Este handout é baseado em alguns conceitos de biologia. Para revê-los, volte na introdução da  [APS3](https://ensino.hashi.pro.br/desprog/aps3/index.html).
!!!

O problema 
---------

O algoritmo de Needleman-Wunsch, proposto por Saul Needleman e Christia Wunsch na decada de 1970, têm o objetivo de alinhar duas sequências, ou seja, comparar duas sequências de forma a observar sua similaridade. 

Sua principal aplicação provêm do ramo biológico, sendo responsável pelo alinhamento de nucleotídeos. Existem duas formas de fazê-lo, globalmente e localmente:

 ![](Capture.PNG)

Como é possível observar na imagem anterior, o alinhamento global é responsável por encontrar similaridade ao longo de toda sua extensão, além de manter as regiões onde o alinhamento não é possível através da inserção de gaps, ou seja, as entradas não precisam ter o mesmo tamanho, mas as saídas sim. Já o alinhamento local é feito através de pequenas regiões, desprezando áreas onde não é possível alinhar, e portanto, não preservando o tamanho original das sequências.

O Neefleman-Wunsch utiliza o alinhamento global, e é  implementado segundo um conceito conhecido como [algoritimo guloso](https://pt.wikipedia.org/wiki/Algoritmo_guloso) (kk), ou seja, escolhe as alternativas mais promissoras, nesse caso, o melhor alinhamento de sequências possível!

A formulação
---------

O algoritmo
---------

Para criar um parágrafo, basta escrever um texto contínuo, sem pular linhas.

Você também pode criar

1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md ;` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

;bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
