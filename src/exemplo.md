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

O Neefleman-Wunsch utiliza o alinhamento global, e é  implementado segundo um conceito conhecido como [algoritimo guloso](https://pt.wikipedia.org/wiki/Algoritmo_guloso), ou seja, escolhe as alternativas mais promissoras, nesse caso, o melhor alinhamento de sequências possível!

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
Seguindo o mesmo raciocínio, para o terceito nó da segunda coluna a melhor opção seria vir pela rua de cima, acumulando 7 (3 + 2 + 2) pontos ao invés de 6 (1 + 3 + 2).

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
Agora sim podemos voltar ao problema do algoritmo de Needleman-Wunsch e resolvê-lo. A ideia é essecialmente a mesma: precisamos preencher uma matriz saindo da fonte e chegando ao sumidouro, de forma a acumular a maior pontuação. 

Err... nada é tão simples, temos um problema, como vamos saber quais valores acumular a cada decisão? Bom, para isso vamos usar um sistema de pontuação:

|    Gap   | Missmatch | Match |
|----------|-----------|-------|
|    -2    |     -1    |   1   |

Vamos utilizá-lo para definir qual é o melhor alinhamento. São três as possibilidades:

* Gap: utilizado para inserir lacunas nas sequências, de forma a estendê-las criando espaços vazios. Minimizar as lacunas é importante para criar um alinhamento útil, desta forma o Gap possui uma penalidade maior. ([penalidade de lacuna](https://pt.wikipedia.org/wiki/Penalidade_para_lacunas)). Gaps são contados quando não andamos na diagonal, ou seja, não damos passos em direção ao sumidouro, mas sim estamos "estendendo" o caminho... capiche?

* Missmatch: corresponde aos nucleotídeos que estão desalinhados. Possui pontuação negativa porque queremos alinhar a sequência e não deslinhá-la. É contado especialmente quando damos passos na diagnonal, mas a letras (nucleotídeos) não são iguais.


* Match: correspode aos nucleotídeos que estão alinhados. É contado quando damos passos na diagonal e as letras são iguais.

O preenchimento da matriz será dividido em três partes:

1. Inicialização da Matriz 

A matriz é inicializada com uma sequência colocada na primeira coluna e a outra na primeira linha. Além disso sabemos que na fonte (ponto de partida) temos 0 pontos acumulados, como mostra a imagem abaixo: 

![](matriz_initial.PNG)

??? Checkpoint 4
Como fizemos anteriormente, precisamos preencher a primeira linha/coluna da matriz. Nesse caso, qual(quais) das três opções (gap, match, missmatch) você usaria?

::: Gabarito
Nesse caso, estamos andando somente para os lados, portanto usaríamos somente gaps.
:::
???
 
 ??? Checkpoint 5

 Preencha a primeira linha/coluna da matriz assim como foi feito no problema do turista de Manhattan.

::: Gabarito
Como estamos usando apenas gaps, o resultado da matriz inicializada seria:

![](matriz-1.png)
:::

???

2. Preencher a Matriz Com a pontuação máxima

Agora precisamos preencher o resto da matriz, de forma a acumular a maior pontuação. 

Vamos ao exemplo do segundo nó da segunda coluna: chegar no nó por cima ou pelo lado signfica inserir um gap, ou seja, acumularíamos -4 pontos (-2 - 2), porém ao vir pela diagonal, como A != T, significa um missmatch, ou seja, acumularíamos -1 ponto (0 - 1). Portanto, o valor do nó é -1, pois -1 > -4.

![](matriz-2.PNG)

??? Checkpoint 6

Seguindo a mesma lógica trabalhada anteriormente, preencha o restante da linha.

::: Gabarito
 ![](matriz-3.PNG)
 
 Sendo m a matriz, temos:
 * m[2][2] = missmatch;
 * m[3][2] = missmatch;
 * m[4][2] = gap;
 * m[5][2] = gap
???

??? Checkpoint 7
 Termine de preencher a matriz.

::: Gabarito
 ![](matriz-4.PNG)
???


3. Traceback

Por fim, precisamos definir o melhor alinhamento, ou seja, qual é o melhor caminho do sumidouro até a fonte, da mesma forma que fizemos para o problema do turista de Manhattan. 

As regras são as seguintes:

* Andar na diagonal signifca manter a posição atual
dos nucleotídeos, portanto fazemos isso quando eles já estão alinhados.

* Caso não estejam alinhados, precisamos procurar a melhor opção entre os nós que sobraram. Note que andar para os lados, signfica estender a sequência das colunas, já andar para cima/baixo significa estender a sequência das linhas.

??? Checkpoint 8
Vamos brincar de labirinto? Comece de trás para frente e encontre o melhor caminho do sumidouro para a fonte. Desenhe setas apontando para cada direção escolhida.
 ::: Gabarito
 ![](matriz-5.PNG)
:::

???

??? Checkpoint 9
Usando as regras citadas anteriormente e o caminho escolhido no checkpoint anterior, determine o alinhamento ótimo das sequências.
 ::: Gabarito

;alinhamento
:::
???

Complexidade
---------

Agora que temos uma noção de como o algoritmo funciona e qual o problema que ele resolve, podemos discutir um pouco sobre sua complexidade.
Sabemos que para determinar a complexidade do algoritmo, precisamos primeiro identificar se ele possui loops.

??? Checkpoint 10

 Mesmo sem conhecer o código do Needleman-Wunsch, tente pensar intuitivamente: ele teria loops? Se sim, quantos?
 ::: Gabarito

Como precisamos preencher uma matriz, espera-se que o código tenha dois loops, um para percorrer as linhas e outro para percorrer as colunas.

:::

???

??? Checkpoint 11

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



