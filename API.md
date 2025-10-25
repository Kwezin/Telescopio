# API.md

## 📘 Serviço de Agendamento (Flask)

O Serviço de Agendamento é o “cérebro” do SCTEC.  
Ele oferece uma **API RESTful** seguindo o padrão **HATEOAS**.

---

## 🔹 GET /time
Retorna o horário UTC oficial do servidor.

**Resposta (200 OK)**
```json
{
  "server_time_utc": "2025-10-26T18:00:00Z",
  "_links": {
    "self": "/time",
    "agendamentos": "/agendamentos",
    "cientistas": "/cientistas"
  }
}

## GET /cientistas

{
  "cientistas": [
    {
      "id": 1,
      "nome": "Marie Curie",
      "email": "marie@universite.fr",
      "instituicao": "Université de Paris",
      "_links": {
        "self": "/cientistas/1",
        "agendamentos": "/agendamentos?cientista_id=1"
      }
    }
  ]
}


## 🔹 POST /cientistas

requisição:
{
  "nome": "Albert Einstein",
  "email": "einstein@princeton.edu",
  "instituicao": "Princeton University",
  "pais": "Alemanha"
}

resposta:
{
  "id": 2,
  "status": "cientista_criado",
  "_links": {
    "self": "/cientistas/2",
    "criar_agendamento": "/agendamentos"
  }
}

##🔹 GET /telescopios
Respostar 200
{
  "telescopios": [
    {
      "id": 1,
      "nome": "Hubble-Acad",
      "descricao": "Telescópio acadêmico orbital",
      "status": "ativo",
      "_links": {
        "self": "/telescopios/1",
        "agendamentos": "/agendamentos?telescopio_id=1"
      }
    }
  ]
}

## 🔹 POST /agendamentos

Requisição
{
  "cientista_id": 1,
  "telescopio_id": 1,
  "horario_inicio_utc": "2025-12-01T03:00:00Z",
  "horario_fim_utc": "2025-12-01T03:05:00Z"
}

Resposta
{
  "agendamento_id": 123,
  "status": "criado",
  "_links": {
    "self": "/agendamentos/123",
    "cancelar": "/agendamentos/123/cancelar",
    "cientista": "/cientistas/1",
    "telescopio": "/telescopios/1"
  }
}


## 🔹 GET /agendamentos/{id}

Resposta
{
  "agendamento_id": 123,
  "cientista_id": 1,
  "telescopio_id": 1,
  "horario_inicio_utc": "2025-12-01T03:00:00Z",
  "horario_fim_utc": "2025-12-01T03:05:00Z",
  "status": "confirmado",
  "_links": {
    "cancelar": "/agendamentos/123/cancelar"
  }
}

## 🔹 POST /agendamentos/{id}/cancelar

{
  "agendamento_id": 123,
  "status": "cancelado",
  "_links": {
    "self": "/agendamentos/123",
    "reativar": "/agendamentos/123/reativar"
  }
}


## 🔹 GET /logs

Resposta

{
  "logs": [
    {
      "timestamp_utc": "2025-10-26T18:00:05.123Z",
      "level": "AUDIT",
      "event_type": "AGENDAMENTO_CRIADO"
    }
  ]
}



