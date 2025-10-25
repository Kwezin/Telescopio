# MODELOS.md

## 📘 Sistema de Controle de Telescópio Espacial Compartilhado (SCTEC)

O sistema SCTEC permite que cientistas do mundo todo reservem tempo de observação em telescópios espaciais acadêmicos.  
A seguir estão os modelos de dados que definem as entidades centrais do sistema.

---

## 🧑‍🔬 Entidade: Cientista
Representa o pesquisador que solicita tempo de observação.

**Atributos**
- `id` (int): Identificador único.
- `nome` (string): Nome completo.
- `email` (string): E-mail institucional.
- `instituicao` (string): Universidade ou instituto de pesquisa.
- `pais` (string): País de origem.
- `criado_em` (datetime): Data/hora de cadastro.

---

## 🔭 Entidade: Telescopio
Representa o telescópio disponível para agendamento.

**Atributos**
- `id` (int): Identificador único.
- `nome` (string): Nome do telescópio (ex.: *Hubble-Acad*).
- `descricao` (string): Descrição técnica.
- `localizacao_orbital` (string): Posição ou órbita.
- `status` (string): “ativo”, “em manutenção”, “offline”.
- `criado_em` (datetime): Data/hora de cadastro.

---

## 🗓️ Entidade: Agendamento
Representa a reserva de uso de um telescópio por um cientista.

**Atributos**
- `id` (int): Identificador único.
- `cientista_id` (int): ID do cientista solicitante.
- `telescopio_id` (int): ID do telescópio.
- `horario_inicio_utc` (datetime): Início da observação (UTC).
- `horario_fim_utc` (datetime): Fim da observação (UTC).
- `status` (string): “pendente”, “confirmado”, “cancelado”.
- `criado_em` (datetime): Data/hora de criação.
- `ultima_atualizacao` (datetime): Última modificação.

---

## 🔒 Entidade: Lock
Controla exclusão mútua para evitar condições de corrida.

**Atributos**
- `id` (int): Identificador do lock.
- `recurso` (string): Nome do recurso bloqueado  
  (ex.: `"Hubble-Acad_2025-12-01T03:00:00Z"`).
- `status` (string): “ativo” ou “liberado”.
- `criado_em` (datetime): Momento em que o lock foi criado.
- `liberado_em` (datetime): Momento de liberação.
