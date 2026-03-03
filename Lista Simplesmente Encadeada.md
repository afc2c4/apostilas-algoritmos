# Lista Simplesmente Encadeada

## Introdução

A **Lista Simplesmente Encadeada** é uma das estruturas de dados mais fundamentais e importantes no estudo da ciência da computação. Ela é uma coleção linear de elementos, onde cada elemento, chamado de **nó**, aponta para o próximo nó na sequência. Diferente de um array, onde os elementos são armazenados em posições de memória contíguas, os nós de uma lista encadeada podem estar espalhados pela memória. A conexão entre eles é feita através de **ponteiros** ou **referências**.

Imagine uma fila de pessoas onde cada pessoa sabe quem é a próxima na fila. Se você quiser encontrar alguém, precisa começar do início e perguntar a cada pessoa quem é a próxima até encontrar quem procura. Essa é a essência de uma lista simplesmente encadeada.

## Conceitos Fundamentais

Para entender uma lista simplesmente encadeada, é crucial conhecer alguns termos:

*   **Nó (Node)**: É o bloco fundamental da lista. Cada nó contém dois campos principais:
    *   **Dado (Data)**: O valor real que o nó armazena (pode ser um número, uma string, um objeto, etc.).
    *   **Próximo (Next)**: Uma referência (ou ponteiro) para o próximo nó na sequência. O último nó da lista terá seu campo `Próximo` apontando para `null` (ou `Nothing` em algumas linguagens), indicando o fim da lista.

*   **Cabeça (Head)**: É o primeiro nó da lista. Através da `Cabeça`, temos acesso a todos os outros nós da lista. Se a `Cabeça` for `null`, significa que a lista está vazia.

## Como Funciona?

As operações básicas em uma lista simplesmente encadeada incluem:

*   **Inserção**: Adicionar um novo nó à lista (no início, no fim ou em uma posição específica).
*   **Remoção**: Excluir um nó da lista.
*   **Busca**: Encontrar um nó com um valor específico.
*   **Exibição/Travessia**: Percorrer todos os nós da lista para exibir seus dados.

## Vantagens e Desvantagens

### Vantagens

*   **Alocação Dinâmica de Memória**: A lista pode crescer ou diminuir de tamanho dinamicamente em tempo de execução, ao contrário de arrays que geralmente têm um tamanho fixo.
*   **Inserção e Remoção Eficientes**: Adicionar ou remover elementos no início da lista é muito rápido (complexidade de tempo O(1)). Inserir ou remover no meio ou no fim também pode ser eficiente se o nó anterior for conhecido (O(1)), mas a busca pelo nó anterior pode ser O(n).

### Desvantagens

*   **Acesso Sequencial**: Para acessar um elemento em uma posição específica (ex: o quinto elemento), é necessário percorrer a lista desde o início. Isso resulta em uma complexidade de tempo O(n) para acesso.
*   **Uso de Memória**: Cada nó requer memória adicional para armazenar o ponteiro para o próximo nó, o que pode ser um overhead em comparação com arrays.
*   **Dificuldade de Acesso ao Nó Anterior**: Não há uma maneira direta de acessar o nó anterior a um dado nó, a menos que se percorra a lista desde o início, mantendo um ponteiro para o nó anterior.

## Implementação em C#

Vamos implementar uma lista simplesmente encadeada em C#. Primeiro, definiremos a classe `Node` e depois a classe `LinkedList`.

### Classe `Node`

```csharp
// Node.cs

using System;

public class Node
{
    public int Data { get; set; } // O valor que o nó armazena
    public Node Next { get; set; } // Referência para o próximo nó

    public Node(int data)
    {
        Data = data;
        Next = null; // Inicialmente, o próximo nó é nulo
    }
}
```

**Explicação do Código `Node.cs`:**

*   `public int Data { get; set; }`: Esta propriedade armazena o valor real do nosso nó. Neste exemplo, estamos usando um `int` (número inteiro), mas poderia ser qualquer tipo de dado (string, objeto, etc.). O `{ get; set; }` significa que podemos ler (`get`) e escrever (`set`) o valor desta propriedade.
*   `public Node Next { get; set; }`: Esta é a parte crucial de uma lista encadeada. `Next` é uma referência para outro objeto do tipo `Node`. É como um 
elo que conecta um nó ao próximo. Se `Next` for `null`, significa que este é o último nó da lista.
*   `public Node(int data)`: Este é o construtor da classe `Node`. Quando criamos um novo nó, passamos o `data` que ele deve armazenar. O `Next` é inicializado como `null` porque, ao ser criado, um nó ainda não está conectado a nenhum outro nó.

### Classe `LinkedList`

Agora, vamos criar a classe `LinkedList` que gerenciará os nós.

```csharp
// LinkedList.cs

using System;

public class LinkedList
{
    public Node Head { get; private set; } // O primeiro nó da lista

    public LinkedList()
    {
        Head = null; // Inicialmente, a lista está vazia
    }

    // 1. Inserir no Início
    public void AddFirst(int data)
    {
        Node newNode = new Node(data);
        newNode.Next = Head;
        Head = newNode;
        Console.WriteLine($"Adicionado {data} no início da lista.");
    }

    // 2. Inserir no Final
    public void AddLast(int data)
    {
        Node newNode = new Node(data);
        if (Head == null)
        {
            Head = newNode;
            Console.WriteLine($"Adicionado {data} no final da lista (lista estava vazia). ");
            return;
        }

        Node current = Head;
        while (current.Next != null)
        {
            current = current.Next;
        }
        current.Next = newNode;
        Console.WriteLine($"Adicionado {data} no final da lista.");
    }

    // 3. Remover do Início
    public void RemoveFirst()
    {
        if (Head == null)
        {
            Console.WriteLine("A lista está vazia. Nada para remover.");
            return;
        }
        Console.WriteLine($"Removendo {Head.Data} do início da lista.");
        Head = Head.Next;
    }

    // 4. Remover do Final
    public void RemoveLast()
    {
        if (Head == null)
        {
            Console.WriteLine("A lista está vazia. Nada para remover.");
            return;
        }

        if (Head.Next == null) // Apenas um nó na lista
        {
            Console.WriteLine($"Removendo {Head.Data} do final da lista (único nó). ");
            Head = null;
            return;
        }

        Node current = Head;
        while (current.Next.Next != null)
        {
            current = current.Next;
        }
        Console.WriteLine($"Removendo {current.Next.Data} do final da lista.");
        current.Next = null;
    }

    // 5. Buscar um elemento
    public bool Contains(int data)
    {
        Node current = Head;
        while (current != null)
        {
            if (current.Data == data)
            {
                Console.WriteLine($"Elemento {data} encontrado na lista.");
                return true;
            }
            current = current.Next;
        }
        Console.WriteLine($"Elemento {data} NÃO encontrado na lista.");
        return false;
    }

    // 6. Exibir todos os elementos
    public void DisplayList()
    {
        if (Head == null)
        {
            Console.WriteLine("A lista está vazia.");
            return;
        }

        Node current = Head;
        Console.Write("Lista: ");
        while (current != null)
        {
            Console.Write($"{current.Data} -> ");
            current = current.Next;
        }
        Console.WriteLine("null");
    }
}
```

**Explicação do Código `LinkedList.cs`:**

*   `public Node Head { get; private set; }`: Esta propriedade representa o primeiro nó da nossa lista. É o ponto de entrada para acessar todos os outros nós. `private set` significa que apenas métodos dentro da classe `LinkedList` podem modificar a `Head`.
*   `public LinkedList()`: O construtor da lista. Quando uma nova lista é criada, a `Head` é definida como `null`, indicando que a lista está vazia.

*   **`AddFirst(int data)` (Inserir no Início):**
    1.  `Node newNode = new Node(data);`: Cria um novo nó com o dado fornecido.
    2.  `newNode.Next = Head;`: O `Next` do novo nó aponta para o que era a `Head` antiga. Isso faz com que o novo nó se torne o primeiro.
    3.  `Head = newNode;`: Atualiza a `Head` da lista para ser o `newNode`.

*   **`AddLast(int data)` (Inserir no Final):**
    1.  `Node newNode = new Node(data);`: Cria um novo nó.
    2.  `if (Head == null)`: Se a lista estiver vazia, o novo nó se torna a `Head`.
    3.  `Node current = Head;`: Se não estiver vazia, começa a percorrer a lista a partir da `Head`.
    4.  `while (current.Next != null)`: Avança `current` até encontrar o último nó (aquele cujo `Next` é `null`).
    5.  `current.Next = newNode;`: O `Next` do último nó agora aponta para o `newNode`, adicionando-o ao final.

*   **`RemoveFirst()` (Remover do Início):**
    1.  `if (Head == null)`: Verifica se a lista está vazia. Se sim, não há nada para remover.
    2.  `Head = Head.Next;`: Simplesmente move a `Head` para o próximo nó. O antigo primeiro nó (que não tem mais nenhuma referência apontando para ele) será eventualmente coletado pelo coletor de lixo do C#.

*   **`RemoveLast()` (Remover do Final):**
    1.  `if (Head == null)`: Verifica se a lista está vazia.
    2.  `if (Head.Next == null)`: Se houver apenas um nó, a `Head` é definida como `null`, esvaziando a lista.
    3.  `Node current = Head;`: Começa a percorrer a lista.
    4.  `while (current.Next.Next != null)`: Avança `current` até que `current.Next` seja o último nó. Ou seja, `current` para no penúltimo nó.
    5.  `current.Next = null;`: O `Next` do penúltimo nó é definido como `null`, efetivamente removendo o último nó.

*   **`Contains(int data)` (Buscar um elemento):**
    1.  `Node current = Head;`: Começa a percorrer a lista.
    2.  `while (current != null)`: Continua enquanto houver nós para verificar.
    3.  `if (current.Data == data)`: Se o dado do nó atual for o que estamos buscando, retorna `true`.
    4.  `current = current.Next;`: Move para o próximo nó.
    5.  Se o loop terminar e o elemento não for encontrado, retorna `false`.

*   **`DisplayList()` (Exibir todos os elementos):**
    1.  `if (Head == null)`: Verifica se a lista está vazia.
    2.  `Node current = Head;`: Começa a percorrer a lista.
    3.  `while (current != null)`: Imprime o `Data` de cada nó e uma seta `->`.
    4.  `Console.WriteLine("null");`: Ao final, imprime `null` para indicar o fim da lista.

## Exemplo de Uso

Para ver a lista simplesmente encadeada em ação, você pode usar o seguinte código em um método `Main`:

```csharp
// Program.cs

using System;

public class Program
{
    public static void Main(string[] args)
    {
        LinkedList myList = new LinkedList();

        myList.DisplayList(); // Lista vazia

        myList.AddFirst(10);
        myList.AddFirst(20);
        myList.AddLast(30);
        myList.AddFirst(5);
        myList.AddLast(40);

        myList.DisplayList(); // 5 -> 20 -> 10 -> 30 -> 40 -> null

        myList.RemoveFirst();
        myList.DisplayList(); // 20 -> 10 -> 30 -> 40 -> null

        myList.RemoveLast();
        myList.DisplayList(); // 20 -> 10 -> 30 -> null

        myList.Contains(10); // Elemento 10 encontrado na lista.
        myList.Contains(99); // Elemento 99 NÃO encontrado na lista.

        myList.RemoveLast();
        myList.RemoveLast();
        myList.RemoveLast();
        myList.DisplayList(); // A lista está vazia.

        myList.RemoveFirst(); // A lista está vazia. Nada para remover.
    }
}
```

## Glossário de Termos

*   **Nó (Node)**: Um elemento individual em uma estrutura de dados encadeada, contendo dados e uma referência para o próximo elemento.
*   **Dado (Data)**: O valor armazenado dentro de um nó.
*   **Próximo (Next)**: Uma referência (ponteiro) que aponta para o próximo nó na sequência.
*   **Cabeça (Head)**: O primeiro nó de uma lista encadeada, usado como ponto de partida para acessar a lista.
*   **Ponteiro/Referência**: Uma variável que armazena o endereço de memória de outro elemento, permitindo a conexão entre nós.
*   **Null**: Um valor que indica a ausência de uma referência, geralmente usado para marcar o fim de uma lista encadeada.
*   **Alocação Dinâmica de Memória**: A capacidade de uma estrutura de dados de ajustar seu tamanho durante a execução do programa, alocando ou desalocando memória conforme necessário.
*   **Complexidade de Tempo O(1)**: Significa que o tempo de execução de uma operação é constante, independentemente do tamanho da entrada.
*   **Complexidade de Tempo O(n)**: Significa que o tempo de execução de uma operação cresce linearmente com o tamanho da entrada (n).

## Referências

*   [GeeksforGeeks - Singly Linked List](https://www.geeksforgeeks.org/data-structures-singly-linked-list/)
*   [Microsoft Learn - Listas Encadeadas](https://learn.microsoft.com/pt-br/dotnet/csharp/programming-guide/concepts/collections/linked-lists)
