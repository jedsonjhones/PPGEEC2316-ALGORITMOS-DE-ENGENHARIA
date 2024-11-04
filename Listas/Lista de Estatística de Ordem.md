# Questão 1: 

**Implemente o algoritmo de mínimo e máximo simultâneos da seção 9.1 do livro do Cormen, 4a Ed na sua linguagem favorita e mostre através de medição de tempo que é mais rápido que a abordagem não-simultânea para um vetor de entrada suficientemente grande.** <br>

#### Implementação em python: <br>

```python
import random
import time

# Encontra o mínimo e o máximo simultâneos
def min_max_simultaneous(arr):
    n = len(arr)
    if n == 0:
        return None, None
    elif n == 1:
        return arr[0], arr[0]
    
    # Inicializa min_val e max_val usando o primeiro par de elementos
    if arr[0] < arr[1]:
        min_val, max_val = arr[0], arr[1]
    else:
        min_val, max_val = arr[1], arr[0]

    # Processa pares restantes
    for i in range(2, n - 1, 2):
        if arr[i] < arr[i + 1]:
            min_val = min(min_val, arr[i])
            max_val = max(max_val, arr[i + 1])
        else:
            min_val = min(min_val, arr[i + 1])
            max_val = max(max_val, arr[i])

    # Se o número de elementos for ímpar, compara o último elemento
    if n % 2 == 1:
        min_val = min(min_val, arr[-1])
        max_val = max(max_val, arr[-1])

    return min_val, max_val

# Abordagem não-simultânea
def min_non_simultaneous(arr):
    return min(arr)

def max_non_simultaneous(arr):
    return max(arr)

# Gerando um vetor grande de números aleatórios
arr = random.sample(range(20_000_000), k=10_000_000)

# Medindo o tempo para o algoritmo de mínimo e máximo simultâneos
start_time_simultaneous = time.time()
min_val_simultaneous, max_val_simultaneous = min_max_simultaneous(arr)
end_time_simultaneous = time.time()
time_simultaneous = end_time_simultaneous - start_time_simultaneous

# Medindo o tempo para a abordagem não-simultânea
start_time_non_simultaneous = time.time()
min_val_non_simultaneous = min_non_simultaneous(arr)
max_val_non_simultaneous = max_non_simultaneous(arr)
end_time_non_simultaneous = time.time()
time_non_simultaneous = end_time_non_simultaneous - start_time_non_simultaneous

# Resultados
print("Tempo (Simultâneo):", time_simultaneous, "segundos")
print("Tempo (Não-Simultâneo):", time_non_simultaneous, "segundos")
print("Mínimo:", min_val_simultaneous, "| Máximo:", max_val_simultaneous)

```
![image](https://github.com/user-attachments/assets/364dabdf-68dd-4657-8140-8d888766b55e)







# Questão 2:


# Questão 3:

**Implemente o algoritmo da mediana ponderada e use-o para resolver o item e do Problema 9-3 do Cormen, 4a Ed.** <br>

**Radix Sort:** Algoritmo de ordenação baseado na posição dos dígitos, que ordena os elementos dígito por dígito (ou bit a bit) do menor para o maior dígito (ou inversamente). Ele é eficiente para listas grandes com números de vários dígitos, usando Counting Sort para ordenar em cada posição de dígito. <br>

**Counting Sort:** Algoritmo de ordenação linear, que conta o número de ocorrências de cada valor e reorganiza os elementos com base nesses contadores. É mais eficiente para listas com um intervalo de valores limitado, mas sua complexidade aumenta com o tamanho do intervalo. <br>

Assim o Radix Sort é mais eficiente em listas com valores grandes (como números com muitos dígitos), enquanto o Counting Sort é vantajoso para listas de números pequenos ou com intervalos pequenos. <br>

Assim o caso em que o Radix-sort com o Count-sort vai ser mais rapido que o Count-sort sozinho, vai ser quando em tiver utilizando listas de numeros grandes.<br>



```python
def weighted_median(values, weights):
    # Ordenar os elementos por valores
    sorted_data = sorted(zip(values, weights), key=lambda x: x[0])
    values_sorted, weights_sorted = zip(*sorted_data)

    # Calcular a soma total dos pesos
    total_weight = sum(weights_sorted)
    half_weight = total_weight / 2

    # Encontra o ponto onde a soma acumulada dos pesos atinge metade do peso total
    cumulative_weight = 0
    for i in range(len(values_sorted)):
        cumulative_weight += weights_sorted[i]
        if cumulative_weight >= half_weight:
            return values_sorted[i]  # Mediana ponderada encontrada
# Aplicando
values = [1, 2, 3, 4, 5]
weights = [2, 1, 5, 1, 2]

median = weighted_median(values, weights)
print("A mediana ponderada é:", median)

```

![image](https://github.com/user-attachments/assets/b3ee5da7-0f76-4193-8df4-d3752ebfa458)



