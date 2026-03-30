# Documentação Didática: Multiplicador de 8 bits (Multiplier)

## Multiplicação Binária por Matriz (Array Multiplication)

A multiplicação binária em 8 bits é a operação mais complexa em termos de hardware, pois envolve o cruzamento de cada bit do primeiro número com cada bit do segundo, resultando em 64 produtos parciais que precisam ser somados.

---

## O Coração da Operação: Produtos Parciais (AND)

A primeira etapa consiste em usar 64 **portas lógicas AND** de 1 bit.
-   `1 AND 1 = 1`
-   `1 AND 0 = 0` / `0 AND 0 = 0`

Cada cruzamento `Ai · Bj` gera um bit que será posicionado em uma matriz de acúmulo, semelhante ao método de multiplicação que aprendemos na escola.

### Estrutura da Matriz (ASCII 4x4 - Exemplo)

```text
       A3 A2 A1 A0
    ×  B3 B2 B1 B0
    ───────────────
       P03 P02 P01 P00   (A × B0)
    P13 P12 P11 P10 (0)  (A × B1, shift)
 P23 P22 P21 P20 (0) (0) (A × B2, shift)
P33 P32 P31 P30 (0) (0) (0) (A × B3, shift)
```

**Em 8 bits, essa matriz se estende até 8 níveis vertically.**

---

## Hierarquia de Somadores (Acúmulo de Resultados)

Para somar esses 64 produtos parciais de forma eficiente, dividimos o hardware em duas camadas:

### 1. Camada de Meio-Somadores (HA - Half Adders)
Usada no primeiro nível de soma (quando somamos os primeiros dois produtos parciais). Como não há carry vindo de cima, um somador simples de 2 entradas é suficiente.
-   **Qtd**: 8 Unidades.

### 2. Camada de Somadores Completos (FA - Full Adders)
Usada nos níveis subsequentes (do 2º ao 8º produto parcial). Cada unidade precisa processar **3 sinais**:
-   Bit do produto atual.
-   Bit acumulado da linha superior.
-   Carry (transporte) da coluna à direita.
-   **Qtd**: 48 Unidades.

---

## Densidade de Hardware: Tabela de Atividade

| Componente | Função | Cálculo (N bits) | Quantidade (8 bits) |
| :--- | :--- | :--- | :--- |
| **Portas AND** | Multiplicação de 1 bit | `N²` | **64** |
| **Somadores (HA + FA)** | Acúmulo de bits | `N × (N-1)` | **56** |

### Por que o resultado tem 16 bits?
Ao multiplicarmos dois números de 8 bits (máx 255), o resultado pode chegar a 65.025. Esse valor requer obrigatoriamente 16 bits (`2^16 - 1 = 65535`) para ser representado sem perdas.

---

## Passo a Passo Didático: 3 × 2 = 6

| Etapa | Binário | Comentário |
| :--- | :--- | :--- |
| **A (3)** | `0000 0011` | Multiplicando |
| **B (2)** | `0000 0010` | Multiplicador |
| **PP0 (×0)** | `0000 0000` | (3 × 0) |
| **PP1 (×1)** | `0000 0011 0`| (3 × 1, deslocado 1 casa) |
| **SOMA** | `0000 0000 0000 0110` | **Resultado 16 bits: 6** |

> [!IMPORTANT]
> **Eficiência Térmica**
> Multiplicadores de matriz consomem muita energia e geram calor em processadores reais devido à enorme quantidade de portas lógicas (64 AND + 56 Somadores) operando em paralelo.

> [!TIP]
> **Dica Pro**
> O multiplicador deste projeto é o que chamamos de "Array Multiplier", uma arquitetura que prioriza a simplicidade de conexão sobre a velocidade máxima de processamento.
