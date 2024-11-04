# Questão 1: 

**Implemente as funções da seção 6.5 do livro do Cormen 4th Ed. em sua linguagem favorita e proponha um exemplo de uso com uma demonstração.** <br>

Essa seção do livro do Cormen, trata de operações sobre heaps, sendo as suas fonções nessa seção sobre heaps de máximo.
<br>

*heap_maximum: Retorna o maior elemento do heap.* <br>
*heap_extract_max: Remove e retorna o maior elemento do heap.* <br>
*max_heap_insert: Insere um novo elemento no heap.* <br>
*heap_increase_key: Aumenta a chave de um elemento no heap.* <br>


#### Implementação em python: <br>

```python
import math

class HeapMaximo:
    def __init__(self):
        self.heap = []

    def pai(self, i):
        return (i - 1) // 2

    def esquerdo(self, i):
        return 2 * i + 1

    def direito(self, i):
        return 2 * i + 2

    def heap_maximo(self):
        if len(self.heap) == 0:
            raise IndexError("Heap está vazio")
        return self.heap[0]

    def extrair_maximo(self):
        if len(self.heap) < 1:
            raise IndexError("Heap está vazio")
        maximo = self.heap[0]
        self.heap[0] = self.heap[-1]
        self.heap.pop()
        self.max_heapificar(0)
        return maximo

    def max_heapificar(self, i):
        esq = self.esquerdo(i)
        dir = self.direito(i)
        maior = i
        if esq < len(self.heap) and self.heap[esq] > self.heap[i]:
            maior = esq
        if dir < len(self.heap) and self.heap[dir] > self.heap[maior]:
            maior = dir
        if maior != i:
            self.heap[i], self.heap[maior] = self.heap[maior], self.heap[i]
            self.max_heapificar(maior)

    def aumentar_chave(self, i, chave):
        if chave < self.heap[i]:
            raise ValueError("A nova chave é menor que a chave atual")
        self.heap[i] = chave
        while i > 0 and self.heap[self.pai(i)] < self.heap[i]:
            self.heap[i], self.heap[self.pai(i)] = self.heap[self.pai(i)], self.heap[i]
            i = self.pai(i)

    def inserir(self, chave):
        self.heap.append(-math.inf)
        self.aumentar_chave(len(self.heap) - 1, chave)

    def __str__(self):
        return str(self.heap)

```
![image](https://github.com/user-attachments/assets/be9b16ac-3b64-4e7e-a1a3-8c8569ab943c)







# Questão 2:

**Mostre com experimentos numéricos quando suas próprias implementações de Quicksort e do Quicksort aleatório são mais vantajosas quando comparadas uma com a outra.**

*Expectativa*
Para o Quicksort clássico, quando a lista está ordenada ou quase ordenada, o pivô escolhido é sempre o maior ou o menor, criando partições desbalanceadas, e isso leva a um caso de pior desempenho, levando assim a cair no pior caso, já para Quicksort Aleatório, ele escolhe o pivô aleatoriamente, reduzindo a probabilidade de cair no pior caso <br>

*Implementação em Python**
```python
import random
import time
import matplotlib.pyplot as plt

# Função de Quicksort Clássico
def quicksort_classico(arr, inicio, fim):
    while inicio < fim:
        p = particionar(arr, inicio, fim)
        if p - inicio < fim - p:
            quicksort_classico(arr, inicio, p - 1)
            inicio = p + 1  # Elimina a recursão à direita
        else:
            quicksort_classico(arr, p + 1, fim)
            fim = p - 1  # Elimina a recursão à esquerda

def particionar(arr, inicio, fim):
    pivo = arr[fim]
    i = inicio - 1
    for j in range(inicio, fim):
        if arr[j] <= pivo:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[fim] = arr[fim], arr[i + 1]
    return i + 1

# Função de Quicksort Aleatório
def quicksort_aleatorio(arr, inicio, fim):
    while inicio < fim:
        p = particionar_aleatorio(arr, inicio, fim)
        if p - inicio < fim - p:
            quicksort_aleatorio(arr, inicio, p - 1)
            inicio = p + 1  # Elimina a recursão à direita
        else:
            quicksort_aleatorio(arr, p + 1, fim)
            fim = p - 1  # Elimina a recursão à esquerda

def particionar_aleatorio(arr, inicio, fim):
    pivo_index = random.randint(inicio, fim)
    arr[pivo_index], arr[fim] = arr[fim], arr[pivo_index]
    return particionar(arr, inicio, fim)

# Função para medir o tempo de execução
def medir_tempo(func, arr, *args):
    inicio = time.time()
    func(arr, *args)
    fim = time.time()
    return fim - inicio

# Tamanhos das listas para o experimento
tamanhos = [1000, 5000, 10000]
resultados_classico = {'aleatorio': [], 'ordenado': [], 'quase_ordenado': []}
resultados_aleatorio = {'aleatorio': [], 'ordenado': [], 'quase_ordenado': []}

# Experimento
for n in tamanhos:
    # Casos de teste
    lista_aleatoria = random.sample(range(n), n)
    lista_ordenada = list(range(n))
    lista_quase_ordenada = list(range(n))
    for i in range(0, n, 50):  # Perturba alguns elementos na lista quase ordenada
        lista_quase_ordenada[i] = random.randint(0, n)
    
    # Executando Quicksort Clássico
    resultados_classico['aleatorio'].append(medir_tempo(quicksort_classico, lista_aleatoria[:], 0, n - 1))
    resultados_classico['ordenado'].append(medir_tempo(quicksort_classico, lista_ordenada[:], 0, n - 1))
    resultados_classico['quase_ordenado'].append(medir_tempo(quicksort_classico, lista_quase_ordenada[:], 0, n - 1))
    
    # Executando Quicksort Aleatório
    resultados_aleatorio['aleatorio'].append(medir_tempo(quicksort_aleatorio, lista_aleatoria[:], 0, n - 1))
    resultados_aleatorio['ordenado'].append(medir_tempo(quicksort_aleatorio, lista_ordenada[:], 0, n - 1))
    resultados_aleatorio['quase_ordenado'].append(medir_tempo(quicksort_aleatorio, lista_quase_ordenada[:], 0, n - 1))

# Plotando os resultados
plt.figure(figsize=(15, 8))
tipos = ['aleatorio', 'ordenado', 'quase_ordenado']
for i, tipo in enumerate(tipos):
    plt.subplot(1, 3, i + 1)
    plt.plot(tamanhos, resultados_classico[tipo], label='Quicksort Clássico', marker='o')
    plt.plot(tamanhos, resultados_aleatorio[tipo], label='Quicksort Aleatório', marker='x')
    plt.title(f'Lista {tipo.capitalize()}')
    plt.xlabel('Tamanho da lista')
    plt.ylabel('Tempo de execução (s)')
    plt.legend()

plt.tight_layout()
plt.show()

```
![image](https://github.com/user-attachments/assets/9719cc5e-6f44-4adc-babc-b2dcc6dcd022) <br>
Assim, apartir dos experimentos vemos que o Quicksort Aleatório é uma boa esxolha em listas ordenadas e quase ordenadas, onde o Quicksort Clássico tende a cair no pior caso.


# Questão 3:

**Mostre com experimentos numéricos quando o Radix-sort com o Count-sort é mais rápido que o Count-sort sozinho. Utilize suas próprias implementações ou alguma implementação existente explicando os resultados** <br>

**Radix Sort:** Algoritmo de ordenação baseado na posição dos dígitos, que ordena os elementos dígito por dígito (ou bit a bit) do menor para o maior dígito (ou inversamente). Ele é eficiente para listas grandes com números de vários dígitos, usando Counting Sort para ordenar em cada posição de dígito. <br>

**Counting Sort:** Algoritmo de ordenação linear, que conta o número de ocorrências de cada valor e reorganiza os elementos com base nesses contadores. É mais eficiente para listas com um intervalo de valores limitado, mas sua complexidade aumenta com o tamanho do intervalo. <br>

Assim o Radix Sort é mais eficiente em listas com valores grandes (como números com muitos dígitos), enquanto o Counting Sort é vantajoso para listas de números pequenos ou com intervalos pequenos. <br>

Assim o caso em que o Radix-sort com o Count-sort vai ser mais rapido que o Count-sort sozinho, vai ser quando em tiver utilizando listas de numeros grandes.<br>



```python
import time
import random
import matplotlib.pyplot as plt

# Função de Counting Sort para listas com valores grandes
def counting_sort(arr):
    max_val = max(arr)  # Encontrar o valor máximo
    count = [0] * (max_val + 1)  # Array de contagem baseado no valor máximo

    # Contagem das ocorrências
    for num in arr:
        count[num] += 1

    # Reconstruindo o array ordenado
    index = 0
    for i, freq in enumerate(count):
        for _ in range(freq):
            arr[index] = i
            index += 1

# Função de Counting Sort para ser usada dentro do Radix Sort (ordena por dígito)
def counting_sort_for_radix(arr, exp):
    n = len(arr)
    output = [0] * n
    count = [0] * 10  # Radix usa base 10 (dígitos 0 a 9)

    # Contagem das ocorrências baseadas no dígito atual
    for num in arr:
        index = (num // exp) % 10
        count[index] += 1

    # Atualiza o array de contagem para posição final
    for i in range(1, 10):
        count[i] += count[i - 1]

    # Construção do array de saída ordenado pelo dígito atual
    for i in range(n - 1, -1, -1):
        index = (arr[i] // exp) % 10
        output[count[index] - 1] = arr[i]
        count[index] -= 1

    # Copia para o array original
    for i in range(n):
        arr[i] = output[i]

# Função de Radix Sort usando Counting Sort como sub-rotina
def radix_sort(arr):
    max_val = max(arr)
    exp = 1
    while max_val // exp > 0:
        counting_sort_for_radix(arr, exp)
        exp *= 10

# Função para medir o tempo de execução de uma função de ordenação
def medir_tempo(func, arr):
    inicio = time.time()
    func(arr)
    fim = time.time()
    return fim - inicio

# Tamanhos das listas para o experimento
tamanhos = [1000, 5000, 10000]
resultados_counting = []
resultados_radix = []

# Experimento com listas de números grandes
for n in tamanhos:
    # Lista com números grandes (6 dígitos)
    lista_grandes_numeros = [random.randint(100000, 999999) for _ in range(n)]
    
    # Executando Counting Sort em números grandes
    tempo_counting = medir_tempo(counting_sort, lista_grandes_numeros[:])
    resultados_counting.append(tempo_counting)
    
    # Executando Radix Sort em números grandes
    tempo_radix = medir_tempo(radix_sort, lista_grandes_numeros[:])
    resultados_radix.append(tempo_radix)

# Plotando os resultados
plt.figure(figsize=(10, 5))
plt.plot(tamanhos, resultados_counting, label='Counting Sort em números grandes', marker='o')
plt.plot(tamanhos, resultados_radix, label='Radix Sort em números grandes', marker='x')
plt.xlabel('Tamanho da lista')
plt.ylabel('Tempo de execução (s)')
plt.legend()
plt.title('Comparação de Desempenho: Counting Sort vs. Radix Sort com números grandes')
plt.show()
```

![image](https://github.com/user-attachments/assets/b3ee5da7-0f76-4193-8df4-d3752ebfa458)



