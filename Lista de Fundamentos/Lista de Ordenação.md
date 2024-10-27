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

