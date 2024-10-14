# Questão 1: (Exercício 2.1-4 do livro do Cormen)

**Escreva um pseudocódigo para a busca linear e mostre, usando invariância de laço, que o seu algoritmo está correto.**

#### Pseudocódigo para busca linear:
```pseudo
BUSCALINEAR(A, n, v)
  para i = 1 até n faça
      se A[i] == v então
          retorne i
  retorne NULL
```
Mostrando, usando invariância de laço, que o algoritmo está correto:
Definindo a seguinte invariância de laço: No início de cada iteração do laço, para cada índice j tal que 1 ≤ j < i, o valor A[j] ≠ v.

Essa invariância diz que, antes de iniciar a iteração i-ésima, o algoritmo já verificou todas as posições anteriores e não encontrou o valor v.

- Inicialização: Antes da primeira iteração do laço (quando i=1), ainda não verificamos nenhum elemento da lista. A condição de invariância é verdadeira, pois não há elementos anteriores a A[1] para verificar.

- Manutenção: Suponha que a invariância seja verdadeira no início da iteração i-ésima. Durante essa iteração, o algoritmo verifica o elemento A[i]: <br>
Se A[i] == v, o algoritmo retorna i, o que está correto, pois encontrou o valor v na posição i. <br>
Se A[i] ≠ v, o laço continua para a próxima iteração, e a invariância é mantida, já que verificamos um novo elemento e confirmamos que A[i] ≠ v.
- Termino: O laço termina quando i = n + 1, ou seja, quando todos os elementos de A[1…n] foram verificados. Nesse ponto, se o algoritmo não retornar um índice dentro do laço, podemos concluir que o valor v não está presente na lista, e o algoritmo corretamente retorna NULL.

Assim, a invariância de laço garante que, ao final do laço, ou encontramos o valor v e retornamos sua posição correta, ou verificamos todos os elementos e retornamos NULL se v não estiver presente. Logo, o algoritmo de busca linear está correto.

# Questão 2:

**Implemente o algoritmo de ordenação por inserção e crie uma cópia anotada dele que mede o número de operações no modelo da Random Access Machine  (RAM, seção 2.2 livro do Cormen). Usando entradas de tamanho crescente, mostre em um gráfico quando o tempo de execução no modelo RAM diverge de medições feitas em uma máquina real.**

```python
import time
import matplotlib.pyplot as plt

# Implementação manual do algoritmo de ordenação por inserção
def insertion_sort(arr):
    n = len(arr)
    for i in range(1, n):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key

# Implementação anotada do algoritmo de ordenação por inserção para contar operações RAM
def insertion_sort_annotated(arr):
    n_operations = 0  # Contador de operações RAM
    n = len(arr)
    for i in range(1, n):
        key = arr[i]
        n_operations += 1  # Atribuição key = arr[i]
        j = i - 1
        n_operations += 1  # Atribuição de j = i - 1
        while j >= 0 and arr[j] > key:
            n_operations += 2  # Comparação e acesso a arr[j]
            arr[j + 1] = arr[j]
            n_operations += 1  # Atribuição de arr[j + 1] = arr[j]
            j -= 1
            n_operations += 1  # Decremento de j
        n_operations += 1  # Saída do while
        arr[j + 1] = key
        n_operations += 1  # Atribuição final arr[j + 1] = key
    return n_operations

# Função para comparar tempo de execução e operações RAM
def compare_performance():
    sizes = [10, 50, 100, 200, 500, 1000, 2000, 5000]  # Tamanhos dos arrays
    time_results = []
    ram_operations = []

    for size in sizes:
        arr = list(range(size, 0, -1))  # Vetor decrescente como pior caso
        
        # Medindo o tempo real
        start_time = time.time()
        insertion_sort(arr.copy())  # Usa uma cópia para não alterar o original
        end_time = time.time()
        time_results.append(end_time - start_time)

        # Medindo as operações no modelo RAM
        n_operations = insertion_sort_annotated(arr.copy())
        ram_operations.append(n_operations)

    # Plotando os resultados
    plt.figure(figsize=(10, 6))
    plt.plot(sizes, time_results, label="Tempo Real (s)", marker='o')
    plt.plot(sizes, ram_operations, label="Operações RAM", marker='x')
    plt.xlabel('Tamanho da Entrada')
    plt.ylabel('Tempo/Operações')
    plt.title('Comparação: Tempo Real vs Operações RAM')
    plt.legend()
    plt.grid(True)
    plt.show()

# Executa a comparação
compare_performance()

```
![Grafico](https://github.com/user-attachments/assets/ede671eb-3922-4e93-8be6-124d63594ebf)

# Questão 3:

**Mostre numericamente com suas implementações dos algoritmos de insertion-sort e merge-sort como se comporta o desempenho de cada algoritmo utilizando entradas de tamanho crescente, considerando entradas de pior caso, melhor caso e caso médio. Análise, para cada tipo de entrada, se existe algum ponto a partir do qual um algoritmo passa a ser mais rápido que o outro.**

