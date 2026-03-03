# Matriz Esparsa

## Introdução

Uma **Matriz Esparsa** é uma matriz (ou array bidimensional) na qual a maioria dos elementos são zero. Em contraste, uma matriz densa é aquela em que a maioria dos elementos são não-zero. Quando trabalhamos com matrizes muito grandes que contêm predominantemente zeros, armazenar todos esses zeros na memória de forma tradicional (como um array bidimensional comum) seria um desperdício significativo de espaço e, muitas vezes, de tempo de processamento.

Imagine uma tabela gigante onde quase todas as células estão vazias ou contêm o número zero. Seria ineficiente guardar todas essas células vazias. Em vez disso, a ideia da matriz esparsa é armazenar apenas os valores que são diferentes de zero, juntamente com suas posições (linha e coluna). Isso economiza memória e pode otimizar certas operações.

## Conceitos Fundamentais

Para entender uma matriz esparsa, é importante conhecer os seguintes termos:

*   **Elemento Zero**: Um elemento na matriz cujo valor é zero.
*   **Elemento Não-Zero**: Um elemento na matriz cujo valor é diferente de zero.
*   **Densidade**: A proporção de elementos não-zero em relação ao número total de elementos na matriz. Uma matriz é considerada esparsa se sua densidade for baixa (geralmente menos de 30-50% de elementos não-zero).
*   **Representação Esparsa**: Métodos de armazenamento que visam economizar memória, guardando apenas os elementos não-zero.

## Como Funciona?

Em vez de armazenar todos os `M x N` elementos de uma matriz `M` por `N`, uma matriz esparsa armazena apenas os elementos não-zero. Existem várias maneiras de fazer isso, mas uma abordagem comum é armazenar cada elemento não-zero como uma tupla (linha, coluna, valor).

Por exemplo, se tivermos a matriz:

```
[[1, 0, 0],
 [0, 5, 0],
 [0, 0, 9]]
```

Em vez de armazenar nove elementos, poderíamos armazenar apenas:

*   (0, 0, 1) - (linha 0, coluna 0, valor 1)
*   (1, 1, 5) - (linha 1, coluna 1, valor 5)
*   (2, 2, 9) - (linha 2, coluna 2, valor 9)

Isso reduz drasticamente o espaço de armazenamento necessário para matrizes muito grandes e esparsas.

## Vantagens e Desvantagens

### Vantagens

*   **Economia de Memória**: A principal vantagem é a redução significativa no uso de memória para matrizes com muitos zeros.
*   **Eficiência Computacional**: Operações que envolvem a matriz (como multiplicação) podem ser mais rápidas, pois não precisam processar os muitos elementos zero.

### Desvantagens

*   **Complexidade de Implementação**: A lógica para acessar, inserir ou modificar elementos é mais complexa do que em uma matriz densa, pois é preciso gerenciar a estrutura de armazenamento dos elementos não-zero.
*   **Overhead para Matrizes Densas**: Se a matriz não for realmente esparsa (tiver muitos elementos não-zero), a representação esparsa pode, na verdade, consumir mais memória devido ao armazenamento das coordenadas de cada elemento.

## Implementação em C#

Vamos implementar uma matriz esparsa em C# usando uma lista de objetos que representam os elementos não-zero. Cada objeto armazenará a linha, a coluna e o valor do elemento.

### Classe `SparseMatrixElement`

Primeiro, definimos uma classe para representar um único elemento não-zero da matriz.

```csharp
// SparseMatrixElement.cs

using System;

public class SparseMatrixElement
{
    public int Row { get; set; }
    public int Col { get; set; }
    public int Value { get; set; }

    public SparseMatrixElement(int row, int col, int value)
    {
        Row = row;
        Col = col;
        Value = value;
    }
}
```

**Explicação do Código `SparseMatrixElement.cs`:**

*   `public int Row { get; set; }`: Armazena o índice da linha do elemento.
*   `public int Col { get; set; }`: Armazena o índice da coluna do elemento.
*   `public int Value { get; set; }`: Armazena o valor do elemento (que deve ser não-zero).
*   `public SparseMatrixElement(int row, int col, int value)`: Construtor para inicializar um novo elemento com sua posição e valor.

### Classe `SparseMatrix`

Agora, vamos criar a classe `SparseMatrix` que gerenciará a coleção de `SparseMatrixElement`.

```csharp
// SparseMatrix.cs

using System;
using System.Collections.Generic;
using System.Linq;

public class SparseMatrix
{
    private int rows; // Número total de linhas da matriz
    private int cols; // Número total de colunas da matriz
    private List<SparseMatrixElement> elements; // Lista de elementos não-zero

    public SparseMatrix(int rows, int cols)
    {
        if (rows <= 0 || cols <= 0)
        {
            throw new ArgumentException("O número de linhas e colunas deve ser positivo.");
        }
        this.rows = rows;
        this.cols = cols;
        this.elements = new List<SparseMatrixElement>();
    }

    // 1. Definir um elemento na matriz
    public void SetElement(int row, int col, int value)
    {
        if (row < 0 || row >= rows || col < 0 || col >= cols)
        {
            throw new IndexOutOfRangeException("Índices de linha ou coluna fora dos limites da matriz.");
        }

        // Procura se o elemento já existe
        SparseMatrixElement existingElement = elements.FirstOrDefault(e => e.Row == row && e.Col == col);

        if (value == 0) // Se o valor a ser definido é zero
        {
            if (existingElement != null)
            {
                elements.Remove(existingElement); // Remove o elemento se ele existia e agora é zero
                Console.WriteLine($"Elemento ({row},{col}) removido (valor definido para 0).");
            }
        }
        else // Se o valor a ser definido é não-zero
        {
            if (existingElement != null)
            {
                existingElement.Value = value; // Atualiza o valor do elemento existente
                Console.WriteLine($"Elemento ({row},{col}) atualizado para {value}.");
            }
            else
            {
                elements.Add(new SparseMatrixElement(row, col, value)); // Adiciona um novo elemento
                Console.WriteLine($"Elemento ({row},{col}) adicionado com valor {value}.");
            }
        }
    }

    // 2. Obter o valor de um elemento na matriz
    public int GetElement(int row, int col)
    {
        if (row < 0 || row >= rows || col < 0 || col >= cols)
        {
            throw new IndexOutOfRangeException("Índices de linha ou coluna fora dos limites da matriz.");
        }

        SparseMatrixElement element = elements.FirstOrDefault(e => e.Row == row && e.Col == col);
        return element?.Value ?? 0; // Retorna o valor se encontrado, caso contrário, 0
    }

    // 3. Exibir a matriz completa (com zeros)
    public void DisplayMatrix()
    {
        Console.WriteLine($"Matriz Esparsa ({rows}x{cols}):");
        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < cols; j++)
            {
                Console.Write($"{GetElement(i, j),4}"); // Formata para ocupar 4 espaços
            }
            Console.WriteLine();
        }
        Console.WriteLine();
    }

    // 4. Exibir apenas os elementos não-zero (para depuração)
    public void DisplayNonZeroElements()
    {
        Console.WriteLine("Elementos Não-Zero:");
        if (!elements.Any())
        {
            Console.WriteLine("Nenhum elemento não-zero.");
            return;
        }
        foreach (var element in elements)
        {
            Console.WriteLine($"  Linha: {element.Row}, Coluna: {element.Col}, Valor: {element.Value}");
        }
        Console.WriteLine();
    }
}
```

**Explicação do Código `SparseMatrix.cs`:**

*   `private int rows;`, `private int cols;`: Armazenam as dimensões da matriz.
*   `private List<SparseMatrixElement> elements;`: Esta é a lista onde armazenamos apenas os objetos `SparseMatrixElement` que representam os valores não-zero.
*   `public SparseMatrix(int rows, int cols)`: Construtor que inicializa as dimensões e a lista de elementos.

*   **`SetElement(int row, int col, int value)` (Definir Elemento):**
    1.  Verifica se os índices `row` e `col` estão dentro dos limites da matriz.
    2.  Usa `FirstOrDefault` para verificar se já existe um elemento na posição `(row, col)`.
    3.  **Se `value == 0`**: Se o valor a ser definido for zero, e um `existingElement` for encontrado, ele é removido da lista, pois não precisamos armazenar zeros.
    4.  **Se `value != 0`**: Se o valor for não-zero:
        *   Se um `existingElement` for encontrado, seu `Value` é atualizado.
        *   Caso contrário, um novo `SparseMatrixElement` é criado e adicionado à lista.

*   **`GetElement(int row, int col)` (Obter Elemento):**
    1.  Verifica os limites dos índices.
    2.  Procura um `SparseMatrixElement` na posição `(row, col)`.
    3.  Usa o operador `?.` (null-conditional) e `??` (null-coalescing) para retornar o `Value` do elemento se ele for encontrado, ou `0` se nenhum elemento não-zero for encontrado naquela posição (o que significa que o valor é zero).

*   **`DisplayMatrix()` (Exibir Matriz):**
    1.  Percorre todas as linhas e colunas da matriz lógica.
    2.  Para cada posição `(i, j)`, chama `GetElement(i, j)` para obter o valor (que será 0 se não houver um `SparseMatrixElement` correspondente).
    3.  Imprime o valor, formatando-o para que as colunas fiquem alinhadas.

*   **`DisplayNonZeroElements()` (Exibir Elementos Não-Zero):**
    1.  Itera sobre a lista `elements` e imprime os detalhes de cada elemento não-zero armazenado.

## Exemplo de Uso

Para ver a matriz esparsa em ação, você pode usar o seguinte código em um método `Main`:

```csharp
// Program.cs

using System;

public class Program
{
    public static void Main(string[] args)
    {
        // Cria uma matriz esparsa 3x3
        SparseMatrix matrix = new SparseMatrix(3, 3);

        matrix.DisplayMatrix(); // Matriz inicial (todos zeros)

        // Define alguns elementos não-zero
        matrix.SetElement(0, 0, 10);
        matrix.SetElement(1, 1, 20);
        matrix.SetElement(2, 2, 30);
        matrix.SetElement(0, 2, 5);

        matrix.DisplayMatrix();
        // Saída esperada:
        // Matriz Esparsa (3x3):
        //   10   0   5
        //    0  20   0
        //    0   0  30

        matrix.DisplayNonZeroElements();

        // Obtém um elemento
        Console.WriteLine($"Valor em (1,1): {matrix.GetElement(1, 1)}"); // Valor em (1,1): 20
        Console.WriteLine($"Valor em (0,1): {matrix.GetElement(0, 1)}"); // Valor em (0,1): 0

        // Atualiza um elemento existente
        matrix.SetElement(0, 0, 15);
        matrix.DisplayMatrix();
        // Saída esperada:
        // Matriz Esparsa (3x3):
        //   15   0   5
        //    0  20   0
        //    0   0  30

        // Define um elemento não-zero para zero (removendo-o)
        matrix.SetElement(0, 2, 0);
        matrix.DisplayMatrix();
        // Saída esperada:
        // Matriz Esparsa (3x3):
        //   15   0   0
        //    0  20   0
        //    0   0  30

        matrix.DisplayNonZeroElements();
    }
}
```

## Glossário de Termos

*   **Matriz Esparsa**: Uma matriz onde a maioria dos elementos são zero.
*   **Matriz Densa**: Uma matriz onde a maioria dos elementos são não-zero.
*   **Elemento Zero**: Um valor zero em uma matriz.
*   **Elemento Não-Zero**: Um valor diferente de zero em uma matriz.
*   **Densidade**: A proporção de elementos não-zero em uma matriz.
*   **Tupla**: Um conjunto ordenado de elementos, como (linha, coluna, valor).
*   **Overhead**: Custo adicional (de memória ou processamento) introduzido por uma determinada abordagem ou estrutura de dados.

## Referências

*   [GeeksforGeeks - Sparse Matrix Representation](https://www.geeksforgeeks.org/sparse-matrix-representation/)
*   [Wikipedia - Sparse matrix](https://en.wikipedia.org/wiki/Sparse_matrix)
