# 📑 Caderno Temático: SQL para Análise de Dados

Este repositório é o meu **Caderno Temático de Estudos**, desenvolvido especificamente como entrega do desafio do meu curso. Ele é focado em consolidar de forma prática e profunda os conhecimentos essenciais de **SQL (Structured Query Language)**. 

A pergunta motivadora que guia este material é:
> **"O que preciso para me tornar um bom analista de dados?"**

A resposta documentada aqui foca em um dos pilare mais importantes: o domínio do SQL como ferramenta primordial para extrair, filtrar e estruturar dados brutos de bancos relacionais.

---

## 🎯 Contexto e Objetivos

Neste caderno, o SQL é tratado como a habilidade técnica não negociável do analista [3]. O objetivo é estruturar meu aprendizado de forma incremental, partindo do básico até técnicas avançadas de manipulação de dados [1, 5]. 

Meus objetivos específicos com esta entrega são:
1. **Dominar a extração e manipulação de dados:** Consolidar a sintaxe de consultas, agregações, subconsultas e funções analíticas [6, 7].
2. **Documentar o processo de aprendizado:** Registrar não apenas o código final, mas as dúvidas, os erros comuns ("cicatrizes") e as soluções encontradas durante a jornada.
3. **Criar um Guia Reutilizável:** Disponibilizar resumos, glossários e prompts que facilitem revisões rápidas no futuro.

---

## 📚 Curadoria de Fontes (Grounding no Gemini Notebook)

Este material foi construído e validado utilizando a inteligência do **Gemini Notebook**, tendo como base as seguintes fontes indicadas pela IA:

1. **[Essential SQL Concepts for Data Analysts](https://www.freecodecamp.org/news/sql-concepts-for-data-analysts/)** (freeCodeCamp)  
   * *Contribuição:* Explicações conceituais de comandos básicos, estruturas condicionais (CASE), subconsultas, CTEs e Window Functions [6].
2. **[Free 6-Week SQL Roadmap for Data Science](https://github.com/andresvourakis/free-6-week-sql-roadmap-data-science)** (GitHub)  
   * *Contribuição:* Metodologia de estudo semanal estruturada e ideias de projetos práticos para portfólio [7, 8].
3. **[Guia de Data Analytics para Iniciantes](https://www.rocketseat.com.br/blog/artigos/post/data-analytics-para-iniciantes-guia-completo)** (Rocketseat)  
   * *Contribuição:* Definição do kit de habilidades do analista de dados e a importância do SQL no dia a dia [9, 10].
4. **[Como superar os desafios de se tornar um analista de dados](https://learnsql.com.br/blog/como-superar-os-desafios-de-se-tornar-um-analista-de-dados/)** (LearnSQL.com.br)  
   * *Contribuição:* Estratégias para superar a sobrecarga de informação e trilhas de aprendizado recomendadas para SQL [1, 11].

---

## 🧭 Guia de Estudos (Módulos de SQL)

O caderno está dividido nos seguintes arquivos `.md` individuais, estruturados de acordo com o nível de complexidade recomendado nas fontes [1, 7]:

* **[Módulo 1: Fundamentos DQL (Data Query Language)](01-fundamentos-dql.md)**  
  * *Foco:* Sintaxe essencial: `SELECT`, `FROM` e filtros dinâmicos com `WHERE` e operadores lógicos [12, 13].
* **[Módulo 2: Agregações e Agrupamentos](02-agregacoes-e-agrupamentos.md)**  
  * *Foco:* Sumarização de dados com `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` e filtros pós-agrupamento com `GROUP BY` e `HAVING` [14].
* **[Módulo 3: Modelagem Relacional e JOINs](03-joins-e-relacionamentos.md)**  
  * *Foco:* Como os dados se conectam em bancos de dados relacionais utilizando `INNER`, `LEFT`, `RIGHT`, `FULL JOIN` e operações de conjuntos (`UNION` e `UNION ALL`) [15-18].
* **[Módulo 4: Subconsultas, CTEs e Condicionais](04-subqueries-ctes-e-condicionais.md)**  
  * *Foco:* Organização de queries complexas usando expressões condicionais (`CASE WHEN`), subconsultas tradicionais e Expressões de Tabela Comum (`CTEs`) [19, 20].
* **[Módulo 5: Funções de Janela (Window Functions)](05-window-functions.md)**  
  * *Foco:* Análises estatísticas avançadas, rankings e totais acumulados usando `OVER`, `PARTITION BY`, `ROW_NUMBER()`, `RANK()` e `DENSE_RANK()` [21-23].

---

## 🗺️ Roadmap de Próximos Passos (Além do SQL)

Após consolidar o domínio do SQL, o roadmap de estudos incluirá o desenvolvimento das seguintes habilidades complementares [1, 2, 10]:

1. **Estatística Descritiva e Testes de Hipóteses:** Para dar embasamento matemático e garantir que as conclusões tiradas a partir dos dados estejam corretas [24].
2. **Visualização de Dados e Storytelling:** Dominar ferramentas como **Power BI** ou **Tableau** para transformar queries complexas de SQL em painéis visuais interativos que qualquer pessoa de negócios consiga entender [10, 25].
3. **Python (Pandas & NumPy):** Para automação de pipelines de dados e manipulação de arquivos quando as planilhas ou o banco de dados não forem suficientes [1, 10].
4. **Excel Avançado:** Como ferramenta ágil para análises exploratórias rápidas e tabelas dinâmicas no dia a dia corporativo [2].

