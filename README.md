# Documentação Técnica: ALU de 8 bits (Unidade Lógica e Aritmética)

> **OBS:** Qualquer dúvida que surgir, sobre a explicação ou sobre o projeto, pode me perguntar!    

## O que é uma ALU?

A **Unidade Lógica e Aritmética** (ALU) é o componente fundamental de um processador, responsável por executar operações matemáticas e decisões lógicas. Este projeto implementa uma ALU de 8 bits, capaz de processar dois números inteiros entre 0 e 255.

O funcionamento básico segue o esquema de barramento:
1.  **Entradas**: Dois operandos de 8 bits (A e B).
2.  **Controle**: Um sinal de 3 bits (**SEL**) que define qual operação será executada.
3.  **Saídas**: O resultado da operação (8 ou 16 bits) e as sinalizações (Flags).

### Tabela de Opcodes (Seleção de Operações)

Para cada combinação do sinal de controle `SEL`, a ALU ativa um circuito interno específico:

| SEL (Binário) | Operação Realizada | Tipo | Descrição Didática |
| :--- | :--- | :--- | :--- |
| **000** | Soma | Aritmético | Adição A + B com transporte de carry. |
| **001** | Subtração | Aritmético | A - B usando lógica de Complemento de 2. |
| **010** | Multiplicação | Aritmético | A × B gerando resultado de 16 bits. |
| **011** | Divisão | Aritmético | A ÷ B retornando Quociente e Resto. |
| **100** | Shift Esq. | Lógico | Desloca bits para esquerda (Multiplica por 2). |
| **101** | Shift Dir. | Lógico | Desloca bits para direita (Divide por 2). |
| **110** | NAND | Lógico | Inversão da porta AND (Universalidade). |
| **111** | XOR | Lógico | Comparação de diferença entre bits. |

---

## Estrutura Modular: A Hierarquia do Projeto

Este projeto foi construído de forma modular no simulador **Digital**. Cada arquivo possui uma função específica, permitindo a reutilização de componentes (ex: o somador é usado na multiplicação).

```text
PROJETO_ALU/
│
├── ALU8bits.dig (CÉREBRO - Contém o Multiplexador de seleção)
│
├── MÓDULOS ARITMÉTICOS
│   ├── adder.md (soma8bits.dig)
│   ├── subtractor.md (sub8bits.dig)
│   ├── multiplier.md (multiplicacao8bits.dig)
│   └── divisor.md (divisor8bits.dig)
│
└── MÓDULOS LÓGICOS
    ├── nand.md (nand8bits.dig)
    ├── xor.md (XOR8bits.dig)
    └── shift.md (Shift Estático)
```

### Visualização de Fluxo (Simplificada)

```text
Entrada A [8 bits] ───┐     ┌──────────────┐
                      ├────►│   MÓDULOS    │     ┌──────────────┐
Entrada B [8 bits] ───┤     │ DE OPERAÇÃO  ├────►│ MULTIPLEXADOR│──► Resultado
                      │     │  (EM PARALELO)│     │     8:1      │
Seletor SEL [3 bits] ─┼────►└──────────────┘     └──────────────┘
                      │                                 ▲
                      └─────────────────────────────────┘
```

> [!NOTE]
> **Didática de Simulação**
> No arquivo principal `ALU8bits.dig`, todas as operações acontecem simultaneamente quando A e B são inseridos. O Multiplexador age como uma "chave seletora" que escolhe apenas um dos caminhos para mostrar no visor final.

---

## Demonstração em Vídeo

Para ver o projeto funcionando em tempo real no simulador **Digital**, com explicações sobre cada módulo, assista ao vídeo no link abaixo:

> [**[ASSISTIR AO VÍDEO DE DEMONSTRAÇÃO]**](https://drive.google.com/file/d/1MWK_yahEo4eKdp4fBPRN2aA0hJhM9ukx/view?usp=sharing)
