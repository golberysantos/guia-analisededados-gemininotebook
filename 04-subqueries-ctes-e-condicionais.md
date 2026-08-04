# 📑 Módulo 4: Subconsultas, CTEs e Condicionais (Lógica e Legibilidade)

Este módulo aborda técnicas avançadas de estruturação e fluxo lógico em consultas SQL. À medida que as perguntas de negócios se tornam complexas, precisamos de ferramentas que nos permitam criar regras condicionais personalizadas e dividir problemas grandes em etapas menores, legíveis e fáceis de manter.

---

## 🎯 Por que a Lógica Avançada é Crucial para o Analista?

No ecossistema de análise de dados, o papel do analista vai muito além de extrair tabelas brutas; ele precisa moldar e categorizar dados sob demanda para gerar relatórios de impacto [1, 2]. Nem sempre as categorias que o negócio precisa (como "clientes de alto valor" ou "pedidos atrasados") existem prontas no banco de dados.

Dominar as estruturas condicionais (`CASE WHEN`) e os métodos de organização de consultas complexas — como as Subconsultas e as Expressões de Tabela Comum (CTEs) — é o diferencial que separa um executor de queries básico de um **analista de dados sênior** [3-5]. Essas ferramentas permitem que você resolva lógicas de negócio sofisticadas diretamente no banco de dados, garantindo eficiência, organização e, principalmente, a clareza necessária para que outros membros da equipe entendam o seu raciocínio [1, 5].

---

## 📖 Resumo Estruturado do Assunto

### 1. A Condicional `CASE WHEN`
O comando `CASE` permite embutir a lógica condicional do tipo "se-então-senão" (*if-then-else*) diretamente no seu `SELECT` [6]. Ele avalia uma série de condições e retorna um valor assim que a primeira condição verdadeira for atendida [4, 6]:

```sql
SELECT column_name,
  CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE result3
  END AS alias_name
FROM table_name;
```

*   **Aplicações comuns**: Classificar clientes por faixa de faturamento, calcular taxas de conversão específicas agrupando ações de usuários, ou converter códigos numéricos em categorias textuais compreensíveis [6, 7].

### 2. Subconsultas (Subqueries)
Uma subconsulta é simplesmente uma consulta interna (*inner query*) aninhada dentro de uma instrução SQL maior [8]. Ela pode ser classificada de três formas dependendo do seu retorno [5, 8]:
*   **Subconsulta Escalar (Scalar Subquery)**: Retorna um único valor individual (uma única linha e coluna) [8].
*   **Subconsulta de Coluna (Column Subquery)**: Retorna múltiplos valores dispostos em uma única coluna [5]. É comumente utilizada com operadores como `IN` ou `NOT IN` para filtragem horizontal [9].
*   **Subconsulta de Tabela (Table Subquery)**: Retorna uma estrutura completa de linhas e colunas que pode ser tratada temporariamente como uma tabela física dentro da cláusula `FROM` [5].

### 3. Expressões de Tabela Comum (CTEs - Common Table Expressions)
Definidas pela cláusula `WITH`, as CTEs criam conjuntos de resultados temporários nomeados que podem ser referenciados em comandos subsequentes (`SELECT`, `INSERT`, `UPDATE` ou `DELETE`) [8]. 

Elas servem como uma alternativa elegante e moderna às subconsultas tradicionais, oferecendo duas grandes vantagens [5]:
*   **Legibilidade**: Permitem escrever consultas complexas de cima para baixo de forma linear, em vez de aninhar queries indefinidamente (o que torna o código um "pesadelo" de manutenção) [5, 10].
*   **Modularidade**: Permitem que você divida o problema em pequenas etapas lógicas e teste cada bloco isoladamente antes de juntá-los [10].

---

## 📚 Glossário Conceitual do Módulo

*   **Lógica Condicional**: Estrutura lógica de controle que executa determinadas ações ou retorna valores específicos dependendo se uma condição é verdadeira ou falsa [6].
*   **Query Aninhada (Nested Query)**: Uma consulta SQL escrita dentro dos parênteses de outra query principal [8].
*   **CTE (Common Table Expression)**: Conjunto temporário de dados estruturados e nomeados para simplificar a organização de relatórios longos [5, 8].
*   **Tratamento de Precisão Decimal**: Técnica de multiplicar valores inteiros por `1.0` em divisões SQL para forçar o banco a retornar resultados decimais precisos em vez de arredondamentos grosseiros [8].

---

## 🧠 Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

### Cicatrizes de Aprendizado Comuns

#### 1. Esquecer de fechar o bloco `CASE` com a palavra-chave `END`
*   **O código que tentei rodar:**
    ```sql
    SELECT product_name,
           CASE WHEN total_inventory > 100 THEN 'Estoque Alto'
                ELSE 'Estoque Baixo'
    FROM product_data;
    ```
*   **🔴 Erro retornado no console do Workbench:**
    > `Error Code: 1064. You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'FROM product_data' at line 4`
*   **A Causa Técnica:** A estrutura `CASE` exige um delimitador explícito de encerramento. Se você não colocar a palavra-chave `END`, o banco de dados interpretará que a consulta ainda está dentro da condicional e falhará ao encontrar o `FROM` [4].
*   **A Solução Correta:**
    ```sql
    SELECT product_name,
           CASE WHEN total_inventory > 100 THEN 'Estoque Alto'
                ELSE 'Estoque Baixo'
           END AS status_estoque
    FROM product_data;
    ```

#### 2. Erros de Sintaxe ao Declarar Múltiplas CTEs
*   **O código que tentei rodar:**
    ```sql
    WITH primeira_cte AS (
      SELECT phone_number FROM confirmed_numbers
    ),
    WITH segunda_cte AS (
      SELECT phone_number FROM all_numbers
    )
    SELECT * FROM segunda_cte;
    ```
*   **🔴 Erro retornado no console do Workbench:**
    > `Error Code: 1064. You have an error in your SQL syntax... near 'WITH segunda_cte AS...'`
*   **A Causa Técnica:** Ao encadear múltiplas CTEs, a palavra-chave `WITH` deve ser declarada **apenas uma vez** no início. As CTEs subsequentes devem ser separadas apenas por vírgulas.
*   **A Solução Correta:**
    ```sql
    WITH primeira_cte AS (
      SELECT phone_number FROM confirmed_numbers
    ), -- Apenas a vírgula separa as tabelas temporárias
    segunda_cte AS (
      SELECT phone_number FROM all_numbers
    )
    SELECT * FROM segunda_cte;
    ```

---

## 🤖 Prompts Reutilizáveis para Revisão Técnica

Guarde estes modelos de prompts para utilizá-los sempre que precisar de suporte técnico no Gemini Notebook:

1.  **Para simulação de erros do console (Workbench):**  
    > *"Estou desenvolvendo uma lógica condicional no MySQL Workbench e meu código retornou o seguinte erro: **'[Cole a mensagem de erro exata aqui]'**. O meu script é este: **'[Cole seu script aqui]'**. Ajude-me a entender o que quebrou na sintaxe e como posso reestruturar a query corretamente."*

2.  **Para transformar Subconsultas complexas em CTEs legíveis:**  
    > *"Aja como um especialista em refatoração de código SQL. Eu tenho a seguinte consulta que utiliza subconsultas aninhadas e está difícil de ler e manter: **'[Cole seu script aqui]'**. Reescreva esta mesma consulta utilizando a estrutura de CTEs (WITH) para torná-la linear, modular e limpa."*

3.  **## 🛠️ Prompt Bônus: Para gerar novos exercícios personalizados**  
    > *"Aja como um gerador de testes técnicos de SQL para vagas de analista de dados júnior. Com base nos conceitos de **Subconsultas (Scalar/Table), CTEs e cláusula CASE**, gere um estudo de caso realista de uma empresa de tecnologia. Forneça a estrutura das tabelas em formato SQL, me faça 3 desafios práticos com níveis de dificuldade incrementais para eu resolver e oculte as respostas até que eu peça."*
