# Documentação Didática: Porta Lógica XOR (Exclusive OR)

## A Lógica "OU Exclusivo"

A porta **XOR** (Exclusive OR) é um componente lógico fundamental que entrega o valor `1` somente quando as entradas são **diferentes**. Se as entradas forem iguais (ambas 0 ou ambas 1), o resultado será sempre `0`.

---

## Tabela Verdade (1 bit)

A porta XOR ignora a possibilidade de ambas as entradas serem verdadeiras para gerar um resultado ativo.

| Entrada A | Entrada B | Resultado OR | **Resultado XOR** |
| :--- | :--- | :--- | :--- |
| **0** | **0** | 0 | **0** |
| **0** | **1** | 1 | **1** |
| **1** | **0** | 1 | **1** |
| **1** | **1** | 1 | **0** (Aqui está a diferença!) |

### Equação Booleana
`A XOR B = A ⊕ B = (A · ~B) + (~A · B)`

---

## Aplicações Práticas ( didática )

### 1. Detecção de Igualdade
Se realizarmos um XOR entre dois números e o resultado for `0000 0000`, isso garante que os dois números são **exatamente iguais**.

### 2. Comparador de Bits
Útil para algoritmos que precisam identificar variações em fluxos de dados (ex: detecção de movimento em vídeo).

### 3. Criptografia Simétrica
Se você aplicar um XOR entre seu dado e uma "chave", o dado fica cifrado. Ao aplicar o XOR novamente com a mesma chave, você recupera o dado original.
-   `Dado ⊕ Chave = Cifrado`
-   `Cifrado ⊕ Chave = Dado`

---

## Implementação Técnica (8 bits)

No módulo `XOR8bits.dig`, a operação é aplicada de forma paralela via barramento.

```text
BARRAMENTO A [8 bits] ──► [ GRADE DE 8 XORs ] ──► RESULTADO [8 bits]
BARRAMENTO B [8 bits] ──► 
```

**Bit por Bit (Exemplo):**
-   A: `1 1 1 1 0 0 0 0` (0xF0)
-   B: `1 0 1 0 1 0 1 0` (0xAA)
-   **XOR**: `0 1 0 1 1 0 1 0` **(0x5A)**

> [!TIP]
> **Dica de Projeto**
> O XOR é a alma do somador (Adder) que vimos no começo desta documentação. Sem ele, o cálculo da soma binária exigiria muitas portas lógicas AND/OR extras.

> [!NOTE]
> **Destaque de Entrega**
> No arquivo `ALU8bits.dig`, o Seletor SEL=111 ativa o barramento XOR, enviando a comparação direta de A e B para a saída final.
