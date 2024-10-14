### Questão 1: (Exercício 2.1-4 do livro do Cormen)

**Escreva um pseudocódigo para a busca linear e mostre, usando invariância de laço, que o seu algoritmo está correto.**

#### Pseudocódigo para busca linear:
```pseudo```
BUSCALINEAR(A, n, v)
  para i = 1 até n faça
      se A[i] == v então
          retorne i
  retorne NULL
```pseudo```

Mostrando, usando invariância de laço, que o algoritmo está correto:
Definindo a seguinte invariância de laço: No início de cada iteração do laço, para cada índice j tal que 1 ≤ j < i, o valor A[j] ≠ v.

Essa invariância diz que, antes de iniciar a iteração i-ésima, o algoritmo já verificou todas as posições anteriores e não encontrou o valor v.

Inicialização: Antes da primeira iteração do laço (quando i=1), ainda não verificamos nenhum elemento da lista. A condição de invariância é verdadeira, pois não há elementos anteriores a A[1] para verificar.

Manutenção: Suponha que a invariância seja verdadeira no início da iteração i-ésima. Durante essa iteração, o algoritmo verifica o elemento A[i]:

Se A[i] == v, o algoritmo retorna i, o que está correto, pois encontrou o valor v na posição i.
Se A[i] ≠ v, o laço continua para a próxima iteração, e a invariância é mantida, já que verificamos um novo elemento e confirmamos que A[i] ≠ v.
Termino: O laço termina quando i = n + 1, ou seja, quando todos os elementos de A[1…n] foram verificados. Nesse ponto, se o algoritmo não retornar um índice dentro do laço, podemos concluir que o valor v não está presente na lista, e o algoritmo corretamente retorna NULL.

Assim, a invariância de laço garante que, ao final do laço, ou encontramos o valor v e retornamos sua posição correta, ou verificamos todos os elementos e retornamos NULL se v não estiver presente. Logo, o algoritmo de busca linear está correto.