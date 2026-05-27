# 👥 Sistema de Autenticação Duplo - DentalPro

## 📋 Visão Geral

A plataforma DentalPro agora suporta dois tipos completamente separados de usuários:

1. **👨‍⚕️ Clínicas/Consultórios** - Gerenciam pacientes, agendamentos, dentistas
2. **👤 Pacientes** - Agendamento de consultas, histórico médico, perfil

---

## 🔐 Fluxo de Autenticação

### Landing Page - Seleção de Tipo de Usuário

```
Login/Signup → Selecionar Tipo (Modal)
    ↓
┌─────────────────────┬─────────────────────┐
│   Clínica           │   Paciente          │
└─────────────────────┴─────────────────────┘
```

### Endpoints de Autenticação Clínica

```
POST /api/auth/register
- Email de administrador
- Senha
- Nome da clínica
Response: JWT Token + Clinic Data

POST /api/auth/login
- Email de administrador
- Senha
Response: JWT Token + Clinic Data
```

### Endpoints de Autenticação Paciente

```
POST /api/patient/register
- Email
- Senha
- Nome (firstName)
- Sobrenome (lastName)
- Telefone (opcional)
Response: JWT Token + Patient Profile

POST /api/patient/login
- Email
- Senha
Response: JWT Token + Patient Profile
```

---

## 🎯 Estrutura de Dados

### Clínica (Clinic)
```javascript
{
  id: UUID,
  name: "Clínica Premium",
  user_id: UUID,
  status: "active",
  plan: "free|pro|enterprise",
  created_at: timestamp,
  updated_at: timestamp
}
```

### Paciente (Patient)
```javascript
{
  id: UUID,
  clinic_id: UUID,
  user_id: UUID,
  email: "email@example.com",
  first_name: "João",
  last_name: "Silva",
  phone: "+55 11 99999-9999",
  date_of_birth: "1990-01-15",
  address: "Rua X, 123",
  status: "active|inactive",
  created_at: timestamp,
  updated_at: timestamp
}
```

---

## 🎨 Interfaces de Usuário

### Para Clínicas

**Landing Page:**
- Hero com foco em gestão
- Features voltadas para administração
- Depoimentos de clínicas
- CTA "Começar Grátis" (para clínica)

**Dashboard:**
- Sidebar com navegação
- Stats de consultas, faturamento, pacientes
- Tabela de pacientes
- Lista de agendamentos
- Gráficos e relatórios

### Para Pacientes

**Landing Page:**
- Mesma landing (mas com login para paciente)
- CTA leva ao dashboard do paciente

**Dashboard de Paciente:**
- Cards com próximas consultas
- Saldo pendente
- Dentistas disponíveis
- Rating de saúde bucal
- Prontuário médico
- Histórico de consultas

**Página de Agendamento:**
- Step 1: Selecionar dentista
- Step 2: Escolher data (calendário)
- Step 3: Horário e tipo de consulta
- Step 4: Confirmação

---

## 🔄 JWT Token Structure

### Token de Clínica
```json
{
  "id": "user-id",
  "email": "admin@clinic.com",
  "clinicId": "clinic-id",
  "role": "clinic",
  "iat": 1234567890,
  "exp": 1234567890
}
```

### Token de Paciente
```json
{
  "id": "user-id",
  "email": "patient@email.com",
  "patientId": "patient-id",
  "role": "patient",
  "iat": 1234567890,
  "exp": 1234567890
}
```

---

## 🛣️ Rotas Disponíveis

### Autenticação
```
POST   /api/auth/register        (Clínica)
POST   /api/auth/login           (Clínica)
POST   /api/patient/register     (Paciente)
POST   /api/patient/login        (Paciente)
```

### Gerenciamento de Clínica
```
GET    /api/patients             (Listar pacientes)
POST   /api/patients             (Criar paciente)
GET    /api/patients/:id         (Obter paciente)
PUT    /api/patients/:id         (Atualizar paciente)
DELETE /api/patients/:id         (Deletar paciente)

GET    /api/appointments         (Listar agendamentos)
POST   /api/appointments         (Criar agendamento)
PUT    /api/appointments/:id     (Atualizar agendamento)

GET    /api/dentists             (Listar dentistas)
POST   /api/dentists             (Adicionar dentista)

GET    /api/stats                (Estatísticas)
```

### Gerenciamento de Paciente
```
GET    /api/patient/profile      (Obter perfil)
PUT    /api/patient/profile      (Atualizar perfil)

GET    /api/patient/appointments (Listar minhas consultas)
POST   /api/patient/appointments (Agendar consulta)
PUT    /api/patient/appointments/:id/cancel (Cancelar consulta)

GET    /api/patient/medical-records (Prontuário)

GET    /api/patient/dentists     (Dentistas disponíveis)
```

---

## 📁 Estrutura de Arquivos - Paciente

```
src/
├── pages/
│   ├── patient-dashboard.html    # Dashboard principal do paciente
│   └── booking.html              # Agendamento de consultas
├── css/
│   ├── patient-dashboard.css     # Estilos dashboard
│   └── booking.css               # Estilos agendamento
└── js/
    ├── patient-dashboard.js      # Interatividade dashboard
    └── booking.js                # Interatividade agendamento
```

---

## 🔑 Local Storage

### Armazenar Tipo de Usuário
```javascript
localStorage.setItem('userType', 'clinic' | 'patient');
```

### Armazenar Token
```javascript
localStorage.setItem('authToken', 'jwt-token-aqui');
```

### Recuperar Dados
```javascript
const userType = localStorage.getItem('userType');
const token = localStorage.getItem('authToken');
```

---

## 🚀 Fluxo de Login Completo

### 1. Landing Page - Selecionar Tipo
```javascript
openUserTypeModal('login'); // Abre modal de seleção
selectUserType('patient', 'login'); // Salva tipo e abre login
```

### 2. Modal de Login
```
Email: patient@example.com
Senha: senha123
```

### 3. Backend - Autentica
```bash
POST /api/patient/login
{
  "email": "patient@example.com",
  "password": "senha123"
}
```

### 4. Response
```json
{
  "message": "Login realizado com sucesso",
  "token": "eyJhbGc...",
  "user": {
    "id": "user-123",
    "email": "patient@example.com",
    "patient": {
      "id": "patient-456",
      "first_name": "João",
      "last_name": "Silva"
    }
  }
}
```

### 5. Frontend - Salva Token
```javascript
localStorage.setItem('authToken', response.token);
localStorage.setItem('userType', 'patient');
window.location.href = 'src/pages/patient-dashboard.html';
```

### 6. Dashboard - Usa Token
```javascript
const token = localStorage.getItem('authToken');
fetch('/api/patient/profile', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
})
```

---

## 🛡️ Middleware de Autenticação

Todos os endpoints de paciente usam o middleware `verifyToken`:

```javascript
const verifyToken = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ error: 'Token não fornecido' });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(401).json({ error: 'Token inválido' });
  }
};
```

---

## 🎨 Componentes Paciente

### Dashboard Paciente
- Navbar com perfil e notificações
- Sidebar colapsável com menu
- Stat cards (próximas consultas, saldo, dentistas, saúde)
- Lista de próximas consultas
- Recomendações de ações
- Prontuário médico

### Página de Agendamento
- 4 passos (stepper)
- Seleção de dentista com avatares
- Calendário interativo
- Seleção de horário
- Seleção de tipo de consulta
- Confirmação com resumo
- Mensagem de sucesso

---

## 📊 Fluxo de Dados

```
Paciente → Landing Page
    ↓
Clica em "Login" → Seleciona "Paciente"
    ↓
Modal de Login → Email + Senha
    ↓
API POST /api/patient/login
    ↓
Supabase Auth valida credenciais
    ↓
Backend gera JWT token
    ↓
Frontend salva token + tipo
    ↓
Redireciona para patient-dashboard.html
    ↓
Dashboard carrega dados via API
    ↓
Paciente vê suas consultas, perfil, etc
```

---

## 🔄 Fluxo de Agendamento

```
Paciente no Dashboard
    ↓
Clica "Agendar Consulta" ou nav "Agendar"
    ↓
booking.html → Step 1: Seleciona Dentista
    ↓
Step 2: Escolhe Data (Calendário)
    ↓
Step 3: Horário + Tipo de Consulta
    ↓
Step 4: Confirmação + Notas
    ↓
POST /api/patient/appointments
    ↓
Consulta criada no banco
    ↓
Mensagem de sucesso
    ↓
Voltar ao Dashboard
```

---

## 🔐 Segurança

### Role-Based Access Control
```javascript
if (req.user.role !== 'patient') {
  return res.status(403).json({ error: 'Acesso negado' });
}
```

### Patient Isolation
- Paciente só vê suas próprias consultas
- Paciente só vê seus dados pessoais
- Acesso restrito a endpoints de paciente

### JWT Expiration
- Token expira em 30 dias
- Token renovável fazendo novo login
- Token armazenado localmente

---

## 📱 Responsividade

- ✅ Desktop: 1920px+
- ✅ Notebook: 1024px
- ✅ Tablet: 768px
- ✅ Mobile: 320px+

Todas as páginas do paciente são totalmente responsivas com breakpoints CSS.

---

## 🎯 Próximos Passos

1. **Confirmação por Email**
   - Enviar email ao agendar consulta
   - Link de confirmação
   - Lembretes automáticos

2. **Integração com Pagamento**
   - Stripe para pagamento
   - Histórico de transações
   - Faturas

3. **Notificações**
   - SMS para lembretes
   - Push notifications
   - In-app notifications

4. **Mobile App**
   - React Native
   - App iOS/Android
   - Sincronização de dados

5. **Análises**
   - Histórico completo de consultas
   - Relatórios de saúde
   - Tendências

---

## 📞 Suporte

Para dúvidas sobre a autenticação dupla, consulte:
- API.md - Documentação de endpoints
- README.md - Documentação geral
- Code comments - Código comentado

---

**DentalPro v1.0.0** ✨  
*Plataforma SaaS Premium para Clínicas Odontológicas*
