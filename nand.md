# Documentação Didática: Porta Lógica NAND (NAND Gate)

## A Universalidade da Porta NAND

A porta **NAND** (Not AND) é considerada o componente mais versátil da lógica digital. Sua importância reside no fato de que **qualquer outra porta lógica** (AND, OR, NOT, XOR) pode ser construída utilizando apenas combinações de portas NAND.

---

## Lógica Interna (Tabela Verdade)

A porta NAND inverte o resultado de uma operação AND. O valor de saída só é `0` se **TODAS** as entradas forem `1`.

| Entrada A | Entrada B | Resultado AND | **Resultado NAND** |
| :--- | :--- | :--- | :--- |
| **0** | **0** | 0 | **1** |
| **0** | **1** | 0 | **1** |
| **1** | **0** | 0 | **1** |
| **1** | **1** | 1 | **0** |

### Equação Booleana
`A NAND B = NOT (A AND B) = ~(A · B)`

---

## A Universalidade na Prática

Por que usamos NAND para tudo em chips reais? Porque, do ponto de vista de fabricação física (transistores CMOS), a porta NAND é mais compacta e consome menos energia do que a porta AND ou OR.

### Como emular outras portas (ASCII)

```text
Porta NOT de A  = [ A NAND A ]

Porta AND de AB = [ (A NAND B) NAND (A NAND B) ]

Porta OR de AB  = [ (A NAND A) NAND (B NAND B) ]
```

---

## Implementação Técnica (8 bits)

No módulo `nand8bits.dig`, a operação é aplicada de forma **independente e paralela** para cada um dos 8 bits dos operandos A e B.

```text
BARRAMENTO A [8 bits] ──► [ GRADE DE 8 NANDs ] ──► RESULTADO [8 bits]
BARRAMENTO B [8 bits] ──► 
```

**Bit por Bit (Exemplo):**
-   A: `1 1 1 1 0 0 0 0`
-   B: `1 0 1 0 1 0 1 0`
-   **NAND**: `0 1 0 1 1 1 1 1`

> [!TIP]
> **Aplicação de Entrega**
> No projeto da ALU, o NAND SEL=110 desempenha o papel de operador universal, permitindo a execução de qualquer lógica complexa adicional via software.

> [!NOTE]
> **Delay de Propagação**
> Como não há carry entre as portas, a operação NAND é virtualmente instantânea na simulação da ALU.
