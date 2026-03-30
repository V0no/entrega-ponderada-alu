# Documentação Didática: Somador de 8 bits (Adder)

## Introdução à Soma Binária

A soma de números binários segue a mesma lógica do sistema decimal (base 10). Somamos os dígitos coluna por coluna, da direita para a esquerda. Quando a soma de uma coluna excede o valor máximo de 1 bit (0 ou 1), geramos um **transporte** (**Carry**) para a coluna seguinte.

### Comparação Aritmética (Exemplo)

| Decimal | Binário (Vertical) | Bits Individuais |
| :--- | :--- | :--- |
| **Sum** | `   1  (Carry)` | `C8 C7 C6 C5 C4 C3 C2 C1` |
| **A** | `  10` | `0  0  0  0  1  0  1  0` |
| **B** | `+  6` | `0  0  0  0  0  1  1  0` |
| **R** | `  16` | `0  0  0  1  0  0  0  0` |

---

## O Bloco Fundamental: Somador Completo (Full Adder)

Um somador de 8 bits é construído a partir de **8 somadores completos** individuais. Cada unidade de somador completo (`soma1bit.dig`) possui:

-   **Entradas**: Bit A, Bit B e Carry de Entrada (Cin).
-   **Saídas**: Resultado da Soma (S) e Carry de Saída (Cout).

### Lógica Interna (Equações)

```text
S    = A XOR B XOR Cin         (Resultado binário da coluna)
Cout = (A AND B) OR (Cin AND (A XOR B)) (Sinal de transporte)
```

> [!TIP]
> **Explicação Didática**
> Imagine que o `S` é o resultado que você anota na conta e o `Cout` é o famoso "vai-um" que você leva para a próxima casa decimal.

---

## Arquitetura: Ripple Carry Adder (soma8bits.dig)

Neste projeto, conectamos os 8 somadores em um formato de **cascata**. O Carry de Saída de um bit é ligado diretamente ao Carry de Entrada do bit seguinte.

### Esquema de Encapsulamento

```text
[0] ──► [FA 0] ──C1──► [FA 1] ──C2──► [FA n] ──C7──► [FA 7] ──► Cout(Flag)
         ▲  ▲           ▲  ▲           ▲  ▲           ▲  ▲
         A0 B0          A1 B1          An Bn          A7 B7
          │  │           │  │           │  │           │  │
          ▼  ▼           ▼  ▼           ▼  ▼           ▼  ▼
         [Soma 0]       [Soma 1]       [Soma n]       [Soma 7]
```

### Por que se chama "Ripple Carry"?
O termo "Ripple" (onda) significa que o sinal de transporte precisa "viajar" por todos os 8 estágios. O sistema só termina a conta quando o sinal do primeiro bit chega ao último.

---

## Verificação de Resultados

### Teste de Overflow
O bit extra (Carry Out) do último estágio (FA 7) é usado para ativar a **Flag de Overflow**. Isso indica que o resultado da soma excedeu 255 e não pode ser representado apenas com 8 bits.

| Operação | Resultado | Flag de Over |
| :--- | :--- | :--- |
| **127 + 1** | 128 | Não |
| **255 + 1** | 0 | **Sim** |

> [!IMPORTANT]
> **Conclusão para Atividade**
> Este somador é a base para quase todas as outras operações da ALU, incluindo a subtrataçào e a multiplicação. Sua confiabilidade é vital para o funcionamento de toda a unidade lógica.
