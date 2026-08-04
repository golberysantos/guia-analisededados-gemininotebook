# 📑 Módulo 3: Modelagem Relacional e JOINs (Combinação de Fontes de Dados)

Este módulo aborda o verdadeiro poder dos bancos de dados relacionais: a capacidade de conectar tabelas distintas por meio de relacionamentos lógicos. Aqui, aprendemos a unificar fontes de dados dispersas para realizar análises abrangentes e responder a perguntas complexas de negócios.

---

## 🎯 A Importância de Conectar Fontes para o Analista

Ao analisar o que é preciso para ser um bom analista de dados, as fontes destacam que os dados reais de uma empresa quase nunca estão em um único lugar [1]. Eles são distribuídos em várias tabelas para evitar redundância e garantir a integridade do sistema (processo conhecido como normalização) [2, 3].

Se você deseja gerar insights profundos — como cruzar o faturamento de vendas com o perfil dos representantes de cada região —, dominar as cláusulas de junção (JOINs) é uma habilidade técnica crucial e mandatória [2, 4]. O analista de sucesso é aquele que consegue visualizar mentalmente como as tabelas se conectam antes mesmo de executar a consulta [3].

---

## 📖 Resumo Estruturado do Assunto

### 1. Entendendo Chaves e Relacionamentos
Para que duas tabelas possam ser unidas, elas precisam compartilhar uma coluna em comum, que serve de ponte [2, 5]:
*   **Chave Primária (Primary Key - PK)**: Um identificador exclusivo para cada registro em uma tabela (ex: `rep_id` na tabela de representantes) [5].
*   **Chave Estrangeira (Foreign Key - FK)**: Uma coluna em uma tabela que faz referência direta à Chave Primária de outra tabela (ex: `rep_id` na tabela de vendas) [5].

### 2. Tipos de JOINs no SQL
As junções determinam como o banco de dados deve tratar as correspondências (e as ausências delas) entre as tabelas [2, 4]:

*   **`INNER JOIN`**: Retorna apenas as linhas que possuem correspondência exata em ambas as tabelas [4]. Se um registro existir em uma tabela mas não na outra, ele é descartado do resultado final [6].
*   **`LEFT JOIN` (ou `LEFT OUTER JOIN`)**: Retorna todos os registros da tabela à esquerda (declarada no `FROM`), junto com os dados correspondentes da tabela à direita [4]. Caso não haja correspondência na tabela da direita, o SQL preenche esses campos com `NULL` [6].
*   **`RIGHT JOIN` (ou `RIGHT OUTER JOIN`)**: Funciona de forma inversa ao `LEFT JOIN`. Preserva todos os registros da tabela à direita e traz os correspondentes da esquerda, preenchendo com `NULL` as ausências [4, 7].
*   **`FULL JOIN` (ou `FULL OUTER JOIN`)**: Retorna todos os registros de ambas as tabelas, combinando-os quando há correspondência e exibindo `NULL` onde não há [4, 7].

### 3. Operações de Conjunto: `UNION` vs `UNION ALL`
Diferente dos JOINs (que combinam tabelas lado a lado/horizontalmente), as uniões combinam os resultados de duas ou mais consultas empilhando as linhas verticalmente [2, 8]:
*   **`UNION`**: Mescla os resultados e **remove automaticamente** quaisquer linhas duplicadas entre as consultas [8].
*   **`UNION ALL`**: Mescla os resultados e **mantém todas as linhas**, incluindo as duplicadas, sendo mais rápido por não exigir uma etapa de processamento para deduplicação [8, 9].

---

## 📚 Glossário Conceitual do Módulo

*   **Banco de Dados Relacional**: Um modelo de organização de dados em tabelas bidimensionais que se relacionam entre si por meio de chaves comuns [3].
*   **Chave Primária (PK)**: Atributo que garante que nenhuma linha em uma tabela seja duplicada [5].
*   **Chave Estrangeira (FK)**: Atributo que estabelece e reforça um elo de ligação entre os dados de duas tabelas [5].
*   **Normalização de Dados**: Técnica de design de banco de dados que organiza as tabelas de forma a reduzir a redundância e a dependência de dados.
*   **Deduplicação**: O processo de identificar e remover registros idênticos de um conjunto de dados (comportamento padrão do comando `UNION`) [8].

---

## 🛠️ Troubleshooting e Comportamento de Dados

### O Impacto dos Valores Nulos (`NULL`) em Junções
Um comportamento comum que confunde analistas ao utilizar o `LEFT JOIN` é a geração de valores nulos. Considere o seguinte cenário:

```sql
SELECT s.sales, r.sales_rep
FROM sales_data s
LEFT JOIN representatives r
ON s.rep_id = r.rep_id
WHERE s.region = 'West';
```

Se houver uma venda na região oeste realizada por um representante temporário cujo `rep_id` não está cadastrado na tabela `representatives`, o campo `sales_rep` retornará como `NULL` [5, 6]. Isso ocorre porque o `LEFT JOIN` garante a exibição de todas as vendas (tabela à esquerda), mesmo que não haja um representante mapeado correspondente [4, 6].

---

## 🤖 Prompts Reutilizáveis para Revisão Técnica

Guarde estes modelos de prompts para utilizá-los no Gemini Notebook sempre que precisar praticar ou solucionar problemas de modelagem:

1.  **Para depuração de erros no MySQL Workbench:**  
    > *"Estou desenvolvendo uma consulta com múltiplos JOINs no MySQL Workbench e recebi o seguinte erro do console: **'[Cole o código ou mensagem de erro exata aqui]'**. O meu script atual é o seguinte: **'[Cole sua query aqui]'**. Explique a falha técnica de relacionamento e apresente o código corrigido."*

2.  **Para exercitar a escolha do JOIN correto:**  
    > *"Aja como um mentor de banco de dados. Apresente um cenário de negócios onde eu precise decidir entre usar um `INNER JOIN` ou um `LEFT JOIN`. Me faça perguntas para que eu tente adivinhar qual é o JOIN correto com base no comportamento esperado dos dados nulos."*

3.  **Para praticar UNION vs UNION ALL:**  
    > *"Explique, com um exemplo prático de tabelas de clientes de duas filiais diferentes, quando eu deveria priorizar o uso do `UNION ALL` por questões de performance e quando o `UNION` é estritamente necessário para evitar duplicidade de relatórios."*
