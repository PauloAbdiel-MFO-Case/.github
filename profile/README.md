# Aplicação Multi-Family Office (MFO)

Este projeto implementa uma ferramenta de planejamento financeiro para um Multi-Family Office (MFO), desenhada para auxiliar clientes no alinhamento com seu planejamento financeiro, projetar a evolução patrimonial e registrar eventos como movimentações, seguros e metas.

## Arquitetura

A aplicação é estruturada em duas partes principais: uma API Backend e uma aplicação web Frontend.

### Backend

  - **Tecnologia:** Node.js 20 com TypeScript.
  - **Framework:** Fastify 4, com suporte para documentação de API via `@fastify/swagger`.
  - **ORM:** Prisma ORM, conectado a um banco de dados PostgreSQL 15.
  - **Validação:** Schemas Zod v4 integrados com o Fastify para garantir a segurança e a integridade dos dados de entrada.
  - **Testes:** Suíte de testes unitários, de integração e E2E implementada com Jest e Supertest.
  - **Qualidade de Código:** ESLint e Prettier para padronização e formatação do código.
  - **Arquitetura de Código:** O backend segue o padrão de arquitetura em camadas (Repositories, Services, Routes) для uma clara separação de responsabilidades e maior manutenibilidade.

### Frontend

  - **Tecnologia:** Next.js 14 (com App Router) e TypeScript.
  - **UI:** Biblioteca de componentes ShadCN/UI, com tema dark-mode como padrão.
  - **Gerenciamento de Dados:** TanStack Query (anteriormente React Query) para data fetching, cache e sincronização automática com o backend.
  - **Formulários:** React-Hook-Form integrado com Zod para validação robusta.
  - **Requisições API:** Axios para interagir com a API do Backend.

## Como Iniciar o Projeto

Para configurar e rodar a aplicação localmente, siga os passos abaixo.

### Pré-requisitos

  - Docker e Docker Compose instalados em sua máquina.
  - Node.js (v20 ou superior) e `npm` para rodar os testes localmente (opcional).

### 1\. Clonar os Repositórios

Você precisará clonar ambos os repositórios (backend e frontend) e copiar o docker-compose.yml do repositório .github e organizá-los em uma estrutura de pastas como a seguinte:

```
MFO/
docker-compose.yml
├── backend/
└── frontend/
```

### 2\. Iniciar a Aplicação com Docker Compose

O projeto inteiro (backend, frontend e banco de dados) é orquestrado pelo Docker Compose. O arquivo `docker-compose.yml` deve estar na pasta raiz `MFO/`.

```bash
# A partir da pasta raiz MFO/
docker-compose up --build
```

Este comando irá:

  - Construir as imagens Docker para o backend e o frontend.
  - Iniciar um container para o banco de dados PostgreSQL.
  - Executar as migrações do Prisma para criar o schema do banco de dados e popular com dados iniciais (`seed`).
  - Iniciar o servidor da API Backend.
  - Iniciar o servidor de desenvolvimento do Frontend.

Quando todos os serviços estiverem no ar:

  - A **API do Backend** estará acessível em `http://localhost:3333`.
  - A **Aplicação Frontend** estará acessível em `http://localhost:3000`.

### 3\. Acessar a Aplicação

Abra seu navegador e acesse `http://localhost:3000` para usar a aplicação MFO.

## Endpoints da API Backend

A API fornece os seguintes endpoints. A documentação interativa completa estará disponível via Swagger (ainda a ser configurado).

### Simulações (`/simulations`)

  - `GET /simulations`: Lista as versões mais recentes de todas as simulações.
  - `POST /simulations`: Cria uma nova simulação, duplicando uma existente.
  - `PUT /simulations/versions/:versionId`: Atualiza os dados de uma versão específica (nome, data, taxa).
  - `DELETE /simulations/versions/:id`: Deleta uma versão de uma simulação.
  - `POST /simulations/:simulationId/versions`: Cria uma nova versão para uma simulação existente.
  - `GET /simulations/versions/:versionId`: Retorna os detalhes completos (incluindo movimentações, alocações e seguros) de uma versão.
  - `GET /simulations/:id/projection`: Retorna o cálculo da projeção patrimonial.
      - Query Params: `status` (`Vivo` ou `Morto`), `calculateWithoutInsurance` (`true` ou `false`).
  - `GET /simulations/history`: Retorna o histórico completo de todas as simulações e suas versões.

### Alocações (`/allocations`)

  - `POST /versions/:versionId/allocations`: Adiciona um novo ativo a uma versão de simulação.
  - `POST /allocations/:allocationId/records`: Adiciona um novo registro de valor a um ativo existente.
  - `PUT /allocation-records/:recordId`: Atualiza um registro de valor.
  - `DELETE /allocation-records/:recordId`: Deleta um registro de valor.
  - `GET /versions/:versionId/allocation-records`: Lista todos os registros de valor de uma versão.
  - `GET /allocations`: Lista todos os ativos cadastrados.

### Movimentações (`/movements`)

  - `POST /versions/:versionId/movements`: Cria uma nova movimentação.
  - `PUT /movements/:movementId`: Atualiza uma movimentação.
  - `DELETE /movements/:movementId`: Deleta uma movimentação.
  - `GET /versions/:versionId/movements`: Lista todas as movimentações de uma versão.
  - `GET /movements`: Lista todas as movimentações do sistema.

### Seguros (`/insurances`)

  - `POST /versions/:versionId/insurances`: Adiciona um novo seguro.
  - `PUT /insurances/:insuranceId`: Atualiza um seguro.
  - `DELETE /insurances/:insuranceId`: Deleta um seguro.
  - `GET /versions/:versionId/insurances`: Lista todos os seguros de uma versão.
  - `GET /insurances`: Lista todos os seguros do sistema.

## Desenvolvimento

### Rodando os Testes do Backend

Para executar a suíte de testes do backend, utilize um dos seguintes comandos:

```bash
# Para rodar localmente (requer Node.js e dependências instaladas)
cd backend
npm test

# Para rodar dentro do ambiente Docker (recomendado)
docker-compose exec backend npm test
```

