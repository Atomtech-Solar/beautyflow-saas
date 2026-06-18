# Documentação Técnica — Arquitetura

Este documento descreve as decisões de arquitetura, o modelo de segurança e os principais fluxos da plataforma.

## Visão Geral

A aplicação é uma **SPA (Single Page Application)** construída em React + TypeScript, servida ao cliente e conectada a um backend gerenciado (Supabase). A arquitetura é dividida em camadas bem definidas:

- **Camada de apresentação** — componentes React, organizados por módulos de domínio.
- **Camada de serviços (Service Layer)** — concentra toda a comunicação com o backend.
- **Camada de dados** — PostgreSQL gerenciado pelo Supabase, com regras de acesso aplicadas via Row Level Security.
- **Funções serverless** — Edge Functions (Deno) para operações sensíveis que não devem ocorrer no cliente.

O princípio central é simples: **o frontend nunca é a autoridade**. Ele cuida da experiência do usuário, enquanto o banco de dados decide o que cada usuário pode ler ou escrever.

## Multi-Tenancy

Cada **empresa (tenant)** representa um ambiente isolado dentro da mesma base de dados (modelo *shared database, shared schema*). O isolamento não depende de filtros no frontend — ele é imposto diretamente no banco.

- Cada registro de domínio (agendamentos, serviços, profissionais, financeiro, estoque etc.) referencia a empresa à qual pertence.
- Uma tabela de associação relaciona usuários a empresas, definindo o papel de cada um.
- As políticas de RLS validam, em toda consulta, se o usuário autenticado tem acesso àquela empresa.

Isso permite que múltiplas empresas coexistam com segurança, cada uma enxergando apenas os próprios dados.

## Hierarquia de Usuários

A plataforma define **quatro níveis de acesso**, cada um com escopo e permissões distintas:

| Nível | Escopo | Responsabilidades |
|-------|--------|-------------------|
| **Owner** | Global | Administra todas as empresas da plataforma e tem visão consolidada. |
| **Empresa** | Tenant | Gestão completa do próprio negócio (agenda, equipe, financeiro, configurações). |
| **Funcionário** | Tenant (restrito) | Acesso à própria agenda e atendimentos, conforme permissões. |
| **Cliente** | Pessoal | Agendamento online, histórico e gestão do próprio perfil. |

O controle de rotas no frontend é feito por *guards*, mas serve apenas para orientar a navegação — a autorização efetiva é sempre revalidada no backend.

## Service Layer

Toda a comunicação com o backend é centralizada em uma camada de serviços. Os componentes **nunca** acessam o banco diretamente.

Benefícios dessa abordagem:

- **Baixo acoplamento** — a UI não conhece detalhes de persistência.
- **Reutilização** — a mesma lógica de acesso é compartilhada entre telas.
- **Testabilidade** — serviços podem ser testados de forma isolada.
- **Evolução** — alterações no backend ficam concentradas em um único ponto.

Cada serviço expõe operações de domínio (ex.: agendamentos, clientes, financeiro), retornando dados já normalizados para a interface.

## Supabase

O Supabase é utilizado como backend gerenciado, fornecendo:

- **Autenticação** — gestão de sessões e identidade dos usuários.
- **Banco de dados** — PostgreSQL com acesso via API.
- **Storage** — armazenamento de arquivos (ex.: imagens e documentos).
- **Edge Functions** — execução de lógica sensível no servidor.

O estado de servidor no frontend é gerenciado com **TanStack Query**, responsável por cache, sincronização e revalidação de dados.

## PostgreSQL

O modelo de dados é versionado por meio de **migrations SQL** (aproximadamente 73), o que garante:

- Histórico rastreável da evolução do schema.
- Reprodutibilidade do banco em diferentes ambientes.
- Aplicação de regras de negócio próximas aos dados (constraints, triggers e funções).

Validações críticas — como a prevenção de sobreposição de horários — são reforçadas no próprio banco, evitando estados inconsistentes mesmo diante de requisições concorrentes.

## Row Level Security (RLS)

O **RLS** é o pilar do isolamento multi-tenant. Cada tabela sensível possui políticas que determinam, por linha, quais registros um usuário pode acessar.

- O acesso é concedido com base na associação do usuário à empresa e no seu papel.
- Operações que precisam ignorar o RLS de forma controlada utilizam funções `SECURITY DEFINER`, com escopo restrito e bem auditado.
- Mesmo que o frontend seja comprometido ou manipulado, o banco continua negando acessos indevidos.

## Segurança Zero-Trust

O modelo adotado parte do princípio de que **o cliente é um ambiente público e não confiável**:

- Nenhuma decisão de permissão depende do frontend.
- Validações na interface existem apenas para melhorar a experiência (UX).
- Segredos e chaves privadas nunca são expostos no cliente; apenas chaves públicas são utilizadas.
- Operações sensíveis são delegadas a funções no servidor.

## Fluxo de Autenticação

1. O usuário autentica-se com suas credenciais.
2. O Supabase Auth cria e gerencia a sessão.
3. O perfil do usuário é carregado e disponibilizado de forma global na aplicação (via contexto).
4. As empresas às quais o usuário tem acesso são resolvidas — e o RLS garante que apenas as permitidas sejam retornadas.
5. A interface é renderizada de acordo com o papel e o tenant ativo.

## Fluxo de Agendamento

1. O cliente seleciona os serviços desejados.
2. O sistema calcula a **duração total** do atendimento a partir dos serviços escolhidos.
3. Os períodos já ocupados do profissional são consultados no servidor.
4. Os horários disponíveis são gerados e os que gerariam conflito são desabilitados.
5. Ao confirmar, o agendamento é validado novamente no backend, prevenindo sobreposições.

Esse fluxo combina cálculo no cliente (para uma experiência fluida) com validação final no servidor (para garantir integridade).

## Decisões Arquiteturais

- **Multi-tenant com schema compartilhado + RLS**: equilibra simplicidade operacional e isolamento seguro, sem o custo de manter um banco por cliente.
- **Service Layer obrigatória**: mantém a UI desacoplada e o backend como ponto único de verdade.
- **Validação dupla (cliente + servidor)**: oferece boa UX sem abrir mão da integridade dos dados.
- **Code splitting por rota**: painéis administrativos são carregados sob demanda, reduzindo o bundle inicial e a exposição de código.
- **Tipagem forte com TypeScript e validação com Zod**: contratos explícitos entre camadas, reduzindo erros em tempo de execução.

## Escalabilidade

A arquitetura foi pensada para crescer de forma sustentável:

- **Horizontal por tenant** — novas empresas são adicionadas sem alterações estruturais.
- **Backend gerenciado** — o Supabase abstrai escala de banco, autenticação e storage.
- **Separação de responsabilidades** — a Service Layer permite evoluir o backend (ou substituí-lo) sem reescrever a interface.
- **Lógica próxima aos dados** — regras críticas no PostgreSQL mantêm a consistência mesmo sob concorrência.
- **Carregamento sob demanda** — o code splitting mantém a performance à medida que novos módulos são incorporados.
