# 💄 Automação de Agendamentos para Maquiadoras

Automação desenvolvida em **n8n** para simplificar o processo de orçamento, confirmação de pagamento e agendamento de serviços para maquiadoras autônomas.

O workflow integra **WhatsApp, RD Station CRM e Microsoft Outlook**, reduzindo o trabalho manual na organização dos atendimentos.

## 🎯 Objetivo

Centralizar e automatizar o processo desde o primeiro contato da cliente até a confirmação do agendamento.

A automação busca:

* Reduzir o tempo gasto com organização de agenda;
* Facilitar o processo de orçamento;
* Centralizar os dados das clientes;
* Evitar cadastros duplicados;
* Evitar agendamentos duplicados;
* Garantir que somente serviços pagos sejam adicionados à agenda.

## 🔗 Integrações

* **WhatsApp** — entrada das informações da cliente e do atendimento.
* **RD Station CRM** — armazenamento e atualização dos leads.
* **Microsoft Outlook Calendar** — gerenciamento dos horários confirmados.
* **n8n** — responsável pela orquestração de todo o fluxo.

## 🔄 Fluxo da automação

```text
WhatsApp
   │
   ▼
Receber informações
   │
   ▼
Extrair dados
   │
   ├── Nome
   ├── Data
   ├── Horário
   ├── Telefone
   ├── E-mail
   └── Serviço
   │
   ▼
Criar ou localizar lead
no RD Station
   │
   ▼
Atualizar informações
   │
   ▼
Verificar dados obrigatórios
   │
   ├── Dados incompletos
   │       │
   │       └── Aguardar atualização
   │
   └── Dados completos
           │
           ▼
     Pagamento = "Pago"?
           │
       ┌───┴───┐
       │       │
      NÃO     SIM
       │       │
       ▼       ▼
   Não agendar   Verificar
                 duplicidade
                    │
                    ▼
             Já existe evento?
                │       │
               SIM     NÃO
                │       │
                ▼       ▼
           Não criar   Criar evento
           novamente   no Outlook
                          │
                          ▼
                   Registrar ID do
                   evento no RD Station
```

## 📋 Dados utilizados

A automação trabalha com os seguintes dados:

| Campo       | Descrição                        |
| ----------- | -------------------------------- |
| `nome`      | Nome da cliente                  |
| `telefone`  | Telefone da cliente              |
| `email`     | E-mail da cliente                |
| `data`      | Data desejada para o atendimento |
| `horario`   | Horário desejado                 |
| `servico`   | Serviço contratado               |
| `pagamento` | Status do pagamento              |

## 💰 Regra de pagamento

O agendamento somente poderá ser criado quando o campo personalizado do RD Station:

```text
Pagamento = Pago
```

Qualquer outro valor impede a criação do evento no Outlook.

Exemplos:

```text
Pagamento = Pendente  → ❌ Não agendar
Pagamento = Aguardando → ❌ Não agendar
Pagamento = Cancelado → ❌ Não agendar
Pagamento = Pago       → ✅ Pode prosseguir
```

## 📅 Regras para criação do agendamento

O workflow **não deve criar um evento no Outlook** caso qualquer uma destas condições não seja atendida:

* [ ] Nome preenchido
* [ ] E-mail preenchido
* [ ] Serviço preenchido
* [ ] Data preenchida
* [ ] Horário preenchido
* [ ] Pagamento igual a `Pago`
* [ ] Não existir agendamento correspondente

Somente após todas as validações serem aprovadas o evento poderá ser criado.

## 🧩 Estrutura esperada do workflow

Os principais componentes do workflow incluem:

1. **Trigger/Webhook**

   * Recebe os dados do atendimento.

2. **Tratamento dos dados**

   * Normaliza e organiza as informações recebidas.

3. **RD Station**

   * Localiza ou cria o lead.
   * Atualiza os dados disponíveis.

4. **Validação**

   * Verifica se todos os campos necessários estão preenchidos.

5. **Validação do pagamento**

   * Confirma se `Pagamento = Pago`.

6. **Verificação de duplicidade**

   * Verifica se o atendimento já possui um evento no Outlook.

7. **Microsoft Outlook**

   * Cria o evento na agenda.

8. **Atualização do RD Station**

   * Registra que o atendimento foi agendado.
   * Armazena, quando aplicável, o identificador do evento criado.

## 🛡️ Regras de segurança do fluxo

O workflow deve seguir alguns princípios:

* Nunca agendar sem pagamento confirmado.
* Nunca agendar sem e-mail.
* Nunca agendar sem serviço.
* Nunca agendar sem data e horário.
* Nunca criar eventos duplicados.
* Nunca criar leads duplicados quando as informações forem complementadas.
* Manter o lead no processo enquanto houver informações pendentes.
* Registrar erros de integração para facilitar diagnóstico.

## 🗂️ Organização sugerida do projeto

```text
/
├── README.md
├── workflows/
│   └── automacao-agendamento.json
├── docs/
│   └── arquitetura.md
└── examples/
    └── exemplo-dados.json
```

## 🚀 Próximos passos

* [ ] Definir o mecanismo de entrada do WhatsApp.
* [ ] Configurar credenciais do RD Station.
* [ ] Configurar credenciais do Microsoft Outlook.
* [ ] Criar os campos personalizados necessários no RD Station.
* [ ] Definir o identificador utilizado para evitar leads duplicados.
* [ ] Definir o identificador utilizado para evitar eventos duplicados.
* [ ] Construir o workflow no n8n.
* [ ] Testar o fluxo com dados incompletos.
* [ ] Testar pagamento pendente.
* [ ] Testar pagamento confirmado.
* [ ] Testar tentativa de agendamento duplicado.
* [ ] Validar tratamento de erros.

## 📌 Status

**Em desenvolvimento**

Projeto destinado ao planejamento e implementação de uma automação de atendimento e gerenciamento de agenda para maquiadoras autônomas.
