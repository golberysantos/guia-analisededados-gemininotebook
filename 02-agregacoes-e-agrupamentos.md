# 📑 Módulo 2: Agregações e Agrupamentos (Sumarização de Dados)

Este módulo aborda um dos pilares mais práticos do dia a dia de um analista de dados: a capacidade de consolidar grandes volumes de registros individuais em métricas resumidas e inteligíveis [2, 3]. Aqui, aprendemos a extrair indicadores-chave e a estruturar relatórios gerenciais diretamente do banco de dados [3].

---

## 🎯 Por que a Sumarização é Vital para o Analista?

Ao buscar responder à pergunta **"O que preciso para me tornar um bom analista de dados?"**, as fontes indicam que o profissional não deve apenas extrair dados brutos, mas sim **encontrar tendências, padrões e histórias por trás dos números** [4, 5]. 

No ambiente corporativo, os tomadores de decisão raramente precisam ver transações individuais; eles precisam de respostas agregadas (como faturamento mensal, média de vendas por região ou contagem de novos clientes) [4, 6]. Dominar as agregações e agrupamentos em SQL é o que permite ao analista **transformar dados brutos em relatórios estruturados e painéis (dashboards) de impacto** [3, 5]. Além disso, esses resumos matemáticos servem de base para análises estatísticas mais profundas [7].

---

## 📖 Resumo Estruturado do Assunto

### 1. Funções de Agregação Essenciais
As funções de agregação realizam um cálculo matemático sobre um conjunto de valores em uma coluna e retornam um único valor resumido [3]:
*   **`COUNT()`**: Conta o número total de linhas ou de registros não nulos em uma coluna [3, 8].
*   **`SUM()`**: Soma todos os valores numéricos de uma coluna específica [3, 8].
*   **`AVG()`**: Calcula a média aritmética dos valores de uma coluna [3, 9].
*   **`MIN()`**: Identifica o menor valor contido naquela coluna [3].
*   **`MAX()`**: Identifica o maior valor contido naquela coluna [3].

### 2. O Comando `GROUP BY` (Agrupamento)
Por padrão, as funções de agregação resumem a tabela inteira em uma única linha [8]. Para segmentar esse resumo por categorias (ex: faturamento *por região*, média de preço *por categoria*), utilizamos a cláusula **`GROUP BY`** [3, 10].
*   O `GROUP BY` instrui o banco de dados a separar as linhas da tabela em grupos com base nos valores de uma ou mais colunas especificadas, aplicando a função de agregação de forma independente para cada grupo [3, 10].

### 3. Filtrando Agregações: `WHERE` vs. `HAVING`
Uma das maiores confusões na escrita de consultas SQL é a diferença entre filtrar linhas e filtrar grupos [3]:
*   **`WHERE`**: Aplica um filtro **antes** que qualquer agrupamento ou agregação seja realizada [3, 11]. Ele atua sobre linhas individuais da tabela física.
*   **`HAVING`**: Aplica um filtro **depois** que o agrupamento (`GROUP BY`) e as agregações foram processados [3]. Ele é usado de forma exclusiva para filtrar grupos com base nos resultados das funções agregadas (ex: mostrar apenas categorias cuja média de vendas seja superior a 1000) [3].

---

## 📚 Glossário Conceitual do Módulo

*   **Sumarização (Summarization)**: O ato de condensar múltiplos pontos de dados em estatísticas descritivas unificadas para facilitar a interpretação [2, 3].
*   **Agrupamento (Grouping)**: Processo de organizar registros de um conjunto de dados em subgrupos homogêneos com base em atributos comuns [3, 10].
*   **Tendência Central**: Conceito estatístico que indica o ponto em torno do qual os dados se distribuem (médias e medianas são exemplos fundamentais para a análise de resumos) [7].
*   **Filtro Pós-Agregação**: Filtragem realizada após o cálculo das métricas de grupo, executada exclusivamente pela cláusula `HAVING` no SQL [3].

---

## 🧠 Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

Nesta seção, registro os erros reais de compilação obtidos no console do **MySQL Workbench** ao praticar as consultas deste módulo, seguidos pelas instruções estratégicas de correção enviadas à IA.

---

### Cicatriz 1: O clássico uso do `WHERE` para filtrar agregações
Tentei filtrar categorias cuja média de preço fosse superior a 50 utilizando o `WHERE` tradicional.

*   **❌ O código que tentei rodar:**
    ```sql
    SELECT category, AVG(price) 
    FROM products 
    WHERE AVG(price) > 50 
    GROUP BY category;
    ```

*   **🔴 Erro retornado no console do Workbench:**
    > `Error Code: 1111. Invalid use of group function`

*   **🤖 Prompt de Socorro enviado ao Gemini Notebook:**
    > *"Estou usando o MySQL Workbench e minha query deu o seguinte erro: **'Error Code: 1111. Invalid use of group function'**. O código que tentei rodar foi esse abaixo. Por que o banco está reclamando dessa função de grupo no WHERE e como eu corrijo isso usando o HAVING?*
    > ```sql
    > SELECT category, AVG(price) FROM products WHERE AVG(price) > 50 GROUP BY category;
    > ```"

*   **✅ Diagnóstico e Solução:**  
    O `WHERE` é executado antes do agrupamento e não aceita funções agregadas. A solução é mover o filtro de média para a cláusula `HAVING`, que é executada após a consolidação dos grupos [3].
    ```sql
    SELECT category, AVG(price) 
    FROM products 
    GROUP BY category
    HAVING AVG(price) > 50;
    ```

---

### Cicatriz 2: O erro de colunas órfãs (Falta de colunas no `GROUP BY`)
Tentei selecionar a região e o ID do produto ao lado do total de vendas, mas esqueci de agrupar por uma das colunas descritivas.

*   **❌ O código que tentei rodar:**
    ```sql
    SELECT region, product_id, SUM(total_sales) 
    FROM product_data 
    GROUP BY region;
    ```

*   **🔴 Erro retornado no console do Workbench:**
    > `Error Code: 1055. Expression #2 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'db.product_data.product_id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by`

*   **🤖 Prompt de Socorro enviado ao Gemini Notebook:**
    > *"Socorro! O MySQL Workbench retornou este erro horrível: **'Error Code: 1055. Expression #2 of SELECT list is not in GROUP BY clause and contains nonaggregated column...'**. Minha query está abaixo. Pode me explicar de forma didática por que o banco se recusa a rodar isso por causa do `product_id` e como eu ajusto o código de forma correta?*
    > ```sql
    > SELECT region, product_id, SUM(total_sales) FROM product_data GROUP BY region;
    > ```"

*   **✅ Diagnóstico e Solução:**  
    O banco de dados não consegue decidir qual `product_id` exibir para cada linha de região resultante, pois há vários produtos para uma única região. Para corrigir, toda coluna que não for uma agregação matemática no `SELECT` deve obrigatoriamente constar na cláusula `GROUP BY`.
    ```sql
    SELECT region, product_id, SUM(total_sales) 
    FROM product_data 
    GROUP BY region, product_id;
    ```

---

### Cicatriz 3: Performance e Travamento ao Agregar Bases Gigantes
*   **O Problema prático:** Ao tentar rodar queries de agregação complexas em uma base volumosa, o Workbench travou e a consulta estourou o limite de tempo (*timeout*).
*   **Dica de Performance das Fontes:** Conforme recomendado pelo roadmap de estudos, quando lidamos com agregações e funções de sumarização complexas, **devemos sempre começar o desenvolvimento utilizando conjuntos de dados menores** [12]. Isso acelera o processo de depuração e evita estresse de processamento na base de dados ativa [12].

---

## 🤖 Prompts Reutilizáveis para Revisão Técnica

Guarde estes modelos de prompts em seu caderno para utilizá-los no Gemini Notebook sempre que enfrentar problemas em projetos futuros:

1.  **Para depuração de erros genéricos de agregação:**  
    > *"Estou trabalhando no MySQL Workbench e recebi o seguinte código de erro: **'[Cole o código e mensagem de erro exata aqui]'**. A query que gerou esse problema foi esta: **'[Cole sua query aqui]'**. Explique o que causou o erro no banco e me dê a solução correta comentada."*
2.  **Para converter lógica de negócios em agrupamento:**  
    > *"Aja como um analista de dados sênior. Eu tenho uma tabela chamada `pedidos` com as colunas `status_pedido`, `canal_venda` e `valor_total`. Como eu posso escrever uma query que traga a contagem total de pedidos e o faturamento somado, agrupados por canal de venda, mas exibindo apenas os canais que faturaram mais de 15000 no total?"*
