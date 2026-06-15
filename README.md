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

## Explicando o código do algoritmo A*

### 1º: Estrutura de Dados

O grafo do mapa e a tabela de heurística são definidos. Ambos são estruturados como dicionários em Python.

### 2º: Biblioteca Importada

Usamos o `import heapq`, que é uma biblioteca nativa do Python que implementa uma **Fila de Prioridade** (estrutura de dados do tipo *Min-Heap*). Ela garante que o elemento com o menor valor numérico seja sempre o primeiro a ser retirado da lista.

### 3º: Preparação Inicial

A variável `borda` começa como uma lista vazia, que será preenchida conforme o algoritmo for descobrindo novas possibilidades. O `f_inicial` calcula a nota da primeira cidade: o custo real ($g$) é `0` (pois ainda não andamos nenhum quilômetro), somado ao valor de $h$ (heurística da linha reta até o objetivo).

### 4º: Alimentando a Borda

Ao usar `heapq.heappush`, colocamos a cidade inicial na borda garantindo a regra de prioridade. Guardamos dentro dela uma tupla com quatro informações essenciais:

1. A nota atual (`f_inicial`).
2. O nome da cidade atual (`inicio`).
3. O histórico do caminho percorrido até aqui (`[inicio]`), que é uma lista.
4. O valor do custo real acumulado ($g$), que começa em `0`.

### 5º: Diário de Viagem

A variável `explorado` começa como um dicionário vazio. Posteriormente, ela guardará as cidades cuja exploração já foi finalizada, associando-as ao menor custo encontrado para alcançá-las, a fim de evitar loops.

### 6º: O Laço de Busca e o Teste de Parada

O `while borda` mantém o algoritmo rodando enquanto existirem cidades descobertas na mesa que ainda precisam ser analisadas. A primeira ação dentro do laço é retirar da borda a cidade com a menor nota $f$. Em seguida, o `if atual == objetivo` valida se essa cidade retirada é o nosso destino final. Se for, o algoritmo para na hora e devolve o caminho; se não for, ele segue em frente.

### 7º: O Filtro de Segurança

No bloco `if atual in explorado...`, o algoritmo verifica se a cidade atual já foi completamente explorada antes. Se ela já foi visitada por um caminho mais curto (ou de igual valor), ela é ignorada e o algoritmo pula para a próxima. Isso evita loops e impede que o código perca tempo reexplorando rotas piores. Caso seja uma visita inédita ou mais eficiente, ela é registrada no dicionário `explorado`.

### 8º: Olhando os Vizinhos

No laço `for vizinho`, o algoritmo analisa todas as cidades que fazem fronteira direta com a cidade atual através do mapa. A variável `distancia_passo` guarda o custo real da estrada entre a cidade atual e esse vizinho. A partir daí, o código calcula:

* O novo custo real acumulado até o vizinho (`g_vizinho`).
* A estimativa em linha reta dele até o objetivo (`h_vizinho`).
* A nota final do vizinho (`f_vizinho = g_vizinho + h_vizinho`).

### 9º: Tomada de Decisão e Inserção

Na tomada de decisão final do laço, o algoritmo observa se esse vizinho ainda não foi explorado de verdade ou se o novo caminho oferece um custo real ($g$) menor do que o registrado anteriormente. Se passar nesse teste, o algoritmo atualiza a lista do caminho, calcula a nova rota e usa o `heapq.heappush` para colocar esse vizinho na mesa (`borda`). O processo se repete de forma cíclica até que o objetivo final seja alcançado.

---
