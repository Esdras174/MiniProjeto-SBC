# Mini-Projeto 1 — SBC de Regras IF-THEN
## Domínio: Triagem Médica Simples

[![Abrir no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1QRXUCJq7EeXu0uHrD10zYfZ759tuJHfj?usp=sharing)

Sistema Baseado em Conhecimento (SBC) que simula uma triagem médica inicial utilizando **10 regras IF-THEN** com encadeamento progressivo (forward chaining) e resolução de conflitos por **salience** e **NOT**.

---

## Descrição do Domínio

O sistema recebe sintomas de um paciente (febre, tosse, falta de ar, etc.) e, por meio de regras de inferência organizadas em 3 níveis de encadeamento, chega a uma **recomendação de conduta médica**:

- Repouso em casa
- Realizar teste COVID e isolamento
- Ir à UPA
- Ir ao hospital com urgência
- Chamar o SAMU imediatamente

---

## As 10 Regras em Linguagem Natural

### Nível 1 → 2: Sintomas → Hipóteses

| # | Regra | Salience |
|---|-------|----------|
| R1 | SE tem febre E tem tosse E tem dor no corpo → ENTÃO suspeita de gripe | 10 |
| R2 | SE tem febre E perdeu o olfato → ENTÃO suspeita de COVID-19 | 10 |
| R3 | SE tem febre alta E tem dor de garganta E NÃO tem tosse → ENTÃO suspeita de infecção bacteriana | 10 |
| R4 | SE tem dor no peito E tem falta de ar → ENTÃO risco cardiovascular | 10 |
| R5 | SE tem febre E NÃO tem falta de ar E NÃO tem dor no peito → ENTÃO quadro leve | 5 |

### Nível 2 → 3: Hipóteses → Recomendação

| # | Regra | Salience |
|---|-------|----------|
| R6 | SE suspeita de gripe E quadro leve → ENTÃO repouso em casa com analgésico | 8 |
| R7 | SE suspeita de COVID E NÃO tem falta de ar → ENTÃO realizar teste e isolamento | 9 |
| R8 | SE suspeita bacteriana → ENTÃO ir à UPA para avaliação com possível antibiótico | 9 |
| R9 | SE risco cardiovascular → ENTÃO CHAMAR SAMU (192) IMEDIATAMENTE | **100** |
| R10 | SE suspeita de COVID E tem falta de ar → ENTÃO ir ao hospital urgente | **50** |

---

## Estratégia de Resolução de Conflito

Duas estratégias são utilizadas:

1. **Salience (prioridade numérica):** As regras são ordenadas por `salience` decrescente antes de cada ciclo de inferência. Regras de emergência (R9 = 100, R10 = 50) sempre têm prioridade sobre regras de triagem comum.

2. **NOT (negação de fatos):** Algumas regras só disparam quando um determinado sintoma está **ausente** (ex: R3 exige `NOT tosse`; R7 exige `NOT falta_ar`). Isso evita conclusões contraditórias.

---

## Casos de Teste

### Caso 1 — Gripe Leve
**Entrada:** febre=✅ tosse=✅ dor_corpo=✅ falta_ar=❌ dor_peito=❌  
**Encadeamento:** R1 → R5 → R6  
**Saída:** *Repouso em casa, hidratação e analgésico*

### Caso 2 — COVID Grave
**Entrada:** febre=✅ perda_olfato=✅ falta_ar=✅  
**Encadeamento:** R2 → R10 (salience 50 tem prioridade sobre R7)  
**Saída:** *Ir ao HOSPITAL URGENTE - possível COVID grave*

### Caso 3 — Emergência Cardiovascular
**Entrada:** dor_peito=✅ falta_ar=✅  
**Encadeamento:** R4 → R9 (salience 100, prioridade máxima)  
**Saída:** *CHAMAR SAMU (192) IMEDIATAMENTE*

---

## Como Executar

1. Clique no badge **"Abrir no Colab"** acima ou [acesse diretamente aqui](https://colab.research.google.com/drive/1QRXUCJq7EeXu0uHrD10zYfZ759tuJHfj?usp=sharing)
2. Execute as células em ordem (Ctrl+Enter ou "Executar tudo")
3. Os 3 casos de teste já estão prontos nas células 2, 3 e 4
4. A última célula permite inserir sintomas interativamente

---

## Estrutura do Projeto

```
├── mini_projeto1_triagem_medica.ipynb   # Notebook principal (Colab)
└── README.md                             # Este arquivo
```

---

## Equipe

| Esdras Lucas Borges dos Santos |
| Luis Antônio Feliciano |
| Felipe José de Medeiros Melo |
| João Lucas Silva Acioli |

---

