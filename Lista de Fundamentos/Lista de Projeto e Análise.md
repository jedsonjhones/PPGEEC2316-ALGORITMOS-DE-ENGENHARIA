# Questão 1: 

**Mostre numericamente com suas implementações dos algoritmos de multiplicação de matrizes que o algoritmo de Strassen é mais rápido que o algoritmo convencional.** <br>

*Multiplicação Convencional de Matrizes:* <br>
A multiplicação convencional de duas matrizes A e B nxn tem uma complexidade de tempo O(n^3) onde o número de operações escala com o cubo do tamanho da matriz.
<br>

*Algoritmo de Strassen:*<br>
O algoritmo de Strassen é uma forma otimizada de multiplicação de matrizes que utiliza divisões e conquistas recursivas. Ele reduz a complexidade para aproximadamente O(n^2.81) usando sete multiplicações de submatrizes em vez de oito. Isso leva a uma melhora na eficiência para grandes matrizes.
<br>
#### Medindo o tempo de execução de cada algoritmo para diferentes tamanhos de matrizes, em python:
```python
import numpy as np
import time

# Função para multiplicação convencional
def matmul_conventional(A, B):
    n = len(A)
    C = np.zeros((n, n))
    for i in range(n):
        for j in range(n):
            for k in range(n):
                C[i][j] += A[i][k] * B[k][j]
    return C

# Função para multiplicação de matrizes pelo algoritmo de Strassen
def strassen(A, B):
    n = len(A)
    if n == 1:
        return A * B
    else:
        mid = n // 2
        # Dividindo as matrizes em submatrizes
        A11, A12, A21, A22 = A[:mid, :mid], A[:mid, mid:], A[mid:, :mid], A[mid:, mid:]
        B11, B12, B21, B22 = B[:mid, :mid], B[:mid, mid:], B[mid:, :mid], B[mid:, mid:]

        # 7 multiplicações de Strassen
        M1 = strassen(A11 + A22, B11 + B22)
        M2 = strassen(A21 + A22, B11)
        M3 = strassen(A11, B12 - B22)
        M4 = strassen(A22, B21 - B11)
        M5 = strassen(A11 + A12, B22)
        M6 = strassen(A21 - A11, B11 + B12)
        M7 = strassen(A12 - A22, B21 + B22)

        # Combinando os resultados
        C11 = M1 + M4 - M5 + M7
        C12 = M3 + M5
        C21 = M2 + M4
        C22 = M1 - M2 + M3 + M6

        # Combinando submatrizes em uma matriz
        C = np.zeros((n, n))
        C[:mid, :mid], C[:mid, mid:], C[mid:, :mid], C[mid:, mid:] = C11, C12, C21, C22
        return C

# Função para comparar tempos de execução
def compare_times(n):
    A = np.random.rand(n, n)
    B = np.random.rand(n, n)

    # Tempo da multiplicação convencional
    start_time = time.time()
    matmul_conventional(A, B)
    conv_time = time.time() - start_time

    # Tempo do algoritmo de Strassen
    start_time = time.time()
    strassen(A, B)
    strassen_time = time.time() - start_time

    return conv_time, strassen_time

# Testando para tamanhos de matrizes 64 e 128
sizes = [64, 128]
for n in sizes:
    conv_time, strassen_time = compare_times(n)
    print(f"Tamanho da matriz: {n}x{n}")
    print(f"Tempo de execução da multiplicação convencional: {conv_time:.5f} segundos")
    print(f"Tempo de execução do algoritmo de Strassen: {strassen_time:.5f} segundos")
    print("-" * 50)

```
![image](https://github.com/user-attachments/assets/556c4f3e-34ed-45b6-87b9-f4d9c2522bf3)

Assim, conseguimos mostrar que o algoritmo de Strassen não supera o método convencional para tamanhos menores de matrizes, mas para 
matrizes significativamente maiores, o Strassen começa a se tornar mais eficiente, especialmente quando o tamanho da matriz aumenta
além de certo ponto, no nosso caso ja notamos isso apartir de matrizes de 128x128, e assim se torna mais evidente comforme altera o 
tamanho da matriz para um valor maior.





# Questão 2:

**Escolha um algoritmo recorrente para aplicar um dos 4 métodos de resolução de recorrência descritos no capítulo 4 para medir o custo da recorrência do algoritmo escolhido. Compare o resultado com medições de tempo.**

*Merge Sort*
O algoritmo escolhido foi o Merge Sort, e ele segue o paradigma de "dividir e conquistar" e possui a seguinte função de recorrência para seu tempo de execução T(n): <br>
![image](https://github.com/user-attachments/assets/89542f7d-1a2e-4264-8507-f9988df16257) <br>
No caso do Merge Sort, temos:
a=2 (número de subproblemas) <br>
b=2 (fator de divisão do problema) <br>
d=1 (o custo da combinação é linear em relação a 𝑛) <br>

Assim, aplicamos o Teorema Mestre, comparando n^d com n^(logb^a):




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
