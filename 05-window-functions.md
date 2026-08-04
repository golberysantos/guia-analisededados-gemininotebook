# 📑 Módulo 5: Funções de Janela (Window Functions) e Análises Avançadas

Este módulo aborda um dos recursos mais poderosos e sofisticados do SQL: as **Window Functions** (Funções de Janela) [1, 2]. Elas permitem realizar cálculos estatísticos e analíticos complexos em um conjunto de linhas que estão diretamente relacionadas à linha atual, sem a necessidade de reestruturar a query com cruzamentos redundantes (self-joins) ou agrupamentos que ocultem os detalhes dos registros individuais [1].

---

## 🎯 Por que as Window Functions definem um Analista Avançado?

Ao longo deste caderno, buscamos responder à pergunta motivadora: **"O que preciso para me tornar um bom analista de dados?"** [3]. As fontes indicam que, no mercado de trabalho de tecnologia e negócios, as análises mais exigentes dependem do domínio das funções de janela [4]. 

Dominar o uso de `OVER`, `PARTITION BY` e funções de ranking (como `ROW_NUMBER` e `DENSE_RANK`) é o diferencial técnico que permite ao analista realizar análises sofisticadas de comportamento — como calcular a evolução acumulada de vendas (*running totals*), médias móveis ou gerar rankings de performance dentro de setores [1, 4]. É o conhecimento de SQL avançado que eleva o nível técnico do portfólio de qualquer profissional de dados [5].

---

## 📖 Resumo Estruturado do Assunto

### 1. O Conceito de "Janela" (Window)
Diferente das funções de agregação tradicionais (que reduzem múltiplas linhas a um único resultado no `GROUP BY`) [1, 6], as **Window Functions** realizam cálculos para cada linha do conjunto de dados de forma individual, mantendo os detalhes e a identidade de cada registro [1]. A "janela" é o subconjunto de linhas que o banco de dados analisa para realizar o cálculo da linha atual [1].

### 2. A Sintaxe Essencial
A estrutura básica de uma função de janela utiliza a cláusula obrigatória **`OVER`**:

```sql
SELECT product_id,
       total_sales,
       SUM(total_sales) OVER (ORDER BY product_id) AS running_total
FROM product_data;
```

A cláusula `OVER` pode conter dois componentes fundamentais para controlar a janela:
*   **`PARTITION BY`**: Divide o conjunto de resultados em partições (grupos de linhas) e aplica a função separadamente para cada partição (ex: calcular totais de vendas separados por país ou região) [1].
*   **`ORDER BY`**: Define a ordem lógica em que as linhas dentro da partição serão processadas pela função de janela (crucial para cálculos acumulados e ordenação de rankings) [1].

### 3. Principais Funções de Janela
*   **`ROW_NUMBER()`**: Atribui um número inteiro sequencial e exclusivo para cada linha dentro de sua respectiva partição, com base na ordenação do `ORDER BY` [1].
*   **`RANK()`**: Atribui uma classificação (rank) para cada linha de forma ordenada [1]. Se houver empate em valores idênticos, as linhas recebem a mesma classificação, mas o próximo número da sequência é **ignorado/pulado** (ex: 1, 2, 2, 4) [1].
*   **`DENSE_RANK()`**: Funciona de forma semelhante ao `RANK()`, mas garante que os números de classificação sejam estritamente **consecutivos**, não pulando nenhum número em caso de empates (ex: 1, 2, 2, 3) [1].
*   **Agregações em Janela**: Funções como `SUM()`, `AVG()`, `MIN()` e `MAX()` podem ser utilizadas com `OVER` para criar acúmulos dinâmicos ao longo do tempo [1].

---

## 📚 Glossário Conceitual do Módulo

*   **Window Function (Função de Janela)**: Função SQL que executa cálculos em um conjunto de linhas que possuem relação direta com a linha atual [1].
*   **Partição (Partition)**: Subgrupo de dados criado temporariamente dentro de uma consulta pela cláusula `PARTITION BY` para aplicação isolada de cálculos analíticos [1].
*   **Running Total (Total Acumulado)**: Um cálculo que adiciona progressivamente o valor da linha atual à soma de todos os valores das linhas anteriores na mesma janela [1].
*   **Frame (Moldura de Janela)**: Especificação que delimita ainda mais a janela, indicando exatamente quais linhas antes ou depois da atual devem ser incluídas no cálculo.

---

## 🧠 Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

### Cicatrizes de Aprendizado Comuns

#### 1. Tentar filtrar funções de janela diretamente no `WHERE`
*   **O código que tentei rodar (Erro):**
    ```sql
    SELECT product_id, region,
           ROW_NUMBER() OVER (PARTITION BY region ORDER BY total_sales DESC) AS ranking
    FROM product_data
    WHERE ROW_NUMBER() OVER (PARTITION BY region ORDER BY total_sales DESC) <= 3; -- Erro de Sintaxe
    ```
*   **🔴 Erro retornado no console:**
    > `Error Code: 3593. Window function cannot be used in WHERE clause.`
*   **A Causa Técnica:** Na ordem lógica de execução do SQL, as funções de janela são computadas durante a fase de exibição do `SELECT`, que ocorre **depois** do filtro `WHERE`. Portanto, o `WHERE` não tem como filtrar um cálculo que ainda não foi realizado.
*   **A Solução Correta:** Devemos encapsular a consulta principal em uma subconsulta ou, preferencialmente, em uma CTE (Módulo 4), para filtrar o ranking gerado na etapa subsequente:
    ```sql
    WITH ranked_products AS (
      SELECT product_id, region, total_sales,
             ROW_NUMBER() OVER (PARTITION BY region ORDER BY total_sales DESC) AS ranking
      FROM product_data
    )
    SELECT product_id, region, total_sales, ranking
    FROM ranked_products
    WHERE ranking <= 3;
    ```

#### 2. Confundir `RANK()` com `DENSE_RANK()` em relatórios oficiais
*   **A Cicatriz:** Em um relatório de vendas, usei `RANK()` para classificar os melhores vendedores. Como houve um empate triplo em segundo lugar, o sistema classificou os próximos vendedores como 5º lugar (pulando o 3º e o 4º). Isso causou estranheza nos gestores de negócios, que acharam que faltavam dados.
*   **A Solução Correta:** Entender que o `DENSE_RANK()` mantém o ranking contínuo/consecutivo (1, 2, 2, 2, 3). Para exibições e premiações comerciais onde os empates não devem abrir lacunas na ordem de posições, prefira sempre o `DENSE_RANK()` [1].

---

## 🤖 Prompts Reutilizáveis para Revisão Técnica

1.  **Para solucionar erros de sintaxe em cálculos analíticos:**  
    > *"Estou tentando rodar uma consulta utilizando Window Functions no MySQL Workbench e recebi o seguinte erro: **'[Cole o erro retornado aqui]'**. O meu código atual é este: **'[Cole sua query aqui]'**. Explique o motivo do erro e corrija a sintaxe de forma didática."*

2.  **Para converter análises em cálculos móveis:**  
    > *"Aja como um Engenheiro de Dados especialista. Eu tenho uma tabela de faturamento diário. Como posso construir uma query em SQL que calcule uma média móvel de faturamento dos últimos 7 dias usando Window Functions e clausulamento ROWS BETWEEN?"*

3.  **## 🛠️ Prompt Bônus: Para gerar novos exercícios personalizados**  
    > *"Aja como um tutor de banco de dados focado em SQL avançado. Com base nos conceitos de **Window Functions (ROW_NUMBER, RANK, DENSE_RANK e agregações acumuladas com OVER)**, crie um cenário de negócios dinâmico. Forneça o script de criação da tabela de exemplo, crie 3 desafios práticos com graus de dificuldade progressivos para eu testar minhas habilidades e não mostre o gabarito até eu solicitar."*
