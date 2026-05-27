# 📡 Documentação da API DentalPro

> API RESTful completa para gestão de clínicas odontológicas

## 📝 Base URL

```
http://localhost:3001/api
```

## 🔐 Autenticação

Todas as rotas, exceto `/auth/*`, requerem um JWT token no header:

```bash
Authorization: Bearer YOUR_JWT_TOKEN_HERE
```

O token é obtido ao fazer login ou registrar uma clínica.

---

## 🚀 Endpoints

### 📌 Health Check

#### GET /health

Verifica se o servidor está funcionando.

**Request:**
```bash
curl http://localhost:3001/api/health
```

**Response:**
```json
{
  "status": "ok",
  "timestamp": "2024-05-27T10:30:00Z",
  "version": "1.0.0"
}
```

---

## 🔑 Autenticação

### Register - Criar Nova Clínica

#### POST /auth/register

Cria uma nova conta de clínica e usuário administrador.

**Request:**
```bash
curl -X POST http://localhost:3001/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@clinic.com",
    "password": "SecurePassword123!",
    "clinicName": "Clínica Premium"
  }'
```

**Request Body:**
```json
{
  "email": "string (required)",
  "password": "string (required, min 8 chars)",
  "clinicName": "string (required)"
}
```

**Response:**
```json
{
  "message": "Clínica registrada com sucesso",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "admin@clinic.com",
    "clinic": {
      "id": "550e8400-e29b-41d4-a716-446655440001",
      "name": "Clínica Premium",
      "status": "active",
      "plan": "free"
    }
  }
}
```

**Status Codes:**
- `201` - Criado com sucesso
- `400` - Email, senha ou nome da clínica inválidos
- `409` - Email já registrado

---

### Login

#### POST /auth/login

Faz login com email e senha.

**Request:**
```bash
curl -X POST http://localhost:3001/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@clinic.com",
    "password": "SecurePassword123!"
  }'
```

**Request Body:**
```json
{
  "email": "string (required)",
  "password": "string (required)"
}
```

**Response:**
```json
{
  "message": "Login realizado com sucesso",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "admin@clinic.com",
    "clinic": {
      "id": "550e8400-e29b-41d4-a716-446655440001",
      "name": "Clínica Premium",
      "status": "active",
      "plan": "free"
    }
  }
}
```

**Status Codes:**
- `200` - Login bem-sucedido
- `400` - Email ou senha inválidos
- `401` - Credenciais inválidas

---

## 👥 Pacientes

### Listar Todos os Pacientes

#### GET /patients

Lista todos os pacientes da clínica autenticada.

**Request:**
```bash
curl http://localhost:3001/api/patients \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Response:**
```json
{
  "success": true,
  "count": 25,
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "clinic_id": "550e8400-e29b-41d4-a716-446655440001",
      "full_name": "João Silva",
      "email": "joao@example.com",
      "phone": "(11) 98765-4321",
      "birth_date": "1990-05-15",
      "cpf": "123.456.789-00",
      "address": "Rua A, 123",
      "status": "active",
      "created_at": "2024-05-20T10:30:00Z"
    }
  ]
}
```

---

### Obter Paciente Específico

#### GET /patients/:id

Obtém informações detalhadas de um paciente.

**Request:**
```bash
curl http://localhost:3001/api/patients/550e8400-e29b-41d4-a716-446655440000 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "full_name": "João Silva",
    "email": "joao@example.com",
    "phone": "(11) 98765-4321",
    "appointments": [
      {
        "id": "550e8400-e29b-41d4-a716-446655440100",
        "appointment_date": "2024-05-28",
        "time": "10:30",
        "service": "Limpeza",
        "status": "scheduled"
      }
    ]
  }
}
```

---

### Criar Novo Paciente

#### POST /patients

Cria um novo paciente.

**Request:**
```bash
curl -X POST http://localhost:3001/api/patients \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "fullName": "Maria Santos",
    "email": "maria@example.com",
    "phone": "(11) 99876-5432",
    "birthDate": "1995-08-20",
    "cpf": "987.654.321-00",
    "address": "Avenida B, 456"
  }'
```

**Request Body:**
```json
{
  "fullName": "string (required)",
  "email": "string (optional)",
  "phone": "string (required)",
  "birthDate": "date (optional, YYYY-MM-DD)",
  "cpf": "string (optional, unique)",
  "address": "string (optional)"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Paciente criado com sucesso",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440002",
    "full_name": "Maria Santos",
    "email": "maria@example.com",
    "status": "active",
    "created_at": "2024-05-27T10:30:00Z"
  }
}
```

---

### Atualizar Paciente

#### PUT /patients/:id

Atualiza informações de um paciente.

**Request:**
```bash
curl -X PUT http://localhost:3001/api/patients/550e8400-e29b-41d4-a716-446655440000 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "fullName": "João Silva Santos",
    "phone": "(11) 98765-4322"
  }'
```

**Response:**
```json
{
  "success": true,
  "message": "Paciente atualizado com sucesso",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "full_name": "João Silva Santos",
    "phone": "(11) 98765-4322"
  }
}
```

---

### Deletar Paciente

#### DELETE /patients/:id

Deleta um paciente.

**Request:**
```bash
curl -X DELETE http://localhost:3001/api/patients/550e8400-e29b-41d4-a716-446655440000 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Response:**
```json
{
  "success": true,
  "message": "Paciente deletado com sucesso"
}
```

---

## 📅 Agendamentos

### Listar Agendamentos

#### GET /appointments

Lista todos os agendamentos da clínica.

**Request:**
```bash
curl http://localhost:3001/api/appointments \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Response:**
```json
{
  "success": true,
  "count": 12,
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440100",
      "clinic_id": "550e8400-e29b-41d4-a716-446655440001",
      "patient_id": "550e8400-e29b-41d4-a716-446655440000",
      "dentist_id": "550e8400-e29b-41d4-a716-446655440003",
      "appointment_date": "2024-05-28",
      "time": "10:30",
      "service": "Limpeza e Prevenção",
      "status": "scheduled",
      "patient": {
        "full_name": "João Silva",
        "phone": "(11) 98765-4321",
        "email": "joao@example.com"
      },
      "dentist": {
        "full_name": "Dr. Carlos"
      }
    }
  ]
}
```

---

### Criar Agendamento

#### POST /appointments

Cria um novo agendamento.

**Request:**
```bash
curl -X POST http://localhost:3001/api/appointments \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "patientId": "550e8400-e29b-41d4-a716-446655440000",
    "dentistId": "550e8400-e29b-41d4-a716-446655440003",
    "appointmentDate": "2024-05-28",
    "time": "14:00",
    "service": "Restauração",
    "notes": "Restauração em dente 24"
  }'
```

**Request Body:**
```json
{
  "patientId": "uuid (required)",
  "dentistId": "uuid (required)",
  "appointmentDate": "date (required, YYYY-MM-DD)",
  "time": "time (required, HH:MM)",
  "service": "string (required)",
  "notes": "string (optional)",
  "status": "string (optional, default: scheduled)"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Consulta agendada com sucesso",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440100",
    "appointment_date": "2024-05-28",
    "time": "14:00",
    "service": "Restauração",
    "status": "scheduled",
    "created_at": "2024-05-27T10:30:00Z"
  }
}
```

---

### Atualizar Agendamento

#### PUT /appointments/:id

Atualiza um agendamento.

**Request:**
```bash
curl -X PUT http://localhost:3001/api/appointments/550e8400-e29b-41d4-a716-446655440100 \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "status": "confirmed"
  }'
```

**Response:**
```json
{
  "success": true,
  "message": "Consulta atualizada com sucesso",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440100",
    "status": "confirmed"
  }
}
```

---

## 👨‍⚕️ Dentistas

### Listar Dentistas

#### GET /dentists

Lista todos os dentistas da clínica.

**Request:**
```bash
curl http://localhost:3001/api/dentists \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Response:**
```json
{
  "success": true,
  "count": 3,
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440003",
      "full_name": "Dr. Carlos Silva",
      "email": "carlos@clinic.com",
      "phone": "(11) 3456-7890",
      "specialization": "Implantodontia",
      "license": "CRO-SP 123456",
      "status": "active"
    }
  ]
}
```

---

### Criar Dentista

#### POST /dentists

Adiciona um novo dentista à clínica.

**Request:**
```bash
curl -X POST http://localhost:3001/api/dentists \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "fullName": "Dra. Ana Costa",
    "email": "ana@clinic.com",
    "phone": "(11) 3456-7891",
    "specialization": "Ortodontia",
    "license": "CRO-SP 789012"
  }'
```

**Response:**
```json
{
  "success": true,
  "message": "Dentista adicionado com sucesso",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440004",
    "full_name": "Dra. Ana Costa",
    "specialization": "Ortodontia",
    "status": "active"
  }
}
```

---

## 📊 Dashboard

### Obter Estatísticas

#### GET /stats

Obtém estatísticas principais do dashboard.

**Request:**
```bash
curl http://localhost:3001/api/stats \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Response:**
```json
{
  "success": true,
  "stats": {
    "totalPatients": 45,
    "todayAppointments": 8,
    "totalRevenue": 18540.50,
    "averageRating": 4.9
  }
}
```

---

## 🔄 Códigos de Status HTTP

| Código | Significado |
|--------|------------|
| 200 | OK - Requisição bem-sucedida |
| 201 | Created - Recurso criado |
| 400 | Bad Request - Dados inválidos |
| 401 | Unauthorized - Token inválido/expirado |
| 404 | Not Found - Recurso não encontrado |
| 409 | Conflict - Duplicata (ex: email já existe) |
| 500 | Server Error - Erro interno |

---

## ⚠️ Tratamento de Erros

Todas as respostas de erro seguem este formato:

```json
{
  "error": "Descrição do erro",
  "message": "Mensagem detalhada (apenas em desenvolvimento)"
}
```

### Exemplo de Erro

```json
{
  "error": "Token inválido"
}
```

---

## 🔑 Headers Recomendados

```bash
Content-Type: application/json
Authorization: Bearer YOUR_JWT_TOKEN
```

---

## 📚 Rate Limiting

- Sem limite específico em desenvolvimento
- Em produção: 100 requisições por minuto por IP
- Você receberá erro 429 se exceder o limite

---

## 🚀 Exemplo Completo

```bash
# 1. Registrar clínica
TOKEN=$(curl -X POST http://localhost:3001/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "clinic@example.com",
    "password": "SecurePass123!",
    "clinicName": "Minha Clínica"
  }' | jq -r '.token')

# 2. Criar paciente
curl -X POST http://localhost:3001/api/patients \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{
    "fullName": "João da Silva",
    "phone": "(11) 98765-4321"
  }'

# 3. Obter estatísticas
curl http://localhost:3001/api/stats \
  -H "Authorization: Bearer $TOKEN"
```

---

## 📞 Suporte

Para dúvidas sobre a API:
- 📧 Email: api-support@dentalpro.com
- 📖 Documentação: [README.md](README.md)
- 🐛 Issues: GitHub Issues
