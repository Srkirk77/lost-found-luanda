# Achados Luanda

Plataforma comunitária para publicar, procurar e devolver objectos perdidos ou encontrados em Luanda. O projecto reúne uma aplicação web, uma API REST e uma base de dados PostgreSQL.

**Live preview:** [lost-found-luanda.vercel.app](https://lost-found-luanda.vercel.app/)

## Funcionalidades

- Publicar ocorrências de objectos perdidos, encontrados e avisos.
- Pesquisar ocorrências e consultar detalhes, imagens e localização.
- Criar conta, iniciar sessão e gerir o perfil.
- Guardar ocorrências nos favoritos e receber notificações.
- Iniciar conversas entre utilizadores.
- Sugerir correspondências entre ocorrências.
- Disponibilizar ferramentas administrativas para gerir ocorrências, utilizadores, categorias, municípios, denúncias e configurações.

## Estrutura do projecto

```text
.
├── client/                 # Aplicação web React e TanStack Start
│   └── src/
│       ├── components/      # Componentes reutilizáveis
│       ├── hooks/           # Hooks partilhados
│       ├── lib/             # API, autenticação e utilitários
│       └── routes/          # Páginas e rotas da aplicação
├── docs/                   # Diagramas e documentação do projecto
└── server/                 # API NestJS
    ├── prisma/              # Modelo, migrações e dados de demonstração
    └── src/                 # Módulos da API
```

## Tecnologias

**Frontend:** React 19, TypeScript, TanStack Start/Router, Vite, Tailwind CSS e Radix UI.

**Backend:** NestJS 11, TypeScript, Prisma, PostgreSQL, autenticação JWT e Cloudinary para imagens.

## Requisitos

- Node.js 20.19+ (ou 22.12+) e npm.
- PostgreSQL acessível pelo backend.
- Conta Cloudinary para carregar imagens fora do modo de demonstração.
- Credenciais SMTP, caso seja necessário enviar e-mails. Sem estas credenciais, os avisos de e-mail são registados nos logs do servidor.

## Executar localmente

O cliente e o servidor são aplicações independentes; instale as dependências em cada pasta.

### 1. Configurar o backend

```bash
cd server
npm ci
```

Crie `server/.env` com os valores do seu ambiente:

```dotenv
PORT=7000
DATABASE_URL="postgresql://UTILIZADOR:SENHA@HOST:5432/NOME_DA_BASE?schema=public"
JWT_SECRET="substitua-por-um-segredo-aleatorio-e-forte"
JWT_EXPIRES_IN="7d"

CLOUDINARY_CLOUD_NAME=""
CLOUDINARY_API_KEY=""
CLOUDINARY_API_SECRET=""

# Opcional: configuração de e-mail
SMTP_HOST="smtp.gmail.com"
SMTP_PORT=587
EMAIL_USER=""
EMAIL_PASS=""

# Configure credenciais próprias para proteger a documentação Swagger
SWAGGER_USER=""
SWAGGER_PASS=""
SWAGGER_KEY=""
```

Use um segredo JWT forte e credenciais próprias; não publique o ficheiro `.env`. As variáveis Cloudinary são necessárias para carregamentos reais de imagens. A configuração SMTP é opcional.

Com a base de dados criada e `DATABASE_URL` configurada, aplique as migrações e gere o cliente Prisma:

```bash
npx prisma migrate deploy
npm run prisma:generate
```

Inicie o backend:

```bash
npm run start:dev
```

A API fica disponível em `http://localhost:7000/api`. A documentação Swagger encontra-se em `http://localhost:7000/api/docs` e está protegida pelas credenciais/chave configuradas para o servidor.

> **Portas:** o cliente está actualmente configurado para chamar `http://localhost:7000/api` em `client/src/lib/api.ts`. O backend usa a porta `5000` por omissão; por isso, no desenvolvimento local, configure `PORT=7000` como no exemplo ou altere o endereço da API no cliente para corresponder à porta escolhida.

### 2. Configurar e iniciar o frontend

Noutro terminal:

```bash
cd client
npm ci
npm run dev
```

Abra no navegador o endereço indicado pelo Vite, normalmente `http://localhost:5173`.

## Comandos úteis

### Frontend (`client/`)

```bash
npm run dev       # inicia o servidor de desenvolvimento
npm run build     # cria a build de produção
npm run preview   # pré-visualiza a build
npm run lint      # executa o ESLint
```

### Backend (`server/`)

```bash
npm run start:dev          # inicia em modo de desenvolvimento
npm run build              # compila a aplicação
npm run start:prod         # inicia a versão compilada
npm run prisma:generate    # gera o cliente Prisma
npx prisma migrate deploy  # aplica migrações existentes
```

O comando `npm run prisma:seed` carrega dados de demonstração e **apaga os dados existentes** antes de os recriar. Use-o apenas numa base de dados descartável ou local, nunca numa base de produção.

## API e dados

A API usa rotas sob o prefixo `/api`. Entre os recursos disponíveis estão autenticação, ocorrências, categorias, municípios, utilizadores, favoritos, notificações, conversas, denúncias e administração. O modelo de dados está definido em [`server/prisma/schema.prisma`](./server/prisma/schema.prisma).

## Documentação adicional

- [Documentação do frontend](./client/README.md)
- Diagramas do projecto: [diagrama de contexto](<./docs/Diagrama de contexto.png>), [modelo entidade-relacionamento](./docs/mer.jpeg) e [diagrama de casos de uso](./docs/diagrama_caso_uso_plataforma_fundo_branco.png).
# lost-found-luanda
