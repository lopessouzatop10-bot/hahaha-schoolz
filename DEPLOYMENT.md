# 🚀 Guia de Deploy - Hahaha School

Este guia fornece instruções detalhadas para fazer deploy do Hahaha School em produção.

## 📋 Pré-requisitos

- Conta Vercel (para frontend)
- Conta Render/Railway/Heroku (para backend)
- Conta Supabase/Neon (para banco de dados PostgreSQL)
- Conta OpenAI (para API)
- Conta Cloudinary (para upload de imagens)

## 🔧 Configuração de Variáveis de Ambiente

### Backend (.env)

```env
# Banco de Dados
DATABASE_URL="postgresql://user:password@host:5432/database?schema=public"

# JWT
JWT_SECRET="sua-chave-secreta-super-forte-mude-isso"
JWT_EXPIRES_IN="7d"

# OpenAI
OPENAI_API_KEY="sk-..."

# Cloudinary
CLOUDINARY_CLOUD_NAME="seu-cloud-name"
CLOUDINARY_API_KEY="seu-api-key"
CLOUDINARY_API_SECRET="seu-api-secret"

# Servidor
PORT=5000
NODE_ENV="production"

# CORS
FRONTEND_URL="https://seu-dominio.com"
```

### Frontend (.env.local)

```env
NEXT_PUBLIC_API_URL="https://seu-backend-api.com/api"
```

## 🗄️ Configuração do Banco de Dados

### Opção 1: Supabase (Recomendado)

1. Crie uma conta em [supabase.com](https://supabase.com)
2. Crie um novo projeto
3. Vá em Settings > Database
4. Copie a connection string
5. Use como `DATABASE_URL`

### Opção 2: Neon

1. Crie uma conta em [neon.tech](https://neon.tech)
2. Crie um novo projeto PostgreSQL
3. Copie a connection string
4. Use como `DATABASE_URL`

### Opção 3: Railway

1. Crie uma conta em [railway.app](https://railway.app)
2. Crie um novo projeto PostgreSQL
3. Copie a connection string
4. Use como `DATABASE_URL`

## 🚀 Deploy do Backend

### Opção 1: Render

1. Crie uma conta em [render.com](https://render.com)
2. Crie um novo "Web Service"
3. Conecte seu repositório GitHub
4. Configure:
   - **Build Command**: `npm install`
   - **Start Command**: `node src/server.js`
   - **Environment Variables**: Adicione todas as variáveis do backend
5. Deploy

### Opção 2: Railway

1. Crie uma conta em [railway.app](https://railway.app)
2. Crie um novo projeto
3. Adicione um serviço "Web Service"
4. Conecte seu repositório GitHub
5. Configure as variáveis de ambiente
6. Deploy

### Opção 3: Heroku

1. Instale Heroku CLI: `npm install -g heroku`
2. Login: `heroku login`
3. Crie app: `heroku create hahaha-school-api`
4. Configure variáveis: `heroku config:set KEY=VALUE`
5. Deploy: `git push heroku main`

### Migrations do Prisma em Produção

Após o deploy, execute:

```bash
npx prisma migrate deploy
npx prisma generate
```

## 🎨 Deploy do Frontend

### Opção 1: Vercel (Recomendado)

1. Crie uma conta em [vercel.com](https://vercel.com)
2. Clique em "New Project"
3. Importe seu repositório GitHub
4. Configure:
   - **Framework Preset**: Next.js
   - **Environment Variables**: Adicione `NEXT_PUBLIC_API_URL`
5. Deploy

### Opção 2: Netlify

1. Crie uma conta em [netlify.com](https://netlify.com)
2. Conecte seu repositório GitHub
3. Configure:
   - **Build Command**: `npm run build`
   - **Publish Directory**: `.next`
   - **Environment Variables**: Adicione `NEXT_PUBLIC_API_URL`
4. Deploy

## 🔐 Configuração de Segurança

### 1. HTTPS

Certifique-se de que ambos frontend e backend usem HTTPS. Vercel e Render fornecem HTTPS automaticamente.

### 2. Environment Variables

- Nunca commit variáveis sensíveis no código
- Use variáveis de ambiente para todas as credenciais
- Rode as chaves JWT regularmente

### 3. Rate Limiting

O backend já possui rate limiting configurado. Ajuste os valores em `src/server.js` se necessário:

```javascript
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100 // 100 requests por windowMs
});
```

### 4. CORS

Configure o CORS para permitir apenas seu domínio frontend:

```javascript
app.use(cors({
  origin: process.env.FRONTEND_URL,
  credentials: true
}));
```

## 📊 Monitoramento

### Logs

- **Render**: Dashboard > Logs
- **Vercel**: Dashboard > Logs
- **Railway**: Dashboard > Logs

### Analytics

Considere adicionar:
- Google Analytics
- Vercel Analytics
- Sentry para error tracking

## 💾 Backups

### Banco de Dados

Configure backups automáticos:
- **Supabase**: Settings > Database > Backups
- **Neon**: Automático por padrão
- **Railway**: Settings > Backups

## 🔄 CI/CD

### GitHub Actions

Crie `.github/workflows/deploy.yml`:

```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to Render
        run: curl <render-webhook-url>

  deploy-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to Vercel
        run: vercel --prod --token=${{ secrets.VERCEL_TOKEN }}
```

## 🧪 Testes em Produção

### 1. Teste de Conexão

```bash
curl https://seu-backend.com/api/health
```

### 2. Teste de Autenticação

```bash
curl -X POST https://seu-backend.com/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@test.com","password":"test123"}'
```

### 3. Teste de Frontend

Acesse `https://seu-dominio.com` e verifique:
- Carregamento da página
- Funcionalidade de login
- Dashboard funcional

## 🐛 Solução de Problemas

### Backend não inicia

1. Verifique os logs
2. Confirme que todas as variáveis de ambiente estão configuradas
3. Verifique a conexão com o banco de dados
4. Execute `npx prisma migrate deploy`

### Frontend não conecta ao backend

1. Verifique `NEXT_PUBLIC_API_URL`
2. Confirme que o backend está rodando
3. Verifique configuração de CORS
4. Teste a API diretamente

### Erros de Prisma

1. Execute `npx prisma generate`
2. Execute `npx prisma migrate deploy`
3. Verifique `DATABASE_URL`

## 📈 Escalabilidade

### Backend

- Use load balancer para múltiplas instâncias
- Configure cache (Redis)
- Use CDN para arquivos estáticos

### Banco de Dados

- Monitore uso de recursos
- Ajuste pool de conexões
- Considere read replicas para leitura

## 🎯 Checklist de Deploy

- [ ] Variáveis de ambiente configuradas
- [ ] Banco de dados criado e migrations executadas
- [ ] Backend deployado e testado
- [ ] Frontend deployado e testado
- [ ] HTTPS configurado
- [ ] CORS configurado corretamente
- [ ] Rate limiting ativo
- [ ] Logs funcionando
- [ ] Backups configurados
- [ ] Monitoramento configurado
- [ ] Domínios configurados
- [ ] Testes de integração passados

## 📞 Suporte

Para problemas de deploy, verifique:
- Documentação da plataforma de hosting
- Logs do servidor
- Status da API OpenAI
- Status do banco de dados
