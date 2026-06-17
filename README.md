# Agenda Inteligente — Plataforma SaaS Multi-Tenant para Negócios de Beleza

Plataforma SaaS desenvolvida para barbearias, salões de beleza e estúdios de estética. O sistema opera em modelo **multi-tenant**, permitindo que múltiplas empresas utilizem ambientes totalmente isolados dentro da mesma infraestrutura, mantendo independência operacional, segurança e escalabilidade.

> Código-fonte privado.
>
> Este repositório tem como objetivo apresentar a arquitetura, funcionalidades e decisões técnicas do projeto.

---

# Destaques do Projeto

* Arquitetura SaaS Multi-Tenant
* 4 níveis de acesso e permissão
* Isolamento de dados via Row Level Security (RLS)
* Agenda inteligente com prevenção de conflitos
* Landing page automática para cada empresa
* Controle financeiro integrado
* Gestão de estoque
* Controle de comissões
* Módulo fiscal com Edge Functions
* Área exclusiva para clientes
* Dashboard analítico com gráficos
* Aproximadamente 73 migrations SQL versionadas
* Arquitetura baseada em camada de serviços
* Segurança Zero-Trust

---

# Visão Geral

A plataforma funciona como um ecossistema white-label, onde cada empresa possui:

* Dashboard próprio
* Agenda independente
* Equipe própria
* Serviços personalizados
* Landing page exclusiva
* Clientes isolados
* Dados protegidos por políticas de segurança

---

# Hierarquia de Usuários

| Perfil              | Responsabilidades                                              |
| ------------------- | -------------------------------------------------------------- |
| Owner (Super Admin) | Administração global da plataforma e gerenciamento de empresas |
| Empresa (Tenant)    | Operação completa do próprio negócio                           |
| Funcionários        | Gestão de agenda e atendimentos                                |
| Clientes            | Agendamento online e acompanhamento de histórico               |

---

# Principais Funcionalidades

## Agenda Inteligente

* Visualização por profissional
* Controle automático de disponibilidade
* Prevenção de conflitos de horário
* Modos calendário e lista
* Busca de clientes
* Gestão de duração dos serviços

## Gestão Operacional

* Cadastro de serviços
* Cadastro de profissionais
* Controle de horários de funcionamento
* Gestão de clientes

## Financeiro

* Controle de recebimentos
* Registro de movimentações financeiras
* Formas de pagamento
* Metas de faturamento

## Estoque

* Cadastro de produtos
* Movimentações de estoque
* Controle de consumo
* Conversão de pacotes

## Comissões

* Controle de pagamentos por profissional
* Histórico de comissões
* Relatórios de desempenho

## Fiscal

* Emissão de documentos fiscais
* Cancelamento de documentos
* Logs de auditoria
* Processamento via Edge Functions

## Comunicação Interna

* Mural de avisos
* Comentários
* Menções entre colaboradores

## Área do Cliente

* Agendamento online
* Histórico de atendimentos
* Perfil do cliente
* Acompanhamento de serviços

---

# Arquitetura

## Estrutura de Permissões

```text
Owner
  │
  ├── Empresa A
  │      ├── Funcionários
  │      └── Clientes
  │
  ├── Empresa B
  │      ├── Funcionários
  │      └── Clientes
  │
  └── Empresa N
```

## Fluxo da Aplicação

```text
React + TypeScript
          │
          ▼
     Service Layer
          │
          ▼
    Supabase Auth
          │
          ▼
 PostgreSQL + RLS
          │
          ▼
 Edge Functions
```

### Princípios Arquiteturais

#### Camada de Serviços

Toda comunicação com o backend é centralizada em uma camada de serviços dedicada. Os componentes da interface nunca acessam dados diretamente.

#### Multi-Tenant por Design

O isolamento entre empresas é garantido pelo banco de dados através de políticas Row Level Security (RLS), evitando qualquer vazamento de dados entre tenants.

#### Segurança Zero-Trust

O frontend realiza apenas validações de experiência do usuário. Todas as permissões e regras de acesso são validadas no backend, que atua como autoridade final.

---

# Stack Tecnológica

## Frontend

* React 18
* TypeScript
* Vite + SWC
* React Router DOM
* TanStack Query
* React Hook Form
* Zod
* Tailwind CSS
* shadcn/ui
* Radix UI
* Recharts
* date-fns
* jsPDF
* xlsx
* Sonner
* next-themes

## Backend e Infraestrutura

* Supabase
* PostgreSQL
* Supabase Auth
* Supabase Storage
* Row Level Security (RLS)
* Supabase Edge Functions (Deno)

## Qualidade e Ferramentas

* Vitest
* Testing Library
* ESLint
* TypeScript ESLint
* pnpm

---

# Métricas do Projeto

| Métrica                 | Valor        |
| ----------------------- | ------------ |
| Níveis de acesso        | 4            |
| Arquitetura             | Multi-Tenant |
| Migrations SQL          | ~73          |
| Módulos principais      | 10+          |
| Sistema de autenticação | Completo     |
| Landing Pages           | Automáticas  |
| Dashboard Analítico     | Sim          |
| Controle Fiscal         | Sim          |
| Gestão Financeira       | Sim          |
| Controle de Estoque     | Sim          |

---

# Demonstração

## Dashboard

*Adicionar screenshot*

## Agenda Inteligente

*Adicionar screenshot*

## Gestão Financeira

*Adicionar screenshot*

## Área do Cliente

*Adicionar screenshot*

## Fluxo Completo

*Adicionar GIF ou vídeo demonstrativo*

---

# Desafios Técnicos Resolvidos

* Isolamento seguro de dados entre empresas
* Controle granular de permissões
* Prevenção de conflitos de agendamento
* Escalabilidade para múltiplos tenants
* Centralização de regras de negócio
* Segurança baseada em políticas de banco de dados
* Integração entre módulos financeiros, operacionais e fiscais

---

# Objetivo do Projeto

Este projeto foi desenvolvido para aplicar conceitos avançados de arquitetura SaaS, multi-tenancy, segurança baseada em políticas, escalabilidade de aplicações web e organização de sistemas empresariais modernos.

Representa uma solução completa para gestão de negócios de beleza, combinando experiência do usuário, segurança e escalabilidade em uma única plataforma.
