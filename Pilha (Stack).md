# Pilha (Stack)

## Introdução

A **Pilha (Stack)** é uma estrutura de dados linear que segue o princípio **LIFO (Last-In, First-Out)**, ou seja, o último elemento a ser inserido é o primeiro a ser removido. Pense em uma pilha de pratos: você sempre adiciona um prato no topo e sempre remove o prato do topo. O prato que foi colocado por último é o primeiro a ser retirado. Essa é a essência de uma pilha.

As pilhas são amplamente utilizadas em diversas áreas da computação, como em chamadas de funções (a última função chamada é a primeira a terminar), em desfazer/refazer operações em editores de texto, na avaliação de expressões matemáticas e em algoritmos de busca em grafos.

## Conceitos Fundamentais

Para entender uma pilha, é importante conhecer os seguintes termos:

*   **LIFO (Last-In, First-Out)**: O princípio fundamental de uma pilha. O último elemento adicionado é o primeiro a ser removido.

*   **Push (Empilhar)**: A operação de adicionar um novo elemento ao **topo** da pilha.

*   **Pop (Desempilhar)**: A operação de remover e retornar o elemento do **topo** da pilha.

*   **Peek (Espiar)**: A operação de retornar o elemento do **topo** da pilha sem removê-lo.

*   **Top (Topo)**: O elemento que está no topo da pilha e será o próximo a ser removido ou espiado.

## Como Funciona?

As operações básicas de uma pilha são:

*   **Push**: Adiciona um elemento ao topo da pilha.
*   **Pop**: Remove o elemento do topo da pilha.
*   **Peek**: Retorna o elemento do topo da pilha sem removê-lo.
*   **IsEmpty**: Verifica se a pilha está vazia.

## Vantagens e Desvantagens

### Vantagens

*   **Simplicidade**: O conceito LIFO é direto e fácil de implementar.
*   **Gerenciamento de Chamadas**: Ideal para gerenciar chamadas de funções em programas, garantindo que a execução retorne corretamente.
*   **Inversão de Ordem**: Útil para inverter a ordem de elementos ou processar dados em ordem inversa à sua chegada.

### Desvantagens

*   **Acesso Limitado**: O acesso aos elementos é restrito apenas ao topo da pilha. Não é possível acessar um elemento no meio da pilha diretamente sem remover os elementos acima dele.
*   **Overflow/Underflow**: Se implementada com um array de tamanho fixo, pode ocorrer *overflow* (pilha cheia) ao tentar adicionar elementos, ou *underflow* (pilha vazia) ao tentar remover elementos.

## Implementação em C#

Vamos implementar uma pilha em C# usando uma lista encadeada como base, o que permite um tamanho dinâmico. Primeiro, definiremos a classe `Node` (que é a mesma das listas encadeadas) e depois a classe `Stack`.

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

### Classe `Stack`

Agora, vamos criar a classe `Stack` que gerenciará os nós para implementar a pilha.

```csharp
// Stack.cs

using System;

public class Stack
{
    private Node top; // O nó no topo da pilha
    public int Count { get; private set; } // Número de elementos na pilha

    public Stack()
    {
        top = null;
        Count = 0;
    }

    // 1. Push (Empilhar): Adiciona um elemento ao topo da pilha
    public void Push(int data)
    {
        Node newNode = new Node(data);
        newNode.Next = top; // O novo nó aponta para o antigo topo
        top = newNode;      // O novo nó se torna o topo
        Count++;
        Console.WriteLine($"Empilhado: {data}");
    }

    // 2. Pop (Desempilhar): Remove e retorna o elemento do topo da pilha
    public int Pop()
    {
        if (IsEmpty())
        {
            throw new InvalidOperationException("A pilha está vazia. Não é possível desempilhar.");
        }

        int data = top.Data;
        top = top.Next; // O topo se move para o próximo nó
        Count--;
        Console.WriteLine($"Desempilhado: {data}");
        return data;
    }

    // 3. Peek (Espiar): Retorna o elemento do topo da pilha sem removê-lo
    public int Peek()
    {
        if (IsEmpty())
        {
            throw new InvalidOperationException("A pilha está vazia. Não há elementos para espiar.");
        }
        return top.Data;
    }

    // 4. IsEmpty: Verifica se a pilha está vazia
    public bool IsEmpty()
    {
        return top == null;
    }

    // 5. Exibir todos os elementos da pilha
    public void DisplayStack()
    {
        if (IsEmpty())
        {
            Console.WriteLine("A pilha está vazia.");
            return;
        }

        Node current = top;
        Console.Write("Pilha: [Topo] ");
        while (current != null)
        {
            Console.Write($"{current.Data} ");
            current = current.Next;
        }
        Console.WriteLine("[Base]");
    }
}
```

**Explicação do Código `Stack.cs`:**

*   `private Node top;`: `top` aponta para o elemento que está no topo da pilha.
*   `public int Count { get; private set; }`: Mantém o controle do número de elementos na pilha.

*   **`Push(int data)` (Empilhar):**
    1.  Cria um `newNode`.
    2.  O `Next` do `newNode` aponta para o que era o `top` atual. Isso coloca o `newNode` acima do antigo `top`.
    3.  O `top` é atualizado para ser o `newNode`.
    4.  `Count` é incrementado.

*   **`Pop()` (Desempilhar):**
    1.  Verifica se a pilha está vazia e lança uma exceção se for o caso.
    2.  Armazena o `Data` do `top` atual para retorno.
    3.  O `top` é movido para o próximo nó (`top.Next`), efetivamente removendo o antigo `top`.
    4.  `Count` é decrementado.
    5.  Retorna o `data` removido.

*   **`Peek()` (Espiar):**
    1.  Verifica se a pilha está vazia e lança uma exceção se for o caso.
    2.  Retorna o `Data` do `top` sem modificar a pilha.

*   **`IsEmpty()` (Está Vazia):**
    1.  Retorna `true` se `top` for `null`, indicando que a pilha não tem elementos.

*   **`DisplayStack()` (Exibir Pilha):**
    1.  Percorre a pilha do `top` até a base, imprimindo os dados de cada nó.

## Exemplo de Uso

Para ver a pilha em ação, você pode usar o seguinte código em um método `Main`:

```csharp
// Program.cs

using System;

public class Program
{
    public static void Main(string[] args)
    {
        Stack myStack = new Stack();

        myStack.DisplayStack(); // A pilha está vazia.

        myStack.Push(10);
        myStack.Push(20);
        myStack.Push(30);
        myStack.DisplayStack(); // Pilha: [Topo] 30 20 10 [Base]

        Console.WriteLine($"Elemento no topo: {myStack.Peek()}"); // Elemento no topo: 30

        myStack.Pop(); // Desempilhado: 30
        myStack.DisplayStack(); // Pilha: [Topo] 20 10 [Base]

        myStack.Push(40);
        myStack.DisplayStack(); // Pilha: [Topo] 40 20 10 [Base]

        myStack.Pop(); // Desempilhado: 40
        myStack.Pop(); // Desempilhado: 20
        myStack.DisplayStack(); // Pilha: [Topo] 10 [Base]

        myStack.Pop(); // Desempilhado: 10
        myStack.DisplayStack(); // A pilha está vazia.

        try
        {
            myStack.Pop(); // Tenta desempilhar de uma pilha vazia
        }
        catch (InvalidOperationException ex)
        {
            Console.WriteLine($"Erro: {ex.Message}"); // Erro: A pilha está vazia. Não é possível desempilhar.
        }
    }
}
}
```

## Glossário de Termos

*   **Pilha (Stack)**: Uma estrutura de dados linear que segue o princípio LIFO.
*   **LIFO (Last-In, First-Out)**: O último elemento a ser inserido é o primeiro a ser removido.
*   **Push (Empilhar)**: A operação de adicionar um elemento ao topo da pilha.
*   **Pop (Desempilhar)**: A operação de remover um elemento do topo da pilha.
*   **Peek (Espiar)**: Operação que retorna o elemento do topo da pilha sem removê-lo.
*   **Top (Topo)**: O elemento no topo da pilha, o próximo a ser removido ou espiado.
*   **Node (Nó)**: O elemento básico que compõe a pilha, contendo dados e uma referência para o próximo nó.
*   **Ponteiro/Referência**: Uma variável que armazena o endereço de memória de outro elemento, conectando os nós.
*   **Overflow**: Condição que ocorre quando se tenta adicionar um elemento a uma pilha que já está cheia (em implementações de tamanho fixo).
*   **Underflow**: Condição que ocorre quando se tenta remover um elemento de uma pilha que já está vazia.

## Referências

*   [GeeksforGeeks - Stack Data Structure](https://www.geeksforgeeks.org/stack-data-structure/)
*   [Microsoft Learn - Pilha (Stack) em C#](https://learn.microsoft.com/pt-br/dotnet/api/system.collections.stack?view=net-8.0)
