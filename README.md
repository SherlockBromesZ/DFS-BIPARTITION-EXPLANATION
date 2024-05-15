# Verificador de Grafo Bipartido

Este programa verifica se um grafo é bipartido. Um grafo é bipartido se é possível colorir os vértices usando duas cores de tal forma que não existam dois vértices adjacentes com a mesma cor.

## Como funciona o algoritmo

O algoritmo usa uma busca em profundidade (DFS, Depth-First Search) para tentar colorir o grafo. A ideia é começar com um nó, colori-lo e tentar colorir todos os seus vizinhos com a cor oposta. Se em algum ponto encontrarmos um vizinho já colorido com a mesma cor do nó atual, o grafo não é bipartido.

## Detalhes da Implementação

O código é implementado em C++ e utiliza a seguinte estrutura:

1. **Leitura do Grafo**: O grafo é lido do `stdin` onde o usuário fornece o número de vértices e arestas seguido pelas arestas.
2. **DFS para checar bipartição**: A função `dfsBipartido` tenta colorir o grafo e verifica se ele pode ser bipartido.
3. **Vetores de Adjacência**: O grafo é representado usando uma lista de adjacência.
4. **Vetor de Cores**: Um vetor de cores é usado para marcar os nós com 0, 1, ou -1 indicando a cor ou não visitado, respectivamente.

### Código Completo

```cpp
#include<bits/stdc++.h>
using namespace std;

bool dfsBipartido(int inicio, vector<vector<int>> &grafo, vector<int> &cor, int corAtual){
    cor[inicio] = corAtual;
    
    for(int vizinho : grafo[inicio]){
        if(cor[vizinho] == -1){
            if(!dfsBipartido(vizinho, grafo, cor, 1 - corAtual)) return false;
        }else if(cor[vizinho] == cor[inicio]) return false;
    }
    return true;
}

int main()
{
    int num_nos, num_arestas;
    cin >> num_nos >> num_arestas;
    
    vector<vector<int>> grafo(num_nos);
    
    for(int i = 0; i < num_arestas; i++){
        int u, v;
        cin >> u >> v;
        
        grafo[u].push_back(v);
        grafo[v].push_back(u);
    }
    
    vector<int> cor(num_nos, -1);
    
    bool eh_bipartido = true;
    
    for(int i = 0; i < num_nos; i++){
        if(cor[i] == -1){
            if(!dfsBipartido(i, grafo, cor, 0)){
                eh_bipartido = false;
                break;
            }
        }
    }
    
    if(eh_bipartido){
        cout << "e" << endl;
    }else{
        cout << "n" << endl;
    }
    return 0;
}
```

## Simulação Detalhada com Entrada de Exemplo

Considere a entrada:

```
4 4
0 1
1 2
2 3
3 0
```

Este input representa um grafo com 4 vértices e 4 arestas formando um quadrado (ou um ciclo de 4 vértices).

### Passo a Passo do Algoritmo

1. **Inicialização**:
   - Número de nós (`num_nos`) = 4
   - Número de arestas (`num_arestas`) = 4
   - `grafo` = `[[], [], [], []]`
   - `cor` = `[-1, -1, -1, -1]`

2. **Construção do Grafo**:
   - Aresta 0-1: `grafo` = `[[1], [0], [], []]`
   - Aresta 1-2: `grafo` = `[[1], [0, 2], [1], []]`
   - Aresta 2-3: `grafo` = `[[1], [0, 2], [1, 3], [2]]`
   - Aresta 3-0: `grafo` = `[[1, 3], [0, 2], [1, 3], [2, 0]]`

3. **Checagem de Bipartição Usando DFS**:

   - Começamos a explorar o grafo a partir do nó 0. Inicialmente, todos os nós estão sem cor (`cor` = `[-1, -1, -1, -1]`).

4. **Coloração e DFS a partir do nó 0**:
   - Colorimos o nó 0 com a cor 0 (`cor` = `[0, -1, -1, -1]`).
   - Visitamos o vizinho 1 do nó 0. O nó 1 é colorido com 1 (`cor` = `[0, 1, -1, -1]`).

5. **Exploração a partir do nó 1**:
   - Nó 1 já está colorido com 1, então continuamos.
   - Vizinho 0 do nó 1 já está colorido com 0, o que é correto.
   - Vizinho 2 do nó 1 não está colorido, então colorimos com `1 - 1 = 0` (`cor` = `[0, 1, 0, -1]`).

6. **Exploração a partir do nó 2**:
   - Nó 2 já está colorido com 0, então continuamos.
   - Vizinho 1 do nó 2 já está colorido com 1, o que é correto.
   - Vizinho 3 do nó 2 não está colorido, então colorimos com `1 - 0 = 1` (`cor` = `[0, 1, 0, 1]`).

7. **Exploração a partir do nó 3**:
   - Nó 3 já está colorido com 1, então continuamos.
   - Vizinho 2 do nó 3 já está colorido com 0, o que é correto.
   - Vizinho 0 do nó 3 já está colorido com 0.
   - Todos os vizinhos do nó 3 estão corretamente coloridos em relação ao nó 3.

8. **Final da DFS**:
   - Todos os nós foram visitados e coloridos sem encontrar dois vizinhos adjacentes com a mesma cor.
   - Portanto, o grafo é bipartido.

### Resultado

- O algoritmo verifica todos os nós e conclui que o grafo é bipartido.
- A saída para essa entrada será `e`.

### Discussão e Observações

- O processo de coloração alternada é fundamental para verificar a propriedade de bipartição.
- Se algum vizinho tivesse a mesma cor que o nó atual durante a DFS, teríamos identificado que o grafo não é bipartido e o resultado seria `n`.
- O algoritmo funciona corretamente para grafos desconexos, pois tenta colorir a partir de cada nó não visitado, garantindo que todos os componentes sejam verificados.

### Conclusão

Este programa é uma implementação direta e eficiente para verificar se um grafo é bipartido usando DFS. A lógica de coloração é simples e baseia-se na capacidade de colorir alternadamente os nós a partir de qualquer nó não visitado.
