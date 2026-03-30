# Documentação Didática: Subtrator de 8 bits (Subtractor)

## Lógica da Subtração em Hardware

A maneira mais eficiente e didática de subtrair em eletrônica digital não é remover, mas sim somar o valor oposto. Para isso, utilizamos a lógica do **Complemento de 2**.

### O Conceito de Complemento de 2

Subtrair B de A é o mesmo que somar A com o valor negativo de B:
`A - B = A + (-B)`

Em binário de 8 bits:
1.  **Inversão**: Trocamos todos os 0s por 1s e 1s por 0s no operando B (Complemento de 1).
2.  **Soma**: Adicionamos 1 ao resultado da inversão.

```text
Exemplo: B = 5 (0000 0101)
~B = 1111 1010  (Invertido)
-B = 1111 1011  (Somado 1)
```

---

## Implementação Técnica: Hardware Reuse

Neste projeto, reutilizamos o **Somador de 8 bits** para realizar a subtração. A lógica é surpreendentemente simples:

-   **Inversores (NOT)**: Passamos o operando B por um bloco de portas NOT.
-   **Cin = 1**: No primeiro estágio do somador, fixamos o Carry In como 1 para realizar o "+ 1" do complemento de 2.

### Esquema de Blocos ASCII

```text
Entrada B [8 bits] ──► [ BLOCO NOT ] ──► ~B [8 bits] ────┐
                                                         ▼
Entrada A [8 bits] ───────────────────────────► [ SOMADOR DE 8 BITS ] ──► Resultado
                                                         ▲
Canal de Controle SEL = 001 ───────────────────► [Cin = 1]
```

---

## O Subtrator com Restauração (Restoring Subtractor)

Este componente especial (`subRestore1bit.dig`) é a base para o nosso algoritmo de divisão. Sua função é responder a uma pergunta: "A subtração de A - B foi positiva ou negativa?".

1.  **Tenta subtrair**: Ele realiza a operação `A - B`.
2.  **Verifica o Carry Out**: 
    -   Se `Cout = 1`: O resultado é positivo, mantém a subtração.
    -   Se `Cout = 0`: O resultado seria negativo, **restaura** o valor original de A.

### Fluxograma de Decisão (ASCII)

```text
      [ Início ]
          │
    [ Calcular A - B ]
          │
    [ O resultado é < 0? ] ─────► [ SIM ] ──► [ RESETA PARA VALOR A ]
          │                       (Restauração)
       [ NÃO ]
          │
[ MANTÉM O RESULTADO ]
```

---

## Casos de Uso e Verificação

### Teste de Operação: 10 - 4 = 6

| Operação | Representação Binária | Comentário |
| :--- | :--- | :--- |
| **Valor A** | `0000 1010` | (10) |
| **Inverso B** | `1111 1011` | (-4 via Complemento de 2) |
| **Resultado S** | `0000 0110` | **(6)** |

> [!WARNING]
> **Aviso de Sinal (Negativo)**
> Se o resultado da subtração for negativo, o bit mais significativo (MSB) será 1 e o Carry Out final será 0. A ALU possui uma flag `Neg` para sinalizar essa condição para o usuário.

> [!TIP]
> **Destaque de Entrega**
> Reutilizar o somador para subtrair economiza metade do espaço em um chip real, tornando a ALU mais compacta e eficiente.
