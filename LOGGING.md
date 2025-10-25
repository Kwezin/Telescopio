# LOGGING.md

## 🎯 Objetivo
Padronizar a geração de logs de **aplicação** (técnicos) e **auditoria** (de negócio) no sistema **SCTEC — Sistema de Controle de Telescópio Espacial Compartilhado**.  
Os logs garantem rastreabilidade, auditoria e transparência no comportamento da API.

---

## 🔍 Tipos de Log

### 1. Log de Aplicação
Registra eventos internos do sistema, úteis para depuração e acompanhamento da execução.

**Formato**
NIVEL:TIMESTAMP:SERVICO:MENSAGEM


**Exemplo**
NFO:2025-10-26T18:00:04.500Z:servico-agendamento:Requisição recebida em POST /agendamentos
INFO:2025-10-26T18:00:04.505Z:servico-agendamento:Tentando adquirir lock
INFO:2025-10-26T18:00:05.120Z:servico-agendamento:Lock adquirido com sucesso
INFO:2025-10-26T18:00:05.122Z:servico-agendamento:Salvando agendamento no BD
INFO:2025-10-26T18:00:05.124Z:servico-agendamento:Agendamento criado com sucesso


Esses logs de aplicação são gravados continuamente durante o funcionamento do serviço Flask, servindo como histórico técnico das ações do sistema.

---

### 2. Log de Auditoria
Registra eventos relevantes do ponto de vista de negócio em **formato JSON imutável**.  
São usados para comprovar quem executou uma ação, quando e sobre qual recurso.

**Formato JSON**
```json
{
  "timestamp_utc": "2025-10-26T18:00:05.123Z",
  "level": "AUDIT",
  "event_type": "AGENDAMENTO_CRIADO",
  "service": "servico-agendamento",
  "details": {
    "agendamento_id": 123,
    "cientista_id": 7,
    "telescopio_id": 1,
    "horario_inicio_utc": "2025-12-01T03:00:00Z",
    "horario_fim_utc": "2025-12-01T03:05:00Z"
  }
}

Esses logs são gerados após eventos de negócio bem-sucedidos, como:

criação de um agendamento;

cancelamento de agendamento;

alteração de status.

Cada registro de auditoria serve como prova imutável de que uma ação ocorreu, com identificação do cientista, horário e recurso afetado.

3. Log de Erro

Registra falhas de execução, exceções e condições inesperadas.

**Exemplo**
ERROR:2025-10-26T18:00:05.999Z:servico-agendamento:Falha ao adquirir lock — recurso ocupado
ERROR:2025-10-26T18:00:06.100Z:servico-agendamento:Erro ao gravar no banco de dados


| Tipo de Log | Nível           | Formato | Geração                       |
| ----------- | --------------- | ------- | ----------------------------- |
| Aplicação   | INFO / DEBUG    | Texto   | Durante o fluxo normal da API |
| Auditoria   | AUDIT           | JSON    | Após eventos de negócio       |
| Erro        | ERROR / WARNING | Texto   | Durante exceções e falhas     |

**Exemplo Completo de Sequência de Logs**
INFO:2025-12-01T03:00:00.001Z:servico-agendamento:Requisição recebida para POST /agendamentos
INFO:2025-12-01T03:00:00.005Z:servico-agendamento:Tentando adquirir lock para recurso Hubble-Acad_2025-12-01T03:00:00Z
INFO:2025-12-01T03:00:00.020Z:servico-agendamento:Lock adquirido com sucesso
INFO:2025-12-01T03:00:00.050Z:servico-agendamento:Salvando novo agendamento
AUDIT:{"timestamp_utc":"2025-12-01T03:00:00.050Z","level":"AUDIT","event_type":"AGENDAMENTO_CRIADO","service":"servico-agendamento","details":{"agendamento_id":123,"cientista_id":7,"telescopio_id":1,"horario_inicio_utc":"2025-12-01T03:00:00Z"}}
INFO:2025-12-01T03:00:00.060Z:servico-agendamento:Liberando lock para recurso Hubble-Acad_2025-12-01T03:00:00Z

Essa sequência mostra o comportamento esperado do sistema:

- o início da requisição,

- a tentativa e aquisição do lock,

- a criação do agendamento,

- e o log de auditoria confirmando o evento.



## Boas praticas
Todos os timestamps devem estar no formato UTC ISO 8601 (YYYY-MM-DDTHH:MM:SS.sssZ).

Logs de auditoria são imutáveis (não podem ser editados ou apagados).

Logs de aplicação devem ser gravados no console e em arquivo (app.log).

Cada serviço (Flask e Node.js) deve incluir seu nome no campo service.

O nível “AUDIT” é exclusivo para eventos de negócio.

O arquivo de log deve ser versionado apenas em ambiente de desenvolvimento (não em produção).
