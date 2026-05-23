# 🎓 Hahaha School

Sua central inteligente de tarefas escolares - Uma plataforma educacional moderna com inteligência artificial para ajudar estudantes com tarefas escolares.

## ✨ Funcionalidades

### Para Estudantes
- **🤖 IA de Tarefas**: Resolva exercícios enviando prints ou imagens
- **📚 Tarefas Prontas**: Acesse uma biblioteca completa de tarefas já resolvidas
- **✍️ Humanizador**: Transforme textos em redações naturais e humanizadas
- **🔐 Sistema de Login**: Acesso seguro com keys de acesso

### Para Administradores
- **👥 Gestão de Usuários**: Criar, editar, ativar/desativar contas
- **🔑 Gestão de Keys**: Gerar keys de acesso com validade
- **📊 Dashboard**: Estatísticas em tempo real do sistema
- **📝 Gestão de Tarefas**: Adicionar e organizar tarefas prontas

## 🚀 Tecnologias

### Frontend
- **Next.js 14** - Framework React
- **TypeScript** - Tipagem estática
- **TailwindCSS** - Estilização
- **Framer Motion** - Animações
- **Lucide React** - Ícones
- **Axios** - Cliente HTTP
- **React Dropzone** - Upload de arquivos

### Backend
- **Node.js** - Runtime JavaScript
- **Express** - Framework web
- **Prisma ORM** - ORM para banco de dados
- **PostgreSQL** - Banco de dados
- **JWT** - Autenticação
- **bcrypt** - Hash de senhas
- **OpenAI API** - Integração com IA
- **Cloudinary** - Upload de imagens

## 📋 Pré-requisitos

- Node.js 18+ 
- PostgreSQL 14+
- npm ou yarn
- Conta OpenAI API
- Conta Cloudinary (opcional)

## 🔧 Instalação

### 1. Clone o repositório

```bash
git clone <repository-url>
cd hahaha-school
```

### 2. Configure o Backend

```bash
cd backend
npm install
```

Crie o arquivo `.env` baseado em `.env.example`:

```env
DATABASE_URL="postgresql://user:password@localhost:5432/hahaha_school?schema=public"
JWT_SECRET="your-super-secret-jwt-key"
JWT_EXPIRES_IN="7d"
OPENAI_API_KEY="your-openai-api-key"
CLOUDINARY_CLOUD_NAME="your-cloud-name"
CLOUDINARY_API_KEY="your-api-key"
CLOUDINARY_API_SECRET="your-api-secret"
PORT=5000
NODE_ENV="development"
FRONTEND_URL="http://localhost:3000"
```

Execute as migrações do Prisma:

```bash
npx prisma generate
npx prisma migrate dev
```

### 3. Configure o Frontend

```bash
cd frontend
npm install
```

Crie o arquivo `.env.local`:

```env
NEXT_PUBLIC_API_URL="http://localhost:5000/api"
```

### 4. Inicie os Servidores

**Backend (terminal 1):**
```bash
cd backend
npm run dev
```

**Frontend (terminal 2):**
```bash
cd frontend
npm run dev
```

Acesse:
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000

## 📁 Estrutura do Projeto

```
hahaha-school/
├── backend/
│   ├── prisma/
│   │   └── schema.prisma          # Schema do banco de dados
│   ├── src/
│   │   ├── controllers/           # Lógica de negócio
│   │   ├── middleware/            # Middleware de autenticação
│   │   ├── routes/                # Rotas da API
│   │   ├── utils/                 # Utilitários
│   │   └── server.js              # Servidor Express
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── app/                   # Páginas Next.js
│   │   │   ├── dashboard/         # Dashboard do aluno
│   │   │   ├── login/             # Página de login
│   │   │   └── register/          # Página de registro
│   │   ├── components/
│   │   │   ├── layout/            # Componentes de layout
│   │   │   └── ui/                # Componentes UI reutilizáveis
│   │   ├── lib/                   # Utilitários e API
│   │   └── styles/                # Estilos globais
│   ├── tailwind.config.ts
│   └── package.json
└── README.md
```

## 🔐 Autenticação

O sistema usa JWT para autenticação. Os usuários podem se registrar com uma key de acesso opcional fornecida pelo administrador.

### Criar Usuário Admin

Use o Prisma Studio ou execute uma query SQL para criar o primeiro usuário admin:

```bash
npx prisma studio
```

Ou via API:

```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@hahahaschool.com",
    "username": "admin",
    "password": "admin123"
  }'
```

Depois, atualize o role para 'OWNER' no banco de dados.

## 🎨 Design

O sistema utiliza um design dark mode premium com:
- Tons preto, cinza escuro e roxo neon
- Efeitos glow
- Glassmorphism
- Bordas arredondadas
- Animações suaves com Framer Motion
- Fonte Inter
- Responsivo para mobile e desktop

## 📱 Páginas

### Pública
- **Home**: Landing page com informações sobre a plataforma
- **Login**: Página de autenticação
- **Register**: Página de cadastro

### Dashboard (Aluno)
- **Home**: Visão geral com estatísticas
- **Tarefas Prontas**: Biblioteca de tarefas resolvidas
- **IA de Tarefas**: Resolver exercícios com IA
- **Humanizador**: Humanizar textos

### Dashboard (Admin)
- **Dashboard**: Estatísticas do sistema
- **Usuários**: Gerenciar contas
- **Keys**: Gerenciar keys de acesso
- **Tarefas**: Gerenciar tarefas prontas

## 🔧 API Endpoints

### Autenticação
- `POST /api/auth/register` - Registrar usuário
- `POST /api/auth/login` - Fazer login
- `GET /api/auth/me` - Obter usuário atual
- `POST /api/auth/logout` - Fazer logout

### Usuários (Admin)
- `GET /api/users/stats` - Estatísticas
- `GET /api/users` - Listar usuários
- `POST /api/users` - Criar usuário
- `PUT /api/users/:id` - Atualizar usuário
- `DELETE /api/users/:id` - Deletar usuário
- `POST /api/users/:id/reset-password` - Resetar senha

### Keys (Admin)
- `GET /api/keys` - Listar keys
- `POST /api/keys` - Criar key
- `PUT /api/keys/:id` - Atualizar key
- `DELETE /api/keys/:id` - Deletar key
- `GET /api/keys/validate/:key` - Validar key

### Tarefas
- `GET /api/tasks` - Listar tarefas
- `GET /api/tasks/favorites` - Tarefas favoritas
- `GET /api/tasks/:id` - Obter tarefa
- `POST /api/tasks` - Criar tarefa (Admin)
- `PUT /api/tasks/:id` - Atualizar tarefa (Admin)
- `DELETE /api/tasks/:id` - Deletar tarefa (Admin)
- `POST /api/tasks/:id/favorite` - Favoritar/desfavoritar

### IA
- `POST /api/ai/solve` - Resolver tarefa com IA
- `POST /api/ai/humanize` - Humanizar texto
- `GET /api/ai/history` - Histórico de uso
- `GET /api/ai/stats` - Estatísticas de uso (Admin)

## 🚀 Deploy

### Backend (Vercel/Render)

1. Configure as variáveis de ambiente
2. Execute as migrações do Prisma
3. Deploy do código

### Frontend (Vercel)

1. Configure `NEXT_PUBLIC_API_URL` para a URL do backend em produção
2. Deploy automático via GitHub integration

### Banco de Dados

Use serviços como:
- Supabase (PostgreSQL gratuito)
- Neon (PostgreSQL serverless)
- Railway (PostgreSQL)

## 📝 Notas

- O sistema está configurado para desenvolvimento. Para produção, ajuste as variáveis de ambiente adequadamente.
- As chaves JWT devem ser mantidas em segredo.
- Configure rate limiting apropriado para produção.
- Use HTTPS em produção.
- Configure backups regulares do banco de dados.

## 🤝 Contribuindo

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está sob a licença MIT.

## 👥 Autores

- Hahaha School Team

## 🙏 Agradecimentos

- OpenAI pela API GPT
- Vercel pelo Next.js
- Comunidade open source
