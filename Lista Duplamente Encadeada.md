# Lista Duplamente Encadeada

## Introdução

A **Lista Duplamente Encadeada** é uma variação da lista encadeada que, como o nome sugere, permite a travessia em ambas as direções: para frente e para trás. Em uma lista simplesmente encadeada, cada nó aponta apenas para o próximo nó. Na lista duplamente encadeada, cada **nó** contém uma referência para o **próximo** nó e também uma referência para o **nó anterior**.

Imagine uma fila de pessoas onde cada pessoa não só sabe quem está à sua frente, mas também quem está atrás dela. Isso facilita muito a movimentação e a localização, pois você pode ir e vir na fila sem precisar começar do início sempre. Essa flexibilidade é a principal vantagem das listas duplamente encadeadas.

## Conceitos Fundamentais

Os termos essenciais para entender uma lista duplamente encadeada são:

*   **Nó (Node)**: O elemento básico da lista. Cada nó possui três campos principais:
    *   **Dado (Data)**: O valor que o nó armazena.
    *   **Próximo (Next)**: Uma referência (ou ponteiro) para o próximo nó na sequência.
    *   **Anterior (Previous/Prev)**: Uma referência (ou ponteiro) para o nó anterior na sequência. O primeiro nó da lista terá seu campo `Anterior` apontando para `null`.

*   **Cabeça (Head)**: O primeiro nó da lista. Permite o acesso ao início da lista.

*   **Cauda (Tail)**: O último nó da lista. Permite o acesso ao final da lista. O último nó terá seu campo `Próximo` apontando para `null`.

## Como Funciona?

As operações em uma lista duplamente encadeada são semelhantes às da lista simplesmente encadeada, mas a presença do ponteiro `Anterior` simplifica algumas delas:

*   **Inserção**: Adicionar um novo nó (no início, no fim ou em uma posição específica).
*   **Remoção**: Excluir um nó da lista.
*   **Busca**: Encontrar um nó com um valor específico.
*   **Exibição/Travessia**: Percorrer todos os nós da lista (para frente ou para trás).

## Vantagens e Desvantagens

### Vantagens

*   **Travessia Bidirecional**: Permite percorrer a lista em ambas as direções, o que é útil para certas operações.
*   **Remoção Mais Eficiente**: A remoção de um nó é mais direta, pois, dado um nó, é fácil acessar seu nó anterior e ajustar os ponteiros. Em uma lista simplesmente encadeada, seria necessário percorrer a lista desde o início para encontrar o nó anterior.

### Desvantagens

*   **Maior Consumo de Memória**: Cada nó requer um ponteiro adicional (`Anterior`), o que aumenta o consumo de memória em comparação com a lista simplesmente encadeada.
*   **Operações Mais Complexas**: As operações de inserção e remoção são um pouco mais complexas, pois exigem a atualização de dois ponteiros (`Next` e `Previous`) em vez de apenas um.

## Implementação em C#

Vamos implementar uma lista duplamente encadeada em C#. Primeiro, definiremos a classe `Node` e depois a classe `DoublyLinkedList`.

### Classe `Node`

```csharp
// Node.cs

using System;

public class Node
{
    public int Data { get; set; } // O valor que o nó armazena
    public Node Next { get; set; } // Referência para o próximo nó
    public Node Previous { get; set; } // Referência para o nó anterior

    public Node(int data)
    {
        Data = data;
        Next = null; // Inicialmente, o próximo nó é nulo
        Previous = null; // Inicialmente, o nó anterior é nulo
    }
}
```

**Explicação do Código `Node.cs`:**

*   `public int Data { get; set; }`: Armazena o valor do nó.
*   `public Node Next { get; set; }`: Referência para o próximo nó, permitindo a travessia para frente.
*   `public Node Previous { get; set; }`: Referência para o nó anterior, permitindo a travessia para trás. Esta é a principal diferença em relação à lista simplesmente encadeada.
*   `public Node(int data)`: Construtor que inicializa o `Data` e define `Next` e `Previous` como `null`.

### Classe `DoublyLinkedList`

Agora, vamos criar a classe `DoublyLinkedList` que gerenciará os nós.

```csharp
// DoublyLinkedList.cs

using System;

public class DoublyLinkedList
{
    public Node Head { get; private set; } // O primeiro nó da lista
    public Node Tail { get; private set; } // O último nó da lista

    public DoublyLinkedList()
    {
        Head = null;
        Tail = null;
    }

    // 1. Inserir no Início
    public void AddFirst(int data)
    {
        Node newNode = new Node(data);
        if (Head == null)
        {
            Head = newNode;
            Tail = newNode;
        }
        else
        {
            newNode.Next = Head;
            Head.Previous = newNode;
            Head = newNode;
        }
        Console.WriteLine($"Adicionado {data} no início da lista.");
    }

    // 2. Inserir no Final
    public void AddLast(int data)
    {
        Node newNode = new Node(data);
        if (Tail == null)
        {
            Head = newNode;
            Tail = newNode;
        }
        else
        {
            newNode.Previous = Tail;
            Tail.Next = newNode;
            Tail = newNode;
        }
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
        if (Head != null)
        {
            Head.Previous = null;
        }
        else // A lista ficou vazia
        {
            Tail = null;
        }
    }

    // 4. Remover do Final
    public void RemoveLast()
    {
        if (Tail == null)
        {
            Console.WriteLine("A lista está vazia. Nada para remover.");
            return;
        }

        Console.WriteLine($"Removendo {Tail.Data} do final da lista.");
        Tail = Tail.Previous;
        if (Tail != null)
        {
            Tail.Next = null;
        }
        else // A lista ficou vazia
        {
            Head = null;
        }
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

    // 6. Exibir todos os elementos (para frente)
    public void DisplayForward()
    {
        if (Head == null)
        {
            Console.WriteLine("A lista está vazia.");
            return;
        }

        Node current = Head;
        Console.Write("Lista (Frente): ");
        while (current != null)
        {
            Console.Write($"{current.Data} <-> ");
            current = current.Next;
        }
        Console.WriteLine("null");
    }

    // 7. Exibir todos os elementos (para trás)
    public void DisplayBackward()
    {
        if (Tail == null)
        {
            Console.WriteLine("A lista está vazia.");
            return;
        }

        Node current = Tail;
        Console.Write("Lista (Trás): ");
        while (current != null)
        {
            Console.Write($"{current.Data} <-> ");
            current = current.Previous;
        }
        Console.WriteLine("null");
    }
}
```

**Explicação do Código `DoublyLinkedList.cs`:**

*   `public Node Head { get; private set; }` e `public Node Tail { get; private set; }`: Representam o primeiro e o último nó da lista, respectivamente. Ter acesso direto à `Tail` otimiza operações no final da lista.

*   **`AddFirst(int data)` (Inserir no Início):**
    1.  Cria um `newNode`.
    2.  Se a lista estiver vazia, `newNode` se torna `Head` e `Tail`.
    3.  Caso contrário, o `Next` do `newNode` aponta para a `Head` atual, o `Previous` da `Head` atual aponta para o `newNode`, e então `Head` é atualizada para `newNode`.

*   **`AddLast(int data)` (Inserir no Final):**
    1.  Cria um `newNode`.
    2.  Se a lista estiver vazia, `newNode` se torna `Head` e `Tail`.
    3.  Caso contrário, o `Previous` do `newNode` aponta para a `Tail` atual, o `Next` da `Tail` atual aponta para o `newNode`, e então `Tail` é atualizada para `newNode`.

*   **`RemoveFirst()` (Remover do Início):**
    1.  Verifica se a lista está vazia.
    2.  Atualiza `Head` para `Head.Next`.
    3.  Se a nova `Head` não for `null`, seu `Previous` é definido como `null`.
    4.  Se a lista ficar vazia após a remoção, `Tail` também é definida como `null`.

*   **`RemoveLast()` (Remover do Final):**
    1.  Verifica se a lista está vazia.
    2.  Atualiza `Tail` para `Tail.Previous`.
    3.  Se a nova `Tail` não for `null`, seu `Next` é definido como `null`.
    4.  Se a lista ficar vazia após a remoção, `Head` também é definida como `null`.

*   **`Contains(int data)` (Buscar um elemento):**
    *   Funciona de forma idêntica à lista simplesmente encadeada, percorrendo a lista a partir da `Head` usando o ponteiro `Next`.

*   **`DisplayForward()` (Exibir para frente):**
    *   Percorre a lista da `Head` até a `Tail` usando o ponteiro `Next`.

*   **`DisplayBackward()` (Exibir para trás):**
    *   Percorre a lista da `Tail` até a `Head` usando o ponteiro `Previous`. Esta é uma funcionalidade exclusiva das listas duplamente encadeadas.

## Exemplo de Uso

Para ver a lista duplamente encadeada em ação, você pode usar o seguinte código em um método `Main`:

```csharp
// Program.cs

using System;

public class Program
{
    public static void Main(string[] args)
    {
        DoublyLinkedList myList = new DoublyLinkedList();

        myList.DisplayForward(); // Lista vazia

        myList.AddFirst(10);
        myList.AddFirst(20);
        myList.AddLast(30);
        myList.AddFirst(5);
        myList.AddLast(40);

        myList.DisplayForward();  // 5 <-> 20 <-> 10 <-> 30 <-> 40 <-> null
        myList.DisplayBackward(); // 40 <-> 30 <-> 10 <-> 20 <-> 5 <-> null

        myList.RemoveFirst();
        myList.DisplayForward();  // 20 <-> 10 <-> 30 <-> 40 <-> null

        myList.RemoveLast();
        myList.DisplayForward();  // 20 <-> 10 <-> 30 <-> null

        myList.Contains(10); // Elemento 10 encontrado na lista.
        myList.Contains(99); // Elemento 99 NÃO encontrado na lista.

        myList.RemoveLast();
        myList.RemoveLast();
        myList.RemoveLast();
        myList.DisplayForward(); // A lista está vazia.

        myList.RemoveFirst(); // A lista está vazia. Nada para remover.
    }
}
```

## Glossário de Termos

*   **Nó (Node)**: Um elemento individual em uma estrutura de dados encadeada, contendo dados e referências para o próximo e o anterior elemento.
*   **Dado (Data)**: O valor armazenado dentro de um nó.
*   **Próximo (Next)**: Uma referência (ponteiro) que aponta para o próximo nó na sequência.
*   **Anterior (Previous/Prev)**: Uma referência (ponteiro) que aponta para o nó anterior na sequência.
*   **Cabeça (Head)**: O primeiro nó de uma lista encadeada, usado como ponto de partida para acessar o início da lista.
*   **Cauda (Tail)**: O último nó de uma lista encadeada, usado como ponto de partida para acessar o final da lista.
*   **Ponteiro/Referência**: Uma variável que armazena o endereço de memória de outro elemento, permitindo a conexão entre nós.
*   **Null**: Um valor que indica a ausência de uma referência, geralmente usado para marcar o fim ou o início de uma lista encadeada.
*   **Travessia Bidirecional**: A capacidade de percorrer uma estrutura de dados em ambas as direções (para frente e para trás).

## Referências

*   [GeeksforGeeks - Doubly Linked List](https://www.geeksforgeeks.org/data-structures-and-algorithms-doubly-linked-list/)
*   [Microsoft Learn - Listas Encadeadas](https://learn.microsoft.com/pt-br/dotnet/csharp/programming-guide/concepts/collections/linked-lists)
