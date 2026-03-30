# Documentação Didática: Deslocamento de Bits (Shift Lógico)

## O Conceito de Deslocamento (Shift)

O **deslocamento lógico** é a operação mais rápida em hardware para realizar multiplicações ou divisões por potências de 2 (2, 4, 8, etc.). Em vez de realizar cálculos complexos, o circuito apenas "move" os bits para o lado.

---

## 1. Deslocamento para Esquerda (Left Shift << 1)

Mover todos os bits uma casa para a esquerda equivale a **multiplicar o número por 2**.

-   O bit que estava na posição 7 (MSB) é descartado.
-   O bit que cai na posição 0 (LSB) é preenchido com **0**.

### Visualização Bit por Bit (ASCII)

```text
Entrada A:  [A7][A6][A5][A4][A3][A2][A1][A0]
              │   │   │   │   │   │   │   │
SAÍDA << 1: [A6][A5][A4][A3][A2][A1][A0][ 0]
```

**Exemplo: 3 << 1 = 6**
-   `0000 0011` (3) ─► Desloca ─► `0000 0110` (6)

---

## 2. Deslocamento para Direita (Right Shift >> 1)

Mover todos os bits uma casa para a direita equivale a **dividir o número por 2** (apenas a parte inteira).

-   O bit que estava na posição 0 (LSB) é descartado.
-   O bit que cai na posição 7 (MSB) é preenchido com **0**.

### Visualização Bit por Bit (ASCII)

```text
Entrada A:  [A7][A6][A5][A4][A3][A2][A1][A0]
              │   │   │   │   │   │   │   │
SAÍDA >> 1: [ 0][A7][A6][A5][A4][A3][A2][A1]
```

**Exemplo: 12 >> 1 = 6**
-   `0000 1100` (12) ─► Desloca ─► `0000 0110` (6)

---

## Implementação Técnica (Eficiência Máxima)

Diferente da soma ou multiplicação, os deslocamentos não exigem **nenhuma porta lógica**. Eles são implementados apenas com a **fiação**.

-   **Wiring**: O fio do pino A[6] é conectado diretamente ao pino de Saída[7].
-   **Splitters**: No simulador Digital, usamos divisores (Splitters) para reorganizar os caminhos dos bits de forma instantânea.

> [!TIP]
> **Destaque Didático**
> Por não usar portas lógicas, o Shift consome virtualmente zero transistores de processamento em comparação com um multiplicador completo.

> [!WARNING]
> **Atenção ao Overflow**
> Ao deslocar para a esquerda (`<<`), se o bit A[7] for `1`, ele será perdido. No hardware Real, isso ativa a flag de Overflow para alertar que a conta excedeu a capacidade de 8 bits.
