# 📑 Módulo 1: Fundamentos DQL (Data Query Language) e Recuperação de Dados

Este módulo aborda a base de análise de dados com SQL: a habilidade de consultar e extrair registros de um banco de dados de forma precisa [1]. Como primeiro passo do nosso roteiro de estudos, dominamos a sintaxe essencial que nos permite iniciar o processo de tradução de perguntas de negócios em dados estruturados [2, 3].

---

## 🎯 SQL e o papel do Analista de Dados

Para responder à pergunta motivadora deste caderno (**"O que preciso para me tornar um bom analista de dados?"**), as fontes deixam claro que o **SQL é o fundamento técnico básico e inegociável** [2, 4]. Ele funciona como o tradutor universal entre o pensamento humano e os sistemas de armazenamento de dados brutos [5]. 

Antes de criar gráficos visualmente atraentes ou rodar modelos complexos, o analista precisa extrair, limpar e preparar os dados [4, 5]. O domínio do SQL garante que você possa lidar diretamente com conjuntos de dados volumosos nos quais ferramentas tradicionais (como planilhas eletrônicas) não conseguem operar com performance [4, 6].

---

## 📖 Resumo Estruturado do Assunto

### 1. Classificação das Linguagens SQL
Embora o SQL seja visto de forma unificada, ele é categorizado de acordo com a finalidade de seus comandos [7]:
*   **DQL (Data Query Language):** Focada estritamente em realizar consultas e acessar os dados existentes (o comando central é o `SELECT`) [7, 8]. É a vertente mais utilizada no dia a dia da análise de dados [5, 8].
*   **DML (Data Manipulation Language):** Usada para controlar e modificar o conteúdo das tabelas (inserção, atualização e deleção com `INSERT`, `UPDATE` e `DELETE`) [7, 9].
*   **DDL (Data Definition Language):** Usada para criar, alterar e gerenciar a estrutura física do esquema do banco de dados (como as estruturas de tabelas através de `CREATE TABLE`, `ALTER TABLE` e `DROP TABLE`) [7-9].

### 2. A Sintaxe Fundamental da Consulta (Data Retrieval)
A estrutura mais básica e elementar de uma query em SQL é composta por três cláusulas principais [7]:

```sql
	SELECT nome_da_coluna1, nome_da_coluna2
	FROM nome_da_tabela
	WHERE condicao_de_filtragem;
```
* SELECT: Determina quais colunas ou campos você deseja que o banco de dados retorne. É aqui que especificamos as variáveis de interesse para nossa análise.
* FROM: Especifica de qual tabela física (ou conjunto de tabelas) o sistema deve extrair as informações solicitadas.
* WHERE: Funciona como um filtro horizontal dinâmico. Ele avalia linha por linha da tabela e retorna apenas aquelas que atendem aos critérios ou operadores lógicos estabelecidos.
3. Operadores de Filtragem Comuns
Na cláusula WHERE, utilizamos operadores lógicos e de comparação para selecionar dados granulares:
* Operadores de Comparação: =, != (ou <>), >, <, >=, <=.
* Operadores Lógicos: AND (todas as condições devem ser verdadeiras), OR (pelo menos uma deve ser verdadeira) e NOT (negação de uma condição).
* Filtros de Padrão (Strings): LIKE combinado com caracteres coringas (como %) para buscar partes de textos.

---
# 📚 Glossário Conceitual do Módulo
* SQL (Structured Query Language): Linguagem padrão usada mundialmente para interagir com Sistemas Gerenciadores de Bancos de Dados Relacionais (SGBDR).
* DQL (Data Query Language): O subconjunto de comandos SQL usado para extrair informações do banco.
* Query (Consulta): Uma instrução ou comando em formato de código enviada ao banco de dados para recuperar informações específicas.
* Tabela (Table): Estrutura bidimensional organizada em linhas (registros) e colunas (atributos) onde os dados relacionais são de fato armazenados.
* Atributo / Coluna: Uma propriedade ou característica específica dos dados representados na tabela (ex: price, product_id).
* NULL: O estado que indica que um dado está ausente, vazio ou é desconhecido no banco de dados. Não é equivalente a zero ou a uma string vazia.
* Alias (AS): Um apelido temporário atribuído a uma coluna ou tabela em uma consulta para tornar o nome de exibição do resultado mais legível e limpo.

---


# 🧠 Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

Nesta seção, documento os principais obstáculos que enfrentei ao praticar os fundamentos de DQL, as causas técnicas desses erros e como utilizei o Gemini Notebook estrategicamente para superá-los.

---

## Cicatriz 1: O Mistério do "Sumiço" dos Valores Nulos (NULL)

Ao tentar filtrar entregas pendentes na tabela `deliveries` (onde a coluna `delivery_date` é vazia), cometi um erro clássico de lógica relacional.

**❌ O que eu tentei fazer (Erro):**
```sql
SELECT order_id, delivery_status
FROM deliveries
WHERE delivery_date = NULL; -- Retornou zero registros, mesmo havendo dados pendentes!
```

**🧠 Por que deu errado?**  
No SQL, NULL não é um valor, mas sim a **ausência de valor** (um estado desconhecido). Portanto, operadores de comparação comuns como `=` ou `!=` falham porque qualquer comparação com NULL resulta em "Desconhecido" (Three-Valued Logic).

**✅ Como corrigir:**  
Para verificar valores nulos, devemos utilizar o operador específico `IS NULL` ou `IS NOT NULL`.

```sql
-- Correção aplicada
SELECT order_id, delivery_status
FROM deliveries
WHERE delivery_date IS NULL;
```

**🤖 Prompt Estratégico usado no Gemini Notebook para entender:**

> *"Recebi este erro no MySQL Workbench ao tentar filtrar entregas pendentes:*
> 
> ```sql
> SELECT order_id, delivery_status
> FROM deliveries
> WHERE delivery_date = NULL;
> ```
> 
> *O resultado veio vazio (0 linhas retornadas), mas eu sei que existem registros com delivery_date vazio na tabela.*
> 
> *Por favor, me ajude a entender:*
> 1. *Por que a consulta acima não funcionou?*
> 2. *O que significa a lógica de três valores (TRUE, FALSE, UNKNOWN) no SQL e como ela se aplica aqui?*
> 3. *Qual é a forma correta de escrever essa consulta para encontrar os registros com data de entrega vazia?*
> 4. *Mostre um exemplo prático com a tabela deliveries, comparando o comportamento de = NULL vs IS NULL.*
> 
> *Quero entender profundamente para não cometer esse erro novamente."*

---

## Cicatriz 2: A Ilusão da Ordem de Escrita (Order of Execution)

Tentei filtrar uma consulta usando um apelido (alias) criado no SELECT diretamente na cláusula WHERE.

**❌ O que eu tentei fazer (Erro):**
```sql
SELECT 
    order_id,
    quantity * unit_price AS total_venda
FROM orders
WHERE total_venda > 1000; -- Erro: coluna total_venda não reconhecida
```

**🧠 Por que deu errado?**  
Apesar de escrevermos o `SELECT` antes do `WHERE` na consulta, o banco de dados executa a query em uma **ordem lógica diferente**. A ordem real de execução é:

1. **FROM** (localiza a tabela)  
2. **WHERE** (filtra as linhas)  
3. **SELECT** (calcula as colunas e aplica os aliases)

Como o `WHERE` roda **antes** do `SELECT`, ele ainda não conhece o apelido `total_venda`.

**✅ Como corrigir:**  
Deve-se usar a coluna original no filtro `WHERE`:

```sql
-- Correção aplicada
SELECT 
    order_id,
    quantity * unit_price AS total_venda
FROM orders
WHERE quantity * unit_price > 1000;
```

**🤖 Prompt Estratégico usado no Gemini Notebook para entender:**

> *"Recebi este erro no MySQL Workbench ao tentar filtrar vendas acima de 1000:*
> 
> ```sql
> SELECT 
>     order_id,
>     quantity * unit_price AS total_venda
> FROM orders
> WHERE total_venda > 1000;
> ```
> 
> *O Workbench retornou o erro: **"Error Code: 1054. Unknown column 'total_venda' in 'where clause'"**.*
> 
> *Por favor, me ajude a entender:*
> 1. *Por que a consulta acima gerou esse erro, sendo que total_venda foi definido no SELECT?*
> 2. *Qual é a ordem lógica de execução de uma consulta SQL? Mostre passo a passo.*
> 3. *Como devo reescrever essa consulta para funcionar corretamente?*
> 4. *Dê uma dica prática para evitar esse erro em consultas futuras.*
> 
> *Quero entender profundamente para não cometer esse erro novamente."*

---

## Cicatriz 3: Sensibilidade a Maiúsculas e Minúsculas (Case Sensitivity)

Ao filtrar categorias na cláusula WHERE, a consulta não trazia resultados por causa de uma única letra maiúscula.

**❌ O que eu tentei fazer (Erro):**
```sql
SELECT product_name, category
FROM products
WHERE category = 'eletrônicos'; -- Não retornou 'Eletrônicos' com E maiúsculo
```

**🧠 Por que deu errado?**  
Dependendo do Sistema Gerenciador de Banco de Dados (SGBD) e da configuração de intercalação (collation), as comparações de texto podem ser estritamente **case-sensitive**.

**✅ Como corrigir:**  
Para tornar a busca segura, apliquei funções de formatação de string (como `LOWER()`) para padronizar o texto antes da comparação:

```sql
-- Correção aplicada
SELECT product_name, category
FROM products
WHERE LOWER(category) = 'eletrônicos';
```

**🤖 Prompt Estratégico usado no Gemini Notebook para entender:**

> *"Recebi este problema no MySQL Workbench ao filtrar produtos por categoria:*
> 
> ```sql
> SELECT product_name, category
> FROM products
> WHERE category = 'eletrônicos';
> ```
> 
> *A consulta não retornou nenhum resultado, mas eu sei que existem produtos da categoria 'Eletrônicos' (com E maiúsculo) na tabela.*
> 
> *Por favor, me ajude a entender:*
> 1. *Por que a consulta acima não encontrou os registros, mesmo eles existindo?*
> 2. *O que é collation e como ela afeta comparações de texto no SQL?*
> 3. *Qual é a forma correta de escrever essa consulta para encontrar os registros independente de maiúsculas/minúsculas?*
> 4. *Quais são os prós e contras de usar funções como LOWER() em grandes tabelas?*
> 
> *Quero entender profundamente para não cometer esse erro novamente."*

---

## 🤖 Prompts Reutilizáveis para Revisão

Aqui estão três prompts estratégicos estruturados para você utilizar no Gemini Notebook para revisar este módulo ou praticar novos cenários:

### 1. Para fixação de conceitos:
> *"Crie um resumo comparativo entre os erros comuns tratados (NULL, aliases no WHERE, case sensitivity). Para cada um, forneça uma definição simples, um exemplo de erro e a solução correta. Use analogias do dia a dia para facilitar a memorização."*

### 2. Para depuração de erros (Troubleshooting):
> *"Simule um cenário onde uma consulta SQL falha silenciosamente (retorna zero linhas). Mostre um passo a passo de depuração: (1) verificar presença de NULLs, (2) verificar uso de aliases, (3) verificar collation/case. Dê um exemplo para cada etapa com código SQL."*

### 3. Para exercitar a Ordem de Execução:
> *"Monte um exercício prático onde o aluno deve reescrever uma consulta com alias no WHERE, ORDER BY e GROUP BY. Peça para ele explicar, em cada etapa, como a ordem de execução influencia o resultado. Forneça uma resolução comentada ao final."*

---

## 📌 Resumo Rápido das Cicatrizes (Cheat Sheet)

| Cicatriz | Problema | Causa | Solução |
|----------|----------|-------|---------|
| **NULL** | `WHERE coluna = NULL` não retorna nada | NULL é desconhecido, não comparável com `=` | Usar `IS NULL` ou `IS NOT NULL` |
| **Alias no WHERE** | `WHERE alias > 1000` dá erro Code 1054 | Ordem de execução: WHERE roda antes do SELECT | Usar a expressão original no WHERE |
| **Case Sensitivity** | `WHERE categoria = 'eletrônicos'` não acha 'Eletrônicos' | Collation case-sensitive do SGBD | Usar `LOWER()` ou `UPPER()` para padronizar |

---

## 🛠️ Prompt Bônus: Para gerar novos exercícios personalizados

> *"Com base nas três cicatrizes de aprendizado (NULL, alias no WHERE, case sensitivity), crie 5 exercícios práticos com diferentes níveis de dificuldade. Para cada exercício, forneça: (a) enunciado, (b) tabela de exemplo com dados, (c) consulta esperada e (d) explicação passo a passo da resolução. Inclua pelo menos um exercício que combine duas cicatrizes ao mesmo tempo."*

