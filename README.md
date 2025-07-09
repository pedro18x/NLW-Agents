# Agents

Projeto desenvolvido durante a NLW Agents por Pedro Ernesto.

## Descrição
O **Agents** é uma aplicação fullstack que permite a criação de salas para upload de áudios, transcrição automática e perguntas com respostas geradas por IA. O objetivo é facilitar o estudo e a consulta de conteúdos a partir de áudios, utilizando inteligência artificial para responder perguntas baseadas nas transcrições.

### Funcionalidades principais
- Criação de salas temáticas
- Upload e transcrição automática de áudios
- Geração de embeddings para busca semântica
- Envio de perguntas e respostas automáticas com IA (Google Gemini)
- Interface web moderna e responsiva

## Arquitetura

### Backend (Node.js + Fastify)
O backend é estruturado em camadas:

```
server/
├── src/
│   ├── db/           # Camada de dados (Drizzle ORM)
│   │   ├── migrations/   # Migrations do banco
│   │   ├── schema/      # Schema das tabelas
│   │   └── seed.ts      # Dados iniciais
│   ├── http/        # Camada HTTP (Fastify)
│   │   └── routes/      # Rotas da API
│   ├── services/    # Camada de serviços
│   │   └── gemini.ts    # Integração com IA
│   └── server.ts    # Entrada da aplicação
```

### Frontend (React + Vite)
O frontend segue uma arquitetura baseada em componentes:

```
web/
├── src/
│   ├── components/  # Componentes React
│   │   ├── ui/         # Componentes base
│   │   └── ...         # Componentes específicos
│   ├── http/       # Integração com API
│   ├── lib/        # Utilitários
│   └── pages/      # Páginas da aplicação
```

### Fluxo de Dados
1. **Upload de Áudio**
   - Áudio é enviado em chunks de 5 segundos
   - Cada chunk é transcrito pelo Google Gemini
   - Embeddings são gerados para busca semântica
   - Dados são armazenados no PostgreSQL com pgvector

2. **Perguntas e Respostas**
   - Usuário envia pergunta
   - Sistema gera embeddings da pergunta
   - Busca semântica encontra trechos relevantes
   - IA gera resposta contextualizada

## Stack utilizada

### Backend
- **Node.js**
- **Fastify** — framework web
- **Drizzle ORM** — ORM para banco de dados
- **PostgreSQL** — banco de dados relacional
- **Google Gemini API** — transcrição, embeddings e respostas com IA
- **Zod** — validação de dados
- **TypeScript**
- **Docker** — orquestração do banco de dados e ambiente de desenvolvimento

### Frontend
- **React**
- **Vite** — bundler
- **TypeScript**
- **TailwindCSS** — estilização
- **React Hook Form** — formulários
- **TanStack React Query** — gerenciamento de dados assíncronos
- **Radix UI** — componentes de acessibilidade
- **Day.js** — manipulação de datas
- **Lucide React** — ícones

## Como rodar o projeto

### 1. Clone o repositório

```bash
git clone <url-do-repositorio>
cd NLW-Agents
```

### 2. Suba o banco de dados (PostgreSQL + pgvector)

Abra o Docker Desktop e basta rodar:

```bash
cd server
docker-compose up -d
```

Isso irá subir um banco PostgreSQL já com a extensão `vector` habilitada.

### 3. Configure as variáveis de ambiente

Crie um arquivo `.env` na pasta `server` com o seguinte conteúdo:

```env
PORT=3333
DATABASE_URL=postgresql://docker:docker@localhost:5432/agents
GEMINI_API_KEY=<sua-chave-da-api-gemini>
```

- Substitua `<sua-chave-da-api-gemini>` pela sua chave da API do Google Gemini.

### 4. Instale as dependências

```bash
cd server
npm install
cd ../web
npm install
```

### 5. Rode as migrations e o seed do banco

No diretório `server`:

```bash
npm run db:migrate
npm run db:seed
```

### 6. Inicie o backend

No diretório `server`:

```bash
npm run dev
```

### 7. Inicie o frontend

Em outro terminal, no diretório `web`:

```bash
npm run dev
```

Acesse a aplicação em [http://localhost:5173](http://localhost:5173)

## API Endpoints

### Rooms
- `GET /rooms` - Lista todas as salas
- `POST /rooms` - Cria uma nova sala
- `GET /rooms/:roomId/questions` - Lista perguntas de uma sala
- `POST /rooms/:roomId/questions` - Cria pergunta em uma sala
- `POST /rooms/:roomId/audio` - Upload de áudio para uma sala

## Boas Práticas e Segurança

### Backend
- Validação de dados com Zod
- Rate limiting para proteção da API
- Sanitização de inputs
- Logs estruturados
- Tratamento de erros consistente
- Migrations para versionamento do banco

### Frontend
- Componentes reutilizáveis
- Gerenciamento de estado com React Query
- Formulários validados
- Design system consistente
- Feedback visual de loading/erro
- Responsividade

## Desenvolvimento

### Comandos úteis

```bash
# Backend
npm run dev         # Inicia em modo desenvolvimento
npm run db:generate # Gera nova migration
npm run db:migrate  # Aplica migrations
npm run db:seed     # Reseta e popula o banco

# Frontend
npm run dev    # Inicia em modo desenvolvimento
npm run build  # Build para produção
npm run preview # Preview do build
```

### Testando a API
- Use o arquivo `server/client.http` com o plugin REST Client do VSCode
- Ou importe a coleção para o Insomnia/Postman
- O frontend se comunica com o backend em `http://localhost:3333`

### Estrutura do Banco
- Tabela `rooms`: Salas de estudo
- Tabela `audio_chunks`: Trechos de áudio transcritos
- Tabela `questions`: Perguntas e respostas
- Extensão `pgvector`: Busca semântica


Desenvolvido por Pedro Ernesto durante a NLW Agents da [Rocketseat](https://app.rocketseat.com.br/). 
