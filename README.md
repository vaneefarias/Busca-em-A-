## BUSCA EM A*
---
1. Definição do Problema no Mapa
Um problema de busca em um mapa é formalmente definido por cinco componentes principais:

Estado Inicial: Onde o agente começa (ex: Em(Arad)).
Ações: As rotas possíveis a partir de cada cidade (ex: Ir(Sibiu), Ir(Zerind)).
Modelo de Transição: O resultado de cada ação (ex: se estou em Arad e vou para Sibiu, o resultado é estar em Sibiu).
Teste de Objetivo: Verifica se o estado atual é o destino final (ex: Em(Bucareste)).
Função de Custo do Caminho: O custo numérico de cada caminho, geralmente a distância em quilômetros entre as cidades no mapa.

---

2. Funcionamento do Algoritmo A*
O A* minimiza o custo total estimado da solução, utilizando a função de avaliação $f(n)$:
$$f(n) = g(n) + h(n)$$

$g(n)$: É o custo real do caminho do estado inicial até o nó atual $n$. No mapa, seria a soma das distâncias percorridas até a cidade atual.
$h(n)$: É a função heurística, que estima o custo do caminho mais barato do nó $n$ até o objetivo. No caso de mapas, a heurística mais comum é a distância em linha reta entre a cidade atual e o destino final.

---

3. Implementação da Busca
Para implementar o algoritmo, você deve seguir a estrutura de uma busca em grafo para evitar caminhos redundantes e laços infinitos:

Borda (Fronteira): Utilize uma fila de prioridade para armazenar os nós que foram gerados, mas ainda não expandidos. Eles devem ser ordenados pelo menor valor de $f(n)$.
Conjunto Explorado: Mantenha uma lista (geralmente uma tabela hash) de todos os nós que já foram expandidos para não processá-los novamente.
Expansão: O algoritmo retira o nó com menor $f(n)$ da borda, verifica se ele é o objetivo e, se não for, expande seus sucessores calculando o $f(n)$ de cada um deles e adicionando-os à borda.

---

4. Condição para a Eficácia (Admissibilidade)
Para garantir que o A* encontre a solução ótima (o caminho mais curto), a heurística $h(n)$ deve ser admissível. Uma heurística admissível é aquela que nunca superestima o custo para atingir o objetivo. No mapa, a distância em linha reta é sempre admissível, pois não há caminho físico mais curto entre dois pontos do que a reta que os une.
---

## Exemplo Prático
Se o objetivo é ir de Arad para Bucareste:

O algoritmo avalia as cidades vizinhas de Arad.
Calcula para cada uma: (distância percorrida desde Arad) + (distância em linha reta daquela cidade até Bucareste).
Escolhe a cidade com o menor valor total e repete o processo até chegar a Bucareste.

___

## Explicando o código do algoritmo:

* 1º:
O grafo do mapa
A tabela de heurística
Ambos como dicionários.

---
2º usar o HEAPQ, através do [import heapq], que é uma biblioteca nativa do Python que implementa uma Fila de Prioridade
Ele irá garantir que sempre o menor valor seja o primeiro.

---
3º 
