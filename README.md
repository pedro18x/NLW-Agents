# 🤖 Agents

> Projeto desenvolvido por **Pedro Ernesto** durante a **NLW Agents** da [Rocketseat](https://rocketseat.com.br)

## 📋 Sobre o Projeto

O **Agents** é uma aplicação fullstack inovadora que utiliza inteligência artificial para criar um sistema de perguntas e respostas baseado em conteúdo de áudio. A aplicação permite que usuários criem salas temáticas, gravem áudios diretamente pelo navegador (não há upload de arquivos), e posteriormente façam perguntas sobre o conteúdo, recebendo respostas precisas geradas por IA.


## ✨ Funcionalidades

### 🏠 Gestão de Salas
- **Criação de salas**: Organize conteúdos por temas ou assuntos
- **Listagem de salas**: Visualize todas as salas disponíveis
- **Navegação intuitiva**: Interface clean para fácil navegação

### 🎙️ Gravação e Processamento de Áudio
- **Gravação de áudio**: O usuário grava o áudio diretamente pelo navegador 
- **Transcrição automática**: Conversão de áudio para texto usando Google Gemini
- **Processamento em chunks**: Áudios são processados em pedaços de 5 segundos
- **Geração de embeddings**: Criação de vetores para busca semântica

### ❓ Sistema de Perguntas e Respostas
- **Perguntas contextuais**: Faça perguntas sobre o conteúdo do áudio
- **Busca semântica**: Encontra trechos relevantes usando similaridade de embeddings
- **Respostas inteligentes**: IA gera respostas baseadas no contexto das transcrições
- **Histórico de perguntas**: Visualize todas as perguntas e respostas da sala

### 🎤 Gravação em Tempo Real
- **Gravação de áudio**: Capture áudio diretamente pelo navegador
- **Processamento contínuo**: Áudio é processado em tempo real
- **Interface de gravação**: Controles intuitivos para iniciar/parar gravação

## 🛠️ Stack Tecnológica

### Backend
- **Node.js** - Runtime JavaScript
- **TypeScript** - Superset tipado do JavaScript
- **Fastify** - Framework web rápido e eficiente
- **Drizzle ORM** - ORM moderno para TypeScript
- **PostgreSQL** - Banco de dados relacional
- **pgvector** - Extensão para busca vetorial
- **Google Gemini API** - IA para transcrição, embeddings e respostas
- **Zod** - Validação de dados e schemas
- **Docker** - Containerização do ambiente

### Frontend
- **React** - Biblioteca para interfaces de usuário
- **TypeScript** - Tipagem estática
- **Vite** - Build tool moderna e rápida
- **TailwindCSS** - Framework CSS utilitário
- **React Hook Form** - Gerenciamento de formulários
- **TanStack React Query** - Gerenciamento de estado servidor
- **Radix UI** - Componentes acessíveis
- **Lucide React** - Ícones modernos
- **React Router DOM** - Roteamento
- **Day.js** - Manipulação de datas

### Ferramentas de Desenvolvimento
- **Biome** - Linter e formatter
- **Drizzle Kit** - Migrations e introspection
- **Ultracite** - Utilitários de desenvolvimento

## 🏗️ Arquitetura

### Backend
```
server/
├── src/
│   ├── db/                 # Camada de dados
│   │   ├── migrations/     # Migrations do banco
│   │   ├── schema/         # Definição das tabelas
│   │   └── seed.ts         # Dados iniciais
│   ├── http/               # Camada HTTP
│   │   └── routes/         # Rotas da API
│   ├── services/           # Camada de serviços
│   │   └── gemini.ts       # Integração com IA
│   ├── env.ts              # Configuração de ambiente
│   └── server.ts           # Entrada da aplicação
├── docker/                 # Configuração Docker
└── docker-compose.yml      # Orquestração de containers
```

### Frontend
```
web/
├── src/
│   ├── components/         # Componentes React
│   │   ├── ui/             # Componentes base
│   │   └── ...             # Componentes específicos
│   ├── http/               # Integração com API
│   ├── lib/                # Utilitários
│   ├── pages/              # Páginas da aplicação
│   └── main.tsx            # Entrada da aplicação
└── index.html              # Template HTML
```

## 🔄 Fluxo de Funcionamento

### 1. Gravação e Processamento de Áudio
1. Usuário grava áudio diretamente pelo navegador (não há upload de arquivos)
2. Áudio é capturado em chunks de 5 segundos em tempo real
3. Cada chunk é convertido para Base64 e enviado para o backend
4. Google Gemini transcreve cada chunk para texto
5. Sistema gera embeddings da transcrição
6. Dados são armazenados no PostgreSQL com pgvector

### 2. Sistema de Perguntas
1. Usuário faz uma pergunta sobre o conteúdo gravado
2. Sistema gera embeddings da pergunta
3. Busca semântica encontra trechos relevantes (similaridade > 0.7)
4. Google Gemini gera resposta contextualizada
5. Resposta é retornada e armazenada

### 3. Busca Semântica
- Utiliza embeddings para encontrar conteúdo similar
- Algoritmo de similaridade coseno para matching
- Threshold de 0.7 para relevância
- Retorna até 3 trechos mais relevantes

## 🚀 Como Executar

### Pré-requisitos
- Node.js 18+
- Docker e Docker Compose
- Chave da API do Google Gemini
- **Navegador com suporte a MediaRecorder API** (Chrome, Firefox, Edge)

### 1. Clone o repositório
```bash
git clone <url-do-repositorio>
cd agents
```

### 2. Suba o banco de dados (PostgreSQL + pgvector)

Se você tem Docker instalado, basta rodar:

```bash
cd server
docker-compose up -d
```

### 3. Configure as variáveis de ambiente
Crie um arquivo `.env` na pasta `server`:
```env
PORT=3333
DATABASE_URL=postgresql://docker:docker@localhost:5432/agents
GEMINI_API_KEY=sua_chave_aqui
```

### 4. Instale as dependências
```bash
# Backend
cd server
npm install

# Frontend
cd ../web
npm install
```

### 5. Execute as migrations
```bash
cd server
npm run db:migrate
npm run db:seed
```

### 6. Inicie os serviços
```bash
# Backend (terminal 1)
cd server
npm run dev

# Frontend (terminal 2)
cd web
npm run dev
```

Acesse a aplicação em [http://localhost:5173](http://localhost:5173)

> **⚠️ Importante**: A aplicação requer permissão do microfone para funcionar, pois não há upload de arquivos - apenas gravação em tempo real.

## 📡 API Endpoints

### Salas
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