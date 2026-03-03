# Lista Circular

## Introdução

A **Lista Circular** é uma variação da lista encadeada onde o último nó da lista aponta para o primeiro nó, formando um círculo. Diferente das listas simplesmente ou duplamente encadeadas, onde o último nó aponta para `null`, em uma lista circular não há `null` para indicar o fim da lista. Isso significa que você pode percorrer a lista a partir de qualquer nó e eventualmente retornar ao ponto de partida.

Imagine uma roda gigante: cada cabine (nó) está conectada à próxima, e a última cabine se conecta de volta à primeira, criando um ciclo contínuo. Não há um 
início ou fim óbvio, pois a estrutura é cíclica.

## Conceitos Fundamentais

Os termos chave para entender uma lista circular são:

*   **Nó (Node)**: O elemento básico da lista, contendo:
    *   **Dado (Data)**: O valor que o nó armazena.
    *   **Próximo (Next)**: Uma referência (ou ponteiro) para o próximo nó na sequência.

*   **Último (Last)**: Em vez de `Head`, muitas implementações de listas circulares mantêm uma referência ao **último nó**. Isso ocorre porque, a partir do último nó, é fácil acessar o primeiro nó (já que `Last.Next` aponta para o `Head`). Isso simplifica as operações de inserção e remoção no início e no fim da lista.

## Como Funciona?

As operações em uma lista circular exigem um cuidado extra para garantir que o ciclo seja mantido corretamente. A travessia da lista geralmente envolve parar quando o nó atual for igual ao nó inicial da travessia (ou quando o `Next` do nó atual for o `Head` se estivermos usando uma referência `Head`).

*   **Inserção**: Adicionar um novo nó (no início, no fim ou em uma posição específica).
*   **Remoção**: Excluir um nó da lista.
*   **Busca**: Encontrar um nó com um valor específico.
*   **Exibição/Travessia**: Percorrer todos os nós da lista, tomando cuidado para não entrar em um loop infinito.

## Vantagens e Desvantagens

### Vantagens

*   **Acesso Contínuo**: Permite o acesso contínuo aos elementos, pois não há um fim. Isso é útil em aplicações como filas de espera em sistemas operacionais ou jogos.
*   **Implementação de Filas e Buffers**: Pode ser usada para implementar filas de forma eficiente, onde o `Last` aponta para o `Head` para facilitar o acesso ao primeiro elemento.
*   **Início Arbitrário**: A travessia pode começar de qualquer nó na lista.

### Desvantagens

*   **Complexidade de Implementação**: As operações de inserção e remoção são mais complexas do que em listas simplesmente encadeadas, pois é preciso gerenciar o ciclo.
*   **Loop Infinito**: Se não for implementada corretamente, a travessia da lista pode resultar em um loop infinito, pois não há um `null` para indicar o fim.

## Implementação em C#

Vamos implementar uma lista circular em C#. Primeiro, definiremos a classe `Node` (que é a mesma da lista simplesmente encadeada) e depois a classe `CircularLinkedList`.

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
*   `public Node Next { get; set; }`: Referência para o próximo nó. Em uma lista circular, o `Next` do último nó apontará para o primeiro nó.
*   `public Node(int data)`: Construtor que inicializa o `Data` e define `Next` como `null`.

### Classe `CircularLinkedList`

Agora, vamos criar a classe `CircularLinkedList` que gerenciará os nós.

```csharp
// CircularLinkedList.cs

using System;

public class CircularLinkedList
{
    public Node Last { get; private set; } // Referência para o último nó da lista

    public CircularLinkedList()
    {
        Last = null; // Inicialmente, a lista está vazia
    }

    // Verifica se a lista está vazia
    public bool IsEmpty()
    {
        return Last == null;
    }

    // 1. Inserir no Início
    public void AddFirst(int data)
    {
        Node newNode = new Node(data);
        if (IsEmpty())
        {
            Last = newNode;
            Last.Next = Last; // Aponta para si mesmo, formando o ciclo
        }
        else
        {
            newNode.Next = Last.Next; // Novo nó aponta para o antigo primeiro nó
            Last.Next = newNode;      // Último nó aponta para o novo nó
        }
        Console.WriteLine($"Adicionado {data} no início da lista.");
    }

    // 2. Inserir no Final
    public void AddLast(int data)
    {
        Node newNode = new Node(data);
        if (IsEmpty())
        {
            Last = newNode;
            Last.Next = Last; // Aponta para si mesmo, formando o ciclo
        }
        else
        {
            newNode.Next = Last.Next; // Novo nó aponta para o primeiro nó
            Last.Next = newNode;      // Último nó aponta para o novo nó
            Last = newNode;           // O novo nó se torna o último
        }
        Console.WriteLine($"Adicionado {data} no final da lista.");
    }

    // 3. Remover do Início
    public void RemoveFirst()
    {
        if (IsEmpty())
        {
            Console.WriteLine("A lista está vazia. Nada para remover.");
            return;
        }

        Console.WriteLine($"Removendo {Last.Next.Data} do início da lista.");
        if (Last.Next == Last) // Apenas um nó na lista
        {
            Last = null; // Esvazia a lista
        }
        else
        {
            Last.Next = Last.Next.Next; // O último nó aponta para o segundo nó, removendo o primeiro
        }
    }

    // 4. Remover do Final
    public void RemoveLast()
    {
        if (IsEmpty())
        {
            Console.WriteLine("A lista está vazia. Nada para remover.");
            return;
        }

        Console.WriteLine($"Removendo {Last.Data} do final da lista.");
        if (Last.Next == Last) // Apenas um nó na lista
        {
            Last = null; // Esvazia a lista
        }
        else
        {
            Node current = Last.Next; // Começa do primeiro nó
            while (current.Next != Last) // Percorre até o penúltimo nó
            {
                current = current.Next;
            }
            current.Next = Last.Next; // Penúltimo nó aponta para o primeiro
            Last = current;           // Penúltimo nó se torna o último
        }
    }

    // 5. Buscar um elemento
    public bool Contains(int data)
    {
        if (IsEmpty())
        {
            Console.WriteLine($"Elemento {data} NÃO encontrado na lista (lista vazia).");
            return false;
        }

        Node current = Last.Next; // Começa do primeiro nó
        do
        {
            if (current.Data == data)
            {
                Console.WriteLine($"Elemento {data} encontrado na lista.");
                return true;
            }
            current = current.Next;
        } while (current != Last.Next); // Continua até voltar ao primeiro nó

        Console.WriteLine($"Elemento {data} NÃO encontrado na lista.");
        return false;
    }

    // 6. Exibir todos os elementos
    public void DisplayList()
    {
        if (IsEmpty())
        {
            Console.WriteLine("A lista está vazia.");
            return;
        }

        Node current = Last.Next; // Começa do primeiro nó
        Console.Write("Lista Circular: ");
        do
        {
            Console.Write($"{current.Data} -> ");
            current = current.Next;
        } while (current != Last.Next); // Continua até voltar ao primeiro nó
        Console.WriteLine($"(volta para {Last.Next.Data})");
    }
}
```

**Explicação do Código `CircularLinkedList.cs`:**

*   `public Node Last { get; private set; }`: Em vez de `Head`, mantemos uma referência ao `Last` nó. Isso simplifica as operações de inserção e remoção no início e no fim, pois `Last.Next` sempre aponta para o primeiro nó.

*   **`IsEmpty()`**: Método auxiliar para verificar se a lista está vazia (`Last == null`).

*   **`AddFirst(int data)` (Inserir no Início):**
    1.  Cria um `newNode`.
    2.  Se a lista estiver vazia, o `newNode` se torna o `Last` e aponta para si mesmo, formando um ciclo de um único nó.
    3.  Caso contrário, o `Next` do `newNode` aponta para o que era o primeiro nó (`Last.Next`), e o `Next` do `Last` nó é atualizado para apontar para o `newNode`, inserindo-o no início.

*   **`AddLast(int data)` (Inserir no Final):**
    1.  Cria um `newNode`.
    2.  Se a lista estiver vazia, o `newNode` se torna o `Last` e aponta para si mesmo.
    3.  Caso contrário, o `Next` do `newNode` aponta para o que era o primeiro nó (`Last.Next`), o `Next` do `Last` nó é atualizado para apontar para o `newNode`, e então o `newNode` se torna o novo `Last`.

*   **`RemoveFirst()` (Remover do Início):**
    1.  Verifica se a lista está vazia.
    2.  Se houver apenas um nó (`Last.Next == Last`), a lista é esvaziada (`Last = null`).
    3.  Caso contrário, o `Next` do `Last` nó é atualizado para apontar para o segundo nó (`Last.Next.Next`), efetivamente removendo o primeiro nó.

*   **`RemoveLast()` (Remover do Final):**
    1.  Verifica se a lista está vazia.
    2.  Se houver apenas um nó, a lista é esvaziada.
    3.  Caso contrário, percorre a lista a partir do primeiro nó (`Last.Next`) até encontrar o penúltimo nó (`current.Next != Last`). O `Next` do penúltimo nó é então atualizado para apontar para o primeiro nó, e o penúltimo nó se torna o novo `Last`.

*   **`Contains(int data)` (Buscar um elemento):**
    1.  Verifica se a lista está vazia.
    2.  Começa a percorrer a lista a partir do primeiro nó (`Last.Next`).
    3.  Usa um loop `do-while` para garantir que pelo menos um nó seja verificado (mesmo que haja apenas um nó) e para parar quando o nó atual voltar a ser o primeiro nó.

*   **`DisplayList()` (Exibir todos os elementos):**
    1.  Verifica se a lista está vazia.
    2.  Começa a percorrer a lista a partir do primeiro nó (`Last.Next`).
    3.  Usa um loop `do-while` para imprimir os dados de cada nó e indicar o ciclo de volta ao primeiro nó.

## Exemplo de Uso

Para ver a lista circular em ação, você pode usar o seguinte código em um método `Main`:

```csharp
// Program.cs

using System;

public class Program
{
    public static void Main(string[] args)
    {
        CircularLinkedList myList = new CircularLinkedList();

        myList.DisplayList(); // Lista vazia

        myList.AddFirst(10);
        myList.AddFirst(20);
        myList.AddLast(30);
        myList.AddFirst(5);
        myList.AddLast(40);

        myList.DisplayList(); // Lista Circular: 5 -> 20 -> 10 -> 30 -> 40 -> (volta para 5)

        myList.RemoveFirst();
        myList.DisplayList(); // Lista Circular: 20 -> 10 -> 30 -> 40 -> (volta para 20)

        myList.RemoveLast();
        myList.DisplayList(); // Lista Circular: 20 -> 10 -> 30 -> (volta para 20)

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
*   **Último (Last)**: O último nó de uma lista circular, cujo ponteiro `Next` aponta para o primeiro nó da lista.
*   **Ponteiro/Referência**: Uma variável que armazena o endereço de memória de outro elemento, permitindo a conexão entre nós.
*   **Ciclo**: A característica de uma lista circular onde o último elemento aponta de volta para o primeiro, formando uma sequência contínua.
*   **Loop Infinito**: Uma condição em que um programa executa repetidamente um bloco de código sem uma condição de parada, o que pode ocorrer se a travessia de uma lista circular não for controlada corretamente.

## Referências

*   [GeeksforGeeks - Circular Linked List](https://www.geeksforgeeks.org/circular-linked-list/)
*   [Microsoft Learn - Listas Encadeadas](https://learn.microsoft.com/pt-br/dotnet/csharp/programming-guide/concepts/collections/linked-lists)
