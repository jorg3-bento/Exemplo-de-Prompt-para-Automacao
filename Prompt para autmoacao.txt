Prompt:

Atue como um especialista em n8n, automações, CRM e integração entre WhatsApp, RD Station e Microsoft Outlook.
Objetivo
Projete uma automação no n8n para maquiadoras autônomas, com o objetivo de reduzir o tempo gasto na organização de agendamentos e tornar o processo de orçamento e confirmação de serviços mais simples e organizado.
Público
Maquiadoras autônomas responsáveis pelo atendimento, orçamento, confirmação de pagamento e organização da agenda.
Ferramentas
* WhatsApp
* RD Station CRM
* Microsoft Outlook Calendar
* n8n
Dados do atendimento
A automação deve trabalhar com:
* Nome da cliente
* Data do atendimento
* Horário do atendimento, quando disponível
* Telefone
* E-mail
* Serviço
* Pagamento
Fluxo
1. Recebimento dos dados
Receber os dados provenientes do atendimento pelo WhatsApp e identificar os campos disponíveis:
* Nome
* Data
* Horário
* Telefone
* E-mail
* Serviço
Os dados podem chegar de forma incompleta. O workflow deve permitir que as informações sejam complementadas posteriormente.
2. Cadastro/atualização no RD Station
Criar ou localizar o lead correspondente no RD Station utilizando um identificador adequado, como telefone ou e-mail.
Se o lead já existir, atualizar seu cadastro em vez de criar outro.
Se inicialmente faltarem informações, manter o lead cadastrado e permitir sua atualização posteriormente.
Quando o serviço e o e-mail forem informados, atualizar o registro existente.
3. Verificação das condições para agendamento
Consultar os dados atualizados do lead no RD Station.
O workflow somente poderá prosseguir para o agendamento no Outlook se TODAS as condições abaixo forem verdadeiras:
* Nome preenchido;
* E-mail preenchido;
* Serviço preenchido;
* Data do atendimento preenchida;
* Horário do atendimento preenchido;
* Campo personalizado Pagamento no RD Station = Pago.
A condição de pagamento deve ser tratada de forma explícita:
Pagamento == "Pago"
Qualquer outro valor, incluindo campo vazio, Pendente, Aguardando, Cancelado ou qualquer valor diferente de Pago, deve impedir o agendamento.
4. Verificação de duplicidade
Antes de criar o evento no Outlook, verificar se o atendimento já foi agendado.
O workflow deve possuir uma forma confiável de identificar o agendamento já criado, evitando que a mesma cliente ou o mesmo atendimento gere eventos duplicados.
Sempre que possível, utilizar um identificador único do atendimento/lead ou uma informação de controle armazenada no RD Station.
5. Criação do evento no Outlook
Somente depois de todas as condições serem satisfeitas e não existir um evento correspondente, criar o evento no Microsoft Outlook Calendar.
O evento deverá conter, quando disponíveis:
* Nome da cliente;
* Serviço contratado;
* Data;
* Horário;
* E-mail;
* Telefone;
* Informações relevantes do atendimento.
Após a criação bem-sucedida do evento, registrar no RD Station que o agendamento foi realizado, armazenando o identificador do evento do Outlook ou outra referência adequada.
Regras obrigatórias
1. Nunca agendar sem e-mail.
2. Nunca agendar sem serviço definido.
3. Nunca agendar sem data e horário.
4. Nunca agendar sem Pagamento = Pago no RD Station.
5. Não criar leads duplicados.
6. Não criar eventos duplicados no Outlook.
7. Se os dados estiverem incompletos, não realizar o agendamento.
8. Permitir que o lead seja atualizado quando novas informações forem recebidas.
9. Após o pagamento ser alterado para Pago, o processo deve poder identificar que o atendimento está pronto para agendamento.
10. Tratar erros de comunicação com as APIs e evitar perda de dados.
11. Registrar informações suficientes para permitir rastreamento e auditoria do processo.
Nós do n8n
Explique quais nós devem ser utilizados, incluindo, quando aplicável:
* Trigger/Webhook para entrada das informações;
* nós de tratamento e normalização dos dados;
* nós HTTP Request ou integrações específicas para RD Station;
* IF ou Switch para validação das condições;
* nós de consulta ao Outlook;
* nó de criação de evento no Outlook;
* nós de atualização do RD Station;
* nós de tratamento de erros.
Para cada nó, informe:
* Nome sugerido;
* Tipo do nó;
* Objetivo;
* Dados de entrada;
* Dados de saída;
* Principais configurações;
* Condições utilizadas.
Lógica principal
Representar a lógica principal da automação da seguinte maneira:
WhatsApp ↓ Extrair dados ↓ Localizar/Criar lead no RD Station ↓ Atualizar informações disponíveis ↓ Consultar dados atuais do lead ↓ Todos os campos obrigatórios estão preenchidos? ├── NÃO → Aguardar/complementar informações └── SIM ↓ Pagamento = "Pago"? ├── NÃO → Não agendar └── SIM ↓ Já existe agendamento? ├── SIM → Não criar novo evento └── NÃO ↓ Criar evento no Outlook ↓ Registrar confirmação do agendamento no RD Station

Entrega esperada
Explique detalhadamente:
1. Arquitetura completa do workflow.
2. Ordem dos nós.
3. Tipo de cada nó.
4. Configuração de cada nó.
5. Campos e variáveis utilizados.
6. Lógica dos nós IF/Switch.
7. Estratégia para localizar e atualizar o lead no RD Station.
8. Como verificar Pagamento = Pago.
9. Como evitar eventos duplicados.
10. Como registrar o ID do evento do Outlook no RD Station.
11. Como tratar dados incompletos.
12. Como tratar erros.
13. Quais credenciais e permissões são necessárias.
14. Exemplo de entrada e saída dos principais nós.
15. Diagrama textual completo do workflow.
Não invente endpoints, campos ou funcionalidades específicas das APIs. Quando determinada configuração depender da versão atual ou da configuração da conta, deixe isso explícito.
Ao final, apresente uma tabela com:
| Ordem | Nó n8n | Função | Condição/Regra | Próximo passo |
