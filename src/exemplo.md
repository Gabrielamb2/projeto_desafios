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

Problema do Turista em Manhattan
---------
Antes de apresentarmos o funcionamento do algoritmo de Needleman-Wunsch, iremos introduzir o [Problema do Turista de Manhattan](https://homepages.dcc.ufmg.br/~raquelcm/onlinebioinfo/index.php?alias=algoritmo_needleman_wunsch), o qual irá nos ajudar a entender o conceito.

Imagine que você está na cidade de Manhattan, e como um turista raiz, você quer aproveitar o máximo de pontos turísticos possíveis da cidade. Você poderá escolher diversos caminhos diferentes partindo de seu hotel (fonte) e chegando até seu destino (sumidouro).

A imagem abaixo é um esquema da cidade, onde cada uma das setas representa o número de pontos turísticos da rua.

 ![](cidade_manhattan.png)

Para começarmos, vamos preencher a primeira linha e primeira coluna, acumulando os pontos turísticos visitados em cada nó, dessa forma temos algo assim:

 ![](cidade_manhattan_1.png)

 ??? Checkpoint 1
 Para aquecer, qual a melhor caminho para chegar no segundo nó da segunda coluna, de forma a acumular o maior número de pontos?

::: Gabarito
Nessa caso a melhor opção seria vir pela rua da esquerda, acumulando 4 (1 + 3) pontos ao invés de 3 (3 + 0).

 ![](cidade_manhattan_fake.png)
:::
 ???

 ??? Checkpoint 2

 Preencha a segunda linha definindo qual a melhor direção a se seguir para que tenha-se o maior valor em cada nó

::: Gabarito
Seguindo o mesmo raciocínio, para o terceito nó da segnda coluna a melhor opção seria vir pela rua de cima, acumulando 7 (3 + 2 + 2) pontos ao invés de 6 (1 + 3 + 2).

Com esse procedimento, obtemos a seguinte resolução:

 ![](cidade_manhattan_2.png)
:::

???

 ??? Checkpoint 3

 Agora preencha todos os nós.

::: Gabarito
Seguindo o mesmo raciocínio,obtemos a seguinte resolução:

 ![](cidade_manhattan_3.png)

 Desta forma obtemos o caminho a seguir: 

![](cidade_manhattan_4.png)
:::

???



O algoritmo
---------

1. Inicialização da Matriz 

A matriz é construida com uma sequência colocada na primeira coluna e a outra na primeira linha da matriz, como mostrado na imagem abaixo: 

Para preencher a matriz a primeira linha e a primeira coluna da matriz é considerado um gap(vai ser explicado no próximo item), obtendo o seguinte resultado: 

![](matriz-1.PNG)


2. Preencher a Matriz Com a pontuação máxima

Para determina a similaridade entre as sequências é preciso encontrar um forma de mensura-la. E é possivel mensura-las de  varias formas. Nesse handout vamos usar o sistema de pontos: 

| GAP      | Missmatch| Match|
|----------|----------|------|
| -2       | -1       |   1  |

Esses sistema de pontos é utilizado para preencher uma matriz e definir qual é o melhor alinhamento. O GAP é utilizado para inserir lacunas nas sequências para permitir que um algoritmo de alinhamento corresponda a mais termos do que um alinhamento sem lacunas pode. Porem, minimizar as lacunas eh um alinhamneto é importante para crir um alinhamento util, desta forma o GAP possui uma penalidade maio que o Missmatch. O Missmatch corresponde aos que estao desalinhados e o match aos que estão alinhados. 

Vamos tentar preencher o nó na segunda coluna e segunda linha. Se viermos pela esquerda havera um saldo de -4 (-2+(-4)), porque será considerado um gap, assim como se vier por cima. Isso é considerado um GAP, pois se fizermos um GAP na vertical quer dizer que estamos verificando se aquela letra consegue se alinhar com alguma linha, sendo necessario fazer um gap na sequencia da vertical. Não se preoucupe, você vai entender melhor fazendo.

Então deduzimos que vindo por cima e pela esquerda temos -4. Já se vier pela diagonal havera um saldo de -1 (0 +(-1)), tendo em vista que A e T são diferentes. Desta forma temos: 

$$-1 > -4 = -4$$

Então o valor do nó é -1.

IMAGEM

!!! Aviso
Lembrese que são considerados para preencher s[l,c] devemos considerar:
$$max((s[l-1,c]+gap),(s[l,c-1]+gap),(s[l-1,c-1] + (match|missmatch)))$$
!!!

??? Checkpoint 4

 Agora preencha os demais espaços.

::: Gabarito
Seguindo o mesmo raciocínio,obtemos a seguinte resolução:

 ![](matriz_resposta.PNG)

???


3. Traceback

O traceback é o processo que utilizamos para definir o melhor alinhamento. Ou seja, qual é o melhor caminho da fonte até o sumidouro, como no turista de manhattan mostrando ateriormente. 


Complexidade
---------

Agora que temos uma noção de como o algoritmo funciona e qual o problema que ele resolve, podemos discutir um pouco sobre sua complexidade.
Sabemos que para determinar a complexidade do algoritmo, precisamos primeiro identificar se ele possui loops.

??? Checkpoint 5

 Mesmo sem conhecer o código do Needleman-Wunsch, tente pensar intuitivamente se ele teria loops. Se sim, quantos?
 ::: Gabarito

Como precisamos preencher uma matriz, espera-se que o código tenha dois loops, um para percorrer as linhas e outro para percorrer as colunas.

:::

???

??? Checkpoint 6

 Determine a complexidade do algoritmo.

::: Gabarito

Como dito no checkpoint anterior, esperamos que o script tenha dois loops, como o pseudocódigo a seguir:

``` c
void needleman(char seq1[], char seq2[], int n, int m) {
    int matriz[n][m]; //inicializa a matriz
      for(int l = 0; i < n; i++){
        for(int c = 0; l < m; l++){
            //preenche a matriz ...
        }
    }
}
```
Sendo que n e m são os respectivos tamanhos das sequências 1 e 2. Podemos afirmar que a complexidade do algoritmo é O(nm).
:::

???








