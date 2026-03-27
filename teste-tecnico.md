# 🚀 Desafio Técnico: Especialista em n8n e Agentes de IA (SOLABS)

## 🏢 Sobre o SOLABS
O SOLABS atua como uma infraestrutura autônoma de geração de receita, unindo marketing, dados e comercial. Utilizamos uma arquitetura baseada em microsserviços onde o **n8n** atua como nosso motor de automação assíncrona (Event-Driven), conectando nossa API em FastAPI, a plataforma de mensageria Chatwoot e nossos bancos de dados (PostgreSQL e MongoDB). Nossos módulos de Inteligência Artificial, como o **Sol SDR**, atuam 24/7 na linha de frente para qualificar leads e agendar reuniões.

## 🎯 O Desafio: Construindo o "Cérebro" do Sol SDR
Sua missão é criar um fluxo (workflow) no n8n que simule a etapa de triagem e qualificação de um novo lead, executada pelo nosso agente de IA (Sol SDR).

O fluxo deve receber um payload via Webhook, enviar os dados para um modelo de LLM (ex: OpenAI) configurado com um prompt específico de qualificação, analisar a resposta e tomar decisões de roteamento no n8n.

### 🛠️ Requisitos do Fluxo
1. **Gatilho (Trigger):** O fluxo deve ser iniciado por um nó de Webhook configurado para o evento simulado `lead.created`.
2. **Processamento de IA (AI Agent):** Você deve utilizar um nó de IA (ex: *Basic LLM Chain* ou *Agent*) para analisar os dados recebidos. O prompt deve instruir a IA a classificar o lead como `QUALIFICADO` ou `NAO_QUALIFICADO` com base **estritamente** nas seguintes Regras de Negócio Internas:
   - O lead **obrigatoriamente** deve ter um E-mail informado.
   - O lead **obrigatoriamente** deve representar uma empresa (nunca uso pessoal) com faturamento a partir de R$ 120 mil.
   - O lead deve atender a **pelo menos UM** dos seguintes critérios extras:
     - Investimento em Mídia Paga > R$ 2.000,00/mês.
     - Dores na estrutura de Receita que as soluções SOLABS podem resolver.
     - Busca solução técnico/estratégica com IA.
3. **Lógica Condicional (Switch/If):** Com base na saída estruturada da IA, o fluxo deve se dividir em dois caminhos:
   - **Caminho A (Qualificado):** Simular uma requisição HTTP (método POST) para um endpoint mockado (ex: webhook.site), enviando o payload original mais o status `{ "status_crm": "SQL" }`.
   - **Caminho B (Não Qualificado):** Simular uma requisição HTTP enviando o status `{ "status_crm": "Descartado", "motivo": "<motivo_dado_pela_IA>" }`.
4. **Integração com MongoDB:** Após a qualificação, o fluxo deve registrar o resultado em uma coleção no MongoDB, armazenando ao menos os dados do lead, o status final (`QUALIFICADO` ou `NAO_QUALIFICADO`) e o timestamp de processamento.

### 📦 Payload de Teste (Exemplo de Entrada)
Você deve testar seu fluxo enviando requisições HTTP (POST) para o seu webhook com diferentes cenários. Exemplo:
```json
{
  "event_type": "lead.created",
  "lead": {
    "name": "João Silva",
    "email": "joao@techcorp.com.br",
    "phone": "+5511999999999",
    "company": "TechCorp S.A",
    "annual_revenue": "R$ 1.200.000",
    "media_investment": "R$ 2.000/mês",
    "pain_points": "Nossa equipe de vendas perde muito tempo tentando qualificar leads frios e o CRM está sempre desatualizado."
  }
}
```

### 📋 O que será avaliado?
- **Engenharia de Prompt:** Como você estruturou as instruções para garantir que a IA responda no formato exato que o n8n precisa (ex: forçando um output em JSON).
- **Domínio do n8n:** Uso correto de nós, parse de dados (JSON), tratamento de erros (Error Trigger) e uso de variáveis.
- **Simplicidade e Eficiência:** O fluxo é limpo e direto ao ponto?

### 🚀 Como entregar
1. Crie um fork ou um repositório privado no GitHub e libere acesso *READ* para o usuário `henricop`.
2. Exporte o seu fluxo do n8n (arquivo `.json`) e adicione ao repositório.
3. Adicione um arquivo `SOLUTION.md` explicando brevemente:
    - Qual modelo de LLM você escolheu e por quê.
    - Como você garantiu que a IA respeitasse a regra rigorosa de faturamento (R$ 120k).
    - Quais dificuldades encontrou.
4. Enviar um email para `admin@solabs.com.br` com a url do seu repositório

Caso precise tira alguma dúvida entrar em contato com: 
    - Email: admin@solabs.com.br
    - Henrico: (32) 99155-6640

Boa sorte! Equipe SOLABS.
