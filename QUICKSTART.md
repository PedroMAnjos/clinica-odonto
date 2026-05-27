# 🚀 Guia de Início Rápido - DentalPro

> Começar com DentalPro em menos de 5 minutos

## 📋 Pré-requisitos

- Node.js 18+ instalado
- npm ou yarn
- Conta no Supabase (gratuita em supabase.com)
- Conta no Vercel (opcional, para deploy)

## 1️⃣ Setup Inicial

### Clonar o Repositório

```bash
git clone https://github.com/seu-usuario/dental-pro.git
cd dental-pro
```

### Instalar Dependências

```bash
npm install
```

## 2️⃣ Configurar Supabase

### Criar Projeto no Supabase

1. Acesse [supabase.com](https://supabase.com)
2. Clique em "New Project"
3. Selecione a região mais próxima
4. Aguarde a criação (leva alguns minutos)

### Obter Credenciais

1. Acesse Project Settings → API
2. Copie:
   - **Project URL** → `SUPABASE_URL`
   - **Anon Key** → `SUPABASE_ANON_KEY`

### Criar Banco de Dados

1. Acesse SQL Editor
2. Clique em "New Query"
3. Cole o conteúdo de `database.sql`
4. Clique em "Run"

Pronto! Todas as tabelas foram criadas.

## 3️⃣ Configurar Variáveis de Ambiente

### Criar arquivo .env

```bash
cp .env.example .env
```

### Preencher Variáveis

```env
NODE_ENV=development
PORT=3001
CORS_ORIGIN=*

# Gere uma chave aleatória
JWT_SECRET=seu_jwt_secret_super_secreto_aqui

# Do Supabase
SUPABASE_URL=https://xxxxx.supabase.co
SUPABASE_ANON_KEY=eyJxxxx...
```

### Gerar JWT_SECRET

```bash
# No terminal
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Copie o resultado para `JWT_SECRET`

## 4️⃣ Iniciar Desenvolvimento

### Terminal 1 - Backend (API)

```bash
npm run dev
```

Você verá:
```
╔════════════════════════════════════════════╗
║     🚀 DentalPro API Server Started        ║
║     🔗 http://localhost:3001               ║
║     📡 Ready for connections               ║
╚════════════════════════════════════════════╝
```

### Abrir em Navegador

- **Landing Page**: http://localhost:3001
- **Dashboard**: http://localhost:3001/src/pages/dashboard.html
- **API Health**: http://localhost:3001/api/health

## 5️⃣ Testar Funcionalidades

### Criar Conta

1. Clique em "Começar Grátis" na landing page
2. Preencha:
   - Nome da Clínica
   - Email
   - Senha
3. Clique em "Criar Conta Gratuitamente"

### Fazer Login

1. Clique em "Login" na navbar
2. Use as credenciais que acabou de criar
3. Você será redirecionado para o dashboard

### Explorar Dashboard

- **Stats Cards**: Visualize métricas principais
- **Gráficos**: Faturamento e dados em tempo real
- **Agenda**: Próximas consultas
- **Tabela de Pacientes**: Lista de pacientes

## 📡 Testar API com cURL

### Health Check

```bash
curl http://localhost:3001/api/health
```

### Register

```bash
curl -X POST http://localhost:3001/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "clinic@example.com",
    "password": "senha123456",
    "clinicName": "Minha Clínica"
  }'
```

### Login

```bash
curl -X POST http://localhost:3001/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "clinic@example.com",
    "password": "senha123456"
  }'
```

Você receberá um token. Use-o para autenticar em outras requisições:

```bash
curl http://localhost:3001/api/patients \
  -H "Authorization: Bearer seu_token_aqui"
```

## 🎨 Customizar Design

### Cores Principais

Edite `src/css/base.css`:

```css
:root {
  --color-primary: #10b981;        /* Verde principal */
  --color-accent: #06b6d4;         /* Ciano */
}
```

### Dark Mode

O dark mode é automático baseado em preferências do SO. Para forçar:

```javascript
// No console do navegador
document.body.classList.toggle('dark-mode');
```

## 📱 Responsividade

O sistema é totalmente responsivo para:

- ✅ Desktop (1920px+)
- ✅ Notebook (1024px)
- ✅ Tablet (768px)
- ✅ Mobile (320px)

Teste com F12 → Toggle Device Toolbar

## 🚀 Deploy na Vercel

### 1. Push para GitHub

```bash
git add .
git commit -m "Initial commit"
git push origin main
```

### 2. Conectar Vercel

```bash
npm run deploy
```

### 3. Adicionar Variáveis de Ambiente

No dashboard da Vercel:
- Project Settings → Environment Variables
- Adicione as mesmas do `.env`

Pronto! Seu sistema está online!

## 🐛 Troubleshooting

### Erro: "Cannot find module 'express'"

```bash
npm install
```

### Erro: "SUPABASE_URL is not defined"

Verifique se `.env` está preenchido corretamente:
```bash
cat .env  # Ver conteúdo
```

### Erro: "Token inválido"

O JWT_SECRET pode estar errado. Gere um novo:
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

### CORS Error

Se receber erro de CORS, atualize `CORS_ORIGIN` no `.env`:
```env
CORS_ORIGIN=http://localhost:3000,http://localhost:3001
```

### Porta já em uso

```bash
# Usar outra porta
PORT=3002 npm run dev
```

## 📚 Próximos Passos

1. **Criar primeira clínica** via painel
2. **Adicionar dentistas** em Settings
3. **Importar pacientes** (CSV)
4. **Configurar agendamentos** para dentistas
5. **Integrar pagamentos** (Stripe)
6. **Enviar ao Vercel** para produção

## 📖 Documentação Completa

- [README.md](README.md) - Documentação principal
- [database.sql](database.sql) - Schema do banco
- [.env.example](.env.example) - Variáveis de ambiente

## 💬 Suporte

- 📧 Email: dev@dentalpro.com
- 🐛 Issues: GitHub Issues
- 📞 Chat: Discord (link no README)

## ✅ Checklist de Setup

- [ ] Node.js 18+ instalado
- [ ] Repositório clonado
- [ ] `npm install` executado
- [ ] Projeto Supabase criado
- [ ] `.env` preenchido
- [ ] `database.sql` executado
- [ ] `npm run dev` iniciado
- [ ] Landing page abre em http://localhost:3001
- [ ] Dashboard funciona
- [ ] API responde em /api/health

Você está pronto! 🎉
