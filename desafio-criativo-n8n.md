# 🎯 Desafio Criativo: Planejando Automações com N8N Usando Apenas Bons Prompts

## 🧱 Passo 1: Defina a automação desejada

Quero criar uma automação no N8N para **triar automaticamente bugs reportados por formulário**.

**Público ou responsável:**
Equipe de QA (recebe e prioriza os bugs) e pessoas desenvolvedoras (consomem a fila de triagem). Quem reportou o bug também é impactado, pois recebe a confirmação.

**Resultado esperado:**
Cada bug válido fica registrado em uma planilha de triagem com ID, severidade e status inicial; a equipe de QA recebe um e-mail (com destaque se for crítico) e quem reportou recebe uma confirmação com o ID do bug.

---

## 🧱 Passo 2: Adicione contexto e regras

**Ferramentas envolvidas:**
Google Forms, Google Sheets e Gmail.

**Fluxo desejado:**
1. Detectar uma nova resposta do formulário de bug report (o Forms grava na aba de respostas do Google Sheets; o N8N monitora essa aba).
2. Validar os dados (e-mail válido e campos obrigatórios preenchidos).
3. Classificar a severidade (Crítico, Alto, Médio ou Baixo) a partir do campo informado no formulário.
4. Gerar um ID único e registrar o bug na aba "Triagem" da planilha, com status "Novo".
5. Enviar e-mail para a equipe de QA (com prefixo `[CRÍTICO]` quando aplicável).
6. Enviar e-mail de confirmação para quem reportou, contendo o ID gerado.

**Regras importantes:**
- Ignorar registros sem e-mail válido ou sem descrição do bug.
- Evitar duplicidade: se o mesmo registro for reprocessado, atualizar a linha existente em vez de criar outra.
- Bugs críticos devem gerar o alerta ao QA antes do e-mail de confirmação ao solicitante.
- Se o envio de e-mail falhar, o registro na planilha não deve ser perdido.

---

## 🧱 Passo 3: Prompt final

```text
Atue como um especialista em N8N.

Crie uma automação para triar automaticamente bugs reportados por formulário.

Público:
Equipe de QA (recebe e prioriza os bugs), pessoas desenvolvedoras (consomem a fila de triagem) e quem reportou o bug (recebe a confirmação).

Ferramentas envolvidas:
Google Forms, Google Sheets e Gmail.

Fluxo:
1. Detectar uma nova resposta do formulário (o Forms grava na aba de respostas do Google Sheets; o N8N monitora essa aba).
2. Validar e-mail e campos obrigatórios.
3. Classificar a severidade (Crítico, Alto, Médio ou Baixo) com base no campo informado.
4. Gerar um ID único e registrar o bug na aba "Triagem" com status "Novo".
5. Enviar e-mail para a equipe de QA, com prefixo [CRÍTICO] quando aplicável.
6. Enviar e-mail de confirmação para quem reportou, com o ID do bug.

Regras:
- Ignorar registros sem e-mail válido ou sem descrição do bug.
- Evitar duplicidade: reprocessar o mesmo registro deve atualizar a linha, não criar outra.
- Em bugs críticos, o alerta ao QA deve ser enviado antes da confirmação ao solicitante.
- Falha no envio de e-mail não pode causar perda do registro na planilha.

Explique quais nós do N8N devem ser utilizados e a lógica de funcionamento do workflow. Para cada nó, informe também qual sistema ou pessoa é responsável pela etapa (ex.: Google Forms/Sheets gerando o dado, N8N processando, QA recebendo o alerta) e quais são os pontos de falha possíveis.
```
