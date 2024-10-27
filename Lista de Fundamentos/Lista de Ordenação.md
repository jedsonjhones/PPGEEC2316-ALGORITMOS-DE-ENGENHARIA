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







# Questão 2:

**Escolha um algoritmo recorrente para aplicar um dos 4 métodos de resolução de recorrência descritos no capítulo 4 para medir o custo da recorrência do algoritmo escolhido. Compare o resultado com medições de tempo.**

*Merge Sort*
O algoritmo escolhido foi o Merge Sort, e ele segue o paradigma de "dividir e conquistar" e possui a seguinte função de recorrência para seu tempo de execução T(n): <br>
![image](https://github.com/user-attachments/assets/89542f7d-1a2e-4264-8507-f9988df16257) <br>
No caso do Merge Sort, temos:
a=2 (número de subproblemas) <br>
b=2 (fator de divisão do problema) <br>
d=1 (o custo da combinação é linear em relação a 𝑛) <br>

Assim, aplicamos o Teorema Mestre, comparando n^d com n^(logb^a): <br>
![image](https://github.com/user-attachments/assets/8bf66115-2959-4ae2-a69b-7bdbe820d02a)

*Implementação em Python**
```python
import time
import random

# Função Merge Sort
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2  # Encontra o ponto médio
        L = arr[:mid]  # Dividi a lista ao meio
        R = arr[mid:]

        merge_sort(L)  # Ordena a primeira metade
        merge_sort(R)  # Ordena a segunda metade

        i = j = k = 0

        # Merge das duas metades
        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                arr[k] = L[i]
                i += 1
            else:
                arr[k] = R[j]
                j += 1
            k += 1

        # Verifica se ainda há elementos em L ou R
        while i < len(L):
            arr[k] = L[i]
            i += 1
            k += 1

        while j < len(R):
            arr[k] = R[j]
            j += 1
            k += 1

# Medir o tempo de execução
def measure_time(n):
    arr = random.sample(range(n), n)  # Gera uma lista aleatória
    start_time = time.time()
    merge_sort(arr)
    end_time = time.time()
    return end_time - start_time

for size in [1000, 2000, 5000, 10000, 20000]:
    exec_time = measure_time(size)
    print(f"Tamanho da entrada: {size}, Tempo de execução: {exec_time:.6f} segundos")
```
![image](https://github.com/user-attachments/assets/019fbaed-3dbd-42e2-aef4-7bf6aa851e7d) <br>
Assim, O tempo de execução está aumentando, mas não de maneira linear,que é o que era esperado, já que o Merge Sort tem complexidade  O(n log n)


# Questão 3:

**O problema de balanceamento de cargas busca atribuir tarefas de tamanhos diferentes a trabalhadores, de modo a minimizar a carga máxima que um trabalhador irá executar. Em um problema em que temos n tarefas e k trabalhadores (n > k), considere que o balanceador irá distribuir as n/k primeiras tarefas para o primeiro trabalhador, as n/k tarefas seguintes para o segundo trabalhador, e assim por diante. Mostre numericamente como permutar aleatoriamente os dados de entrada, que são as cargas de cada tarefa, pode influenciar na solução desse balanceador.** <br>

Se eu por exemplo considerar Considere n=6 tarefas e k=2 trabalhadores e Tamanhos das tarefas: [5,2,4,7,1,3]:
<br>

se eu fizer uma Distribuição sem permutação eu vou ter:<br>
Trabalhador 1: [5,2,4] → Carga total = 5 + 2 + 4 = 11 <br>
Trabalhador 2: [7,1,3] → Carga total = 7 + 1 + 3 = 11. <br>
assim a carga máxima atribuída a um trabalhador é 11. <br>
<br>
Agora se eu fizer uma permutação aleatória das tarefas: <br>
Permutação aleatória, por exemplo: [4,1,7,2,3,5]. <br>
Trabalhador 1: [4,1,7] → Carga total = 4 + 1 + 7 = 12. <br>
Trabalhador 2: [2,3,5] → Carga total = 2 + 3 + 5 = 10. <br>
A carga máxima agora é 12. <br>

Assim podemos notar que a permutação aleatória das tarefas pode resultar em uma distribuição de cargas que aumenta ou diminui a carga 
máxima atribuída aos trabalhadores.

