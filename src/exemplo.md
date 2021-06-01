Algoritimo De Needleman-Wunsch
======

!!! Aviso
Este handout é baseado em alguns conceitos de biologia. Para revê-los, volte na introdução da  [APS3](https://ensino.hashi.pro.br/desprog/aps3/index.html).
!!!

O problema
---------

Neste handout vamos falar sobre o algoritmo de Needleman-Wunsch, proposto por Saul Needleman e Christia Wunsch na decada de 1970. O objetivo principal deste algoritmo é alinhar duas sequências, e por essa razão vamos começar entendendo melhor este conceito.

Imagine que temos duas sequências de chars:
* s1: A T A
* s2: T A C

Alinhar essas sequências significa, basicamente, obter duas outras sequências, s3 e s4, de mesmo tamanho, de forma que possuam todos os caracteres originais das sequências s1 e s2, respectivamente. Além disso, os caracteres devem estar na mesma ordem da sequência original, porém não necessariamente juntos. Podemos resumir na seguinte imagem:

![](alinhamento_ex1.png)

Note que o ponto chave é a inserção de espaços vazios, os quais vamos chamar de gaps. Inserí-los torna possível modificar parcialmente as sequências originais, de forma a combinar seus caracteres. A cereja do bolo é que esse conceito também funciona para sequências que não são do mesmo tamanho, como nos exemplos logo abaixo:

;alinhamento_exemplos

Agora que entendemos o conceito de alinhamento, podemos voltar ao algoritmo em si. Sua principal aplicação provêm do ramo biológico, sendo responsável pelo alinhamento de nucleotídeos (coincidentemente representados pelas mesmas letras dos exercícios anteriores). Existem duas formas de fazê-lo, globalmente e localmente:

 ![](Capture.png)

Como é possível observar na imagem anterior, o alinhamento global é responsável por encontrar similaridade ao longo de toda sua extensão, além de manter as regiões onde o alinhamento não é possível através da inserção de gaps, ou seja, as entradas não precisam ter o mesmo tamanho, mas as saídas sim. Já o alinhamento local é feito através de pequenas regiões, desprezando áreas onde não é possível alinhar, e portanto, não preservando o tamanho original das sequências.

O Neefleman-Wunsch utiliza o alinhamento global, e é  implementado segundo um conceito conhecido como [algoritimo guloso](https://pt.wikipedia.org/wiki/Algoritmo_guloso), ou seja, escolhe as alternativas mais promissoras, nesse caso, o melhor alinhamento de sequências possível!

Score
---------

Para determinr quanto o alinhamento é bom, é utilizado um sistema de pontuação:


|    Gap   | Missmatch | Match |
|----------|-----------|-------|
|    -2    |     -1    |   1   |

São três as possibilidades:

* Gap: utilizado para inserir lacunas nas sequências, de forma a estendê-las criando espaços vazios. Minimizar as lacunas é importante para criar um alinhamento útil, desta forma o Gap possui uma penalidade maior. ([penalidade de lacuna](https://pt.wikipedia.org/wiki/Penalidade_para_lacunas)). Gaps são contados quando não andamos na diagonal, ou seja, não damos passos em direção ao sumidouro, mas sim estamos "estendendo" o caminho... capiche?

* Missmatch: corresponde aos nucleotídeos que estão desalinhados. Possui pontuação negativa porque queremos alinhar a sequência e não desalinhá-la. 



* Match: correspode aos nucleotídeos que estão alinhados. 


Algoritmo 
---------

Conhecendo agora o sistema de pontos, como podemos encontrar o alinhamento com o melhor score possivel? Ou seja, o melhor alinhamento possível?

Para determinar o melhor alinhamento possível é utilizado uma matriz, a qual vamos explicar seu funcionamento nos próximos passos: 

1. Inicialização da Matriz 

Para determinarmos o melhor alimento de duas sequências, inicializamos a matris colocando uma sequência na primeira linha e a aoutra na primeira coluna,como mostra a imagem abaixo para as sequências mostradas anteriorment (ATA & TACA): 

![](matriz_inicial.png)


O ponto mais importante para você entender como que funciona o preenchimento dessa matriz é que cada célula representa uma versão menor do problema. Um pouco confuso não é? mas a ideia é sair da fonte e chegar ao sumidouro, com o melhor score possível. Vamos olhar so para as primeiras duas linhas:


![](matriz_inicial_linha.png)

Como vamos preencher isso? 

Lembrando que cada célula representa uma versão menor do problema e que cada célula é responsável pelo score de um alinhamento, vamos observar a primeira célula vazia:

![](matriz_inicial_linha_celula1.png)

Essa célula é responsável pelo score da letra A com algo vazio, assim a unica forma de alinhar é com um Gap. 
Agora vamos olhar a próxima célula: 

![](matriz_inicial_linha_celula2.png)

Como vamos alinhar AT com algo vazio, sendo que A esta alinhado com um Gap, T também sera alinhado com um Gap. 
Agora vamos olhar a próxima célula: 

![](matriz_inicial_linha_celula3.png)

Analogamente como explicado anteriormente, teremos que alinhar ATA com algo vazio, sendo que A e T está alinhado com um Gap, o outro A também será alinhado com um Gap, obtendo assim a primeira linha: 

![](matriz_inicial_linha_completa.png)

Ou seja, apartir dessa linha podemos concluir que:
1. o score de alinhamento de A com algo vazio é -2,
2. o score de alinhamento de AT com algo vazio é -4,
3. o score de alinhamento de ATA com algo vazio é -6,

É possível observar que o alinhamento A (1) é uma versão menor do problema de alinhamento AT(2), e AT(2) é uma versão menor do problema de alinhamento ATA. 

??? Checkpoint 4
Como fizemos anteriormente, precisamos preencher agora a primeira coluna da matriz. Agora é com você :)

::: Gabarito
;Coluna


Ou seja, apartir dessa coluna podemos concluir que todos os nucleotideos foram alinhados com Gaps, obtendo assim:
1. o score de alinhamento de T com algo vazio é -2,
2. o score de alinhamento de TA com algo vazio é -4,
3. o score de alinhamento de TAC com algo vazio é -6,
4. o score de alinhamento de TACA com algo vazio é -8.


:::
???

Bom, acho que deu pra ter uma noção, mas vamos fazer agora uma célula um pouco mais dificil :)

2. Preenchendo as outras células da matriz:

??? Checkpoint 5
Se quisermos determinar o score dessa célula:

![](matriz_inteira.png)

O score dessa célula será o resultado do alinhamento de quais sequências?

::: Gabarito

Será o alinhamento da sequência A com T. 

![](matriz_inteira1.png)

:::
???

Bom, para determinarmos o valor dessa células temos 3 opções, porém precisamos determinar a que nos trará o melhor alinhamento. Assim, vamos analisar as opções:

1. Vir por cima e fazer um Gap na sequencia da horizontal, assim teriamos um score de -4 

![](possibilidade1.png)

2. Vir pela esquerda e fazer um Gap na sequencia da vertical, assim teriamos um score de -4

![](possibilidade2.png)

3. Vir pela diagonal e ter um MissMatch, assim teriamos um score de -1

![](possibilidade3.png)

Desta forma, para termos o maior score possível é utilizar a opção 3:

![](matriz_inteira2.png)

Então podemos concluir que para determinar o score de uma célula precisamos de 3 scores de uma versão menor do problema. Da posição:
* [l-1][c], 
* [l][c-1],
* [l-1][c-1].

??? Checkpoint 6
Agora é com você, preencha as outras células da matriz:

![](matriz_inteira3.png)

::: Gabarito

![](matriz_solucao.png)

As setas vermelhas indicam gap.

As setas laranjas indicam missmatch,é especialmente quando damos passos na diagnonal, mas a letras (nucleotídeos) não são iguais.

E as setas verdes indicam match, é quando damos passos na diagonal e as letras são iguais.


:::
???

Complexidade
---------

Agora que temos uma noção de como o algoritmo funciona e qual o problema que ele resolve, podemos discutir um pouco sobre sua complexidade.
Sabemos que para determinar a complexidade do algoritmo, precisamos primeiro identificar se ele possui loops.

??? Checkpoint 7

 Mesmo sem conhecer o código do Needleman-Wunsch, tente pensar intuitivamente: ele teria loops? Se sim, quantos?
 ::: Gabarito

Como precisamos preencher uma matriz, espera-se que o código tenha dois loops, um para percorrer as linhas e outro para percorrer as colunas.

:::

???

??? Checkpoint 8

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

Desafio
---------

Agora que você entendeu como encontrar o melhor score possível para o alinhamento de duas sequências, precisamos entender como usar esse score para inserir os gaps e alinhá-las. Esse processo é chamado de Traceback.

A ideia principal é fazer o caminho inverso, ou seja, sair do sumidouro e voltar para a fonte a partir das setas já desenhadas (note que existe apenas um caminho possível que sai do sumidouro e chega até a fonte).

Vamos ver um exemplo. Considere a matriz preenchida nos checkpoints anteriores:

![](matriz_solucao.png)

Partindo do sumidouro temos o seguinte caso:

![](matriz_finalizada_sumidouro.png)

Repare que nesse caso, o caminho seria voltando pela diagonal, pois a unica seta que chega no sumidouro vem da diagonal. Desta forma o resultado seria:

![](matriz_finalizada_sumidouro_resolucao.png)

??? Checkpoint 7
Seguindo a mesma lógica, complete o caminho, lembre-se de partir do sumidouro e chegar até a fonte. Desenhe setas no caminho percorrido, como demostrado acima.

::: Gabarito
![](matriz_finalizada_solucao.PNG)

Veja abaixo uma animação da resolução completa:

;alinhamento
:::

??? 






