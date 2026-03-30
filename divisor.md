# Documentação Didática: Divisor de 8 bits (Divisor)

## O Desafio da Divisão em Hardware

A divisão binária é a operação de execução mais longa e complexa em uma ALU. Como não podemos dividir "de uma vez só", usamos o **Algoritmo de Divisão com Restauração** (Restoring Division), que mimetiza a divisão de "chave" que aprendemos na escola.

---

## O Coração do Algoritmo: Restauração

Diferente da soma ou multiplicação, a divisão binária funciona em **8 ciclos iterativos** (um para cada bit do dividendo). A cada ciclo, o circuito realiza uma escolha lógica:

1.  **Tenta Subtrair**: Subtraímos o Divisor do Dividendo parcial.
2.  **Verifica o Sinal**: 
    -   Se o resultado da subtração for **positivo** (A ≥ B), colocamos o bit `1` no Quociente.
    -   Se o resultado for **negativo** (A < B), colocamos `0` no Quociente e **restauramos** o valor original (desfazendo a subtração).

### Fluxograma do Ciclo de Restauração (ASCII)

```text
       [ Dividendo Parcial ]
                │
         [ SUBTRAIR (-) ] ◄─────── [ Divisor ]
                │
         [ O RESULTADO < 0? ]
          /            \
      [ SIM ]        [ NÃO ]
         │              │
 [ Bit Qi = 0 ]   [ Bit Qi = 1 ]
 [ RESTAURAR ]    [ MANTER SOMA ]
         └──────┬───────┘
                ▼
         [ PRÓXIMO BIT ]
```

---

## Estrutura Técnica: O Caminho de Dados

Para processar a divisão de 8 bits, o hardware é organizado em uma cascata de **8 módulos de restauração** (`subRestore1bit.dig`). Cada módulo processa um bit do dividendo e gera um bit do quociente.

### Dados de Entrada e Saída

-   **Dividendo (A)**: O número que será dividido (8 bits).
-   **Divisor (B)**: O número que divide (8 bits).
-   **Quociente (Q)**: O resultado da divisão (8 bits).
-   **Resto (R)**: O que sobrou da operação (8 bits).

| Componente | Função Didática |
| :--- | :--- |
| **Registradores** | Guardam o valor atual do dividendo a cada passo. |
| **Subtrator Restorer** | Decide se a divisão "cabe" naquele bit específico. |
| **Shift de Bit** | Move o dividendo para a próxima posição à esquerda. |

---

## Exemplo Prático: 13 ÷ 3 = 4 (Resto 1)

| Ciclo | Operação Parcial | Resultado Parcial | Quociente (Q) |
| :--- | :--- | :--- | :--- |
| **C1-C4** | Subtração < 0 | Valor original | `0000` |
| **C5** | Subtração > 0 | `13 - (3 x 4)` | `0100` |
| **Fim** | **Resultado: 4** | **Resto: 1** | **(Q=4, R=1)** |

> [!WARNING]
> **Divisão por Zero**
> Em circuitos reais, a divisão por zero gera um erro crítico de hardware. Nesta ALU, dependendo da implementação, o resultado pode congelar ou retornar um valor de overflow.

> [!TIP]
> **Dica Didática**
> O módulo `subRestore1bit.dig` é um exemplo perfeito de **Feedback Loop**: o resultado de uma operação (subtração) altera o estado do próprio dado para o próximo estágio.
