# Fila (Queue)

## Introdução

A **Fila (Queue)** é uma estrutura de dados linear que segue o princípio **FIFO (First-In, First-Out)**, ou seja, o primeiro elemento a ser inserido é o primeiro a ser removido. Pense em uma fila de banco ou de supermercado: a primeira pessoa a chegar é a primeira a ser atendida. Essa é a essência de uma fila.

As filas são amplamente utilizadas em sistemas operacionais (para gerenciar tarefas), em redes (para gerenciar pacotes de dados), em impressoras (para gerenciar trabalhos de impressão) e em muitas outras aplicações onde a ordem de processamento é crucial.

## Conceitos Fundamentais

Para entender uma fila, é importante conhecer os seguintes termos:

*   **FIFO (First-In, First-Out)**: O princípio fundamental de uma fila. O primeiro elemento adicionado é o primeiro a ser removido.

*   **Enqueue (Enfileirar)**: A operação de adicionar um novo elemento ao **final** da fila.

*   **Dequeue (Desenfileirar)**: A operação de remover um elemento do **início** da fila.

*   **Front (Frente)**: O elemento que está no início da fila e será o próximo a ser removido.

*   **Rear (Traseira)**: O elemento que está no final da fila e foi o último a ser adicionado.

## Como Funciona?

As operações básicas de uma fila são:

*   **Enqueue**: Adiciona um elemento à traseira da fila.
*   **Dequeue**: Remove o elemento da frente da fila.
*   **Peek/Front**: Retorna o elemento da frente da fila sem removê-lo.
*   **IsEmpty**: Verifica se a fila está vazia.

## Vantagens e Desvantagens

### Vantagens

*   **Ordem de Processamento Clara**: Garante que os elementos sejam processados na ordem em que foram adicionados, o que é essencial para muitas aplicações.
*   **Simplicidade**: O conceito FIFO é intuitivo e fácil de entender.
*   **Gerenciamento de Recursos**: Ideal para gerenciar recursos compartilhados, como impressoras ou acesso a bancos de dados, garantindo um acesso justo.

### Desvantagens

*   **Acesso Limitado**: Acesso aos elementos é restrito apenas à frente e à traseira da fila. Não é possível acessar um elemento no meio da fila diretamente sem remover os elementos anteriores.
*   **Tamanho Fixo (em implementações com array)**: Se implementada com um array de tamanho fixo, a fila pode ficar cheia, impedindo novas inserções, ou pode haver desperdício de espaço se o tamanho máximo for muito grande.

## Implementação em C#

Vamos implementar uma fila em C# usando uma lista encadeada como base, o que permite um tamanho dinâmico. Primeiro, definiremos a classe `Node` (que é a mesma das listas encadeadas) e depois a classe `Queue`.

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

*   `public int Data { get; set; }`: Armazena o valor do nó.
*   `public Node Next { get; set; }`: Referência para o próximo nó.
*   `public Node(int data)`: Construtor que inicializa o `Data` e define `Next` como `null`.

### Classe `Queue`

Agora, vamos criar a classe `Queue` que gerenciará os nós para implementar a fila.

```csharp
// Queue.cs

using System;

public class Queue
{
    private Node front; // O nó na frente da fila
    private Node rear;  // O nó na traseira da fila
    public int Count { get; private set; } // Número de elementos na fila

    public Queue()
    {
        front = null;
        rear = null;
        Count = 0;
    }

    // 1. Enqueue (Enfileirar): Adiciona um elemento ao final da fila
    public void Enqueue(int data)
    {
        Node newNode = new Node(data);
        if (IsEmpty())
        {
            front = newNode;
            rear = newNode;
        }
        else
        {
            rear.Next = newNode;
            rear = newNode;
        }
        Count++;
        Console.WriteLine($"Enfileirado: {data}");
    }

    // 2. Dequeue (Desenfileirar): Remove e retorna o elemento do início da fila
    public int Dequeue()
    {
        if (IsEmpty())
        {
            throw new InvalidOperationException("A fila está vazia. Não é possível desenfileirar.");
        }

        int data = front.Data;
        front = front.Next;
        if (front == null) // Se a fila ficou vazia após a remoção
        {
            rear = null;
        }
        Count--;
        Console.WriteLine($"Desenfileirado: {data}");
        return data;
    }

    // 3. Peek/Front: Retorna o elemento do início da fila sem removê-lo
    public int Peek()
    {
        if (IsEmpty())
        {
            throw new InvalidOperationException("A fila está vazia. Não há elementos para espiar.");
        }
        return front.Data;
    }

    // 4. IsEmpty: Verifica se a fila está vazia
    public bool IsEmpty()
    {
        return front == null;
    }

    // 5. Exibir todos os elementos da fila
    public void DisplayQueue()
    {
        if (IsEmpty())
        {
            Console.WriteLine("A fila está vazia.");
            return;
        }

        Node current = front;
        Console.Write("Fila: [Frente] ");
        while (current != null)
        {
            Console.Write($"{current.Data} ");
            current = current.Next;
        }
        Console.WriteLine("[Traseira]");
    }
}
```

**Explicação do Código `Queue.cs`:**

*   `private Node front;` e `private Node rear;`: `front` aponta para o primeiro elemento da fila (o próximo a sair), e `rear` aponta para o último elemento (onde novos elementos são adicionados).
*   `public int Count { get; private set; }`: Mantém o controle do número de elementos na fila.

*   **`Enqueue(int data)` (Enfileirar):**
    1.  Cria um `newNode`.
    2.  Se a fila estiver vazia, o `newNode` se torna tanto `front` quanto `rear`.
    3.  Caso contrário, o `Next` do `rear` atual aponta para o `newNode`, e então o `rear` é atualizado para ser o `newNode`.
    4.  `Count` é incrementado.

*   **`Dequeue()` (Desenfileirar):**
    1.  Verifica se a fila está vazia e lança uma exceção se for o caso.
    2.  Armazena o `Data` do `front` atual para retorno.
    3.  `front` é movido para o próximo nó (`front.Next`).
    4.  Se `front` se tornar `null` (indicando que a fila ficou vazia), `rear` também é definido como `null`.
    5.  `Count` é decrementado.
    6.  Retorna o `data` removido.

*   **`Peek()` (Espiar):**
    1.  Verifica se a fila está vazia e lança uma exceção se for o caso.
    2.  Retorna o `Data` do `front` sem modificar a fila.

*   **`IsEmpty()` (Está Vazia):**
    1.  Retorna `true` se `front` for `null`, indicando que a fila não tem elementos.

*   **`DisplayQueue()` (Exibir Fila):**
    1.  Percorre a fila do `front` até o `rear`, imprimindo os dados de cada nó.

## Exemplo de Uso

Para ver a fila em ação, você pode usar o seguinte código em um método `Main`:

```csharp
// Program.cs

using System;

public class Program
{
    public static void Main(string[] args)
    {
        Queue myQueue = new Queue();

        myQueue.DisplayQueue(); // A fila está vazia.

        myQueue.Enqueue(10);
        myQueue.Enqueue(20);
        myQueue.Enqueue(30);
        myQueue.DisplayQueue(); // Fila: [Frente] 10 20 30 [Traseira]

        Console.WriteLine($"Elemento na frente: {myQueue.Peek()}"); // Elemento na frente: 10

        myQueue.Dequeue(); // Desenfileirado: 10
        myQueue.DisplayQueue(); // Fila: [Frente] 20 30 [Traseira]

        myQueue.Enqueue(40);
        myQueue.DisplayQueue(); // Fila: [Frente] 20 30 40 [Traseira]

        myQueue.Dequeue(); // Desenfileirado: 20
        myQueue.Dequeue(); // Desenfileirado: 30
        myQueue.DisplayQueue(); // Fila: [Frente] 40 [Traseira]

        myQueue.Dequeue(); // Desenfileirado: 40
        myQueue.DisplayQueue(); // A fila está vazia.

        try
        {
            myQueue.Dequeue(); // Tenta desenfileirar de uma fila vazia
        }
        catch (InvalidOperationException ex)
        {
            Console.WriteLine($"Erro: {ex.Message}"); // Erro: A fila está vazia. Não é possível desenfileirar.
        }
    }
}
```

## Glossário de Termos

*   **Fila (Queue)**: Uma estrutura de dados linear que segue o princípio FIFO.
*   **FIFO (First-In, First-Out)**: O primeiro elemento a ser inserido é o primeiro a ser removido.
*   **Enqueue (Enfileirar)**: A operação de adicionar um elemento ao final da fila.
*   **Dequeue (Desenfileirar)**: A operação de remover um elemento do início da fila.
*   **Front (Frente)**: O elemento no início da fila, o próximo a ser removido.
*   **Rear (Traseira)**: O elemento no final da fila, o último a ser adicionado.
*   **Peek**: Operação que retorna o elemento da frente da fila sem removê-lo.
*   **Node (Nó)**: O elemento básico que compõe a fila, contendo dados e uma referência para o próximo nó.
*   **Ponteiro/Referência**: Uma variável que armazena o endereço de memória de outro elemento, conectando os nós.

## Referências

*   [GeeksforGeeks - Queue Data Structure](https://www.geeksforgeeks.org/queue-data-structure/)
*   [Microsoft Learn - Fila (Queue) em C#](https://learn.microsoft.com/pt-br/dotnet/api/system.collections.queue?view=net-8.0)
