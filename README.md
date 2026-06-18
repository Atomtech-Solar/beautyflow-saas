# Auren — Plataforma SaaS de Agendamento Inteligente

Plataforma SaaS **multi-tenant** para negócios de beleza (barbearias, salões e estúdios de manicure). Cada empresa opera em um ambiente totalmente isolado, com dashboard, agenda, equipe, financeiro e uma landing page própria — tudo em um único produto white-label.

> Projeto de portfólio. O código-fonte é privado; este repositório apresenta o produto, sua arquitetura e suas decisões técnicas.

## Principais Recursos

- **Agenda Inteligente** — visualização por profissional, cálculo automático de duração e prevenção de conflitos de horário.
- **Multi-Tenancy** — isolamento completo de dados entre empresas.
- **Financeiro** — controle de receitas, despesas e metas de faturamento.
- **Estoque** — gestão de produtos e movimentações.
- **Comissões** — apuração de pagamentos por profissional.
- **Landing Pages** — página pública gerada automaticamente para cada empresa.
- **Área do Cliente** — agendamento online e histórico de atendimentos.
- **Módulo Fiscal** — emissão e gestão de documentos fiscais.

## Destaques Técnicos

- Arquitetura **SaaS Multi-Tenant** com isolamento lógico por empresa.
- **Row Level Security (RLS)** como autoridade de acesso no banco de dados.
- Aproximadamente **73 migrations SQL** versionadas.
- **Service Layer** dedicada, desacoplando a UI do backend.
- Segurança **Zero-Trust**: o frontend não decide permissões.
- **4 níveis de acesso** (Owner, Empresa, Funcionário e Cliente).

## Stack Tecnológica

**Frontend**

- React
- TypeScript
- Tailwind CSS
- TanStack Query
- React Hook Form + Zod

**Backend**

- Supabase
- PostgreSQL
- Edge Functions

## Principais Interfaces

### Login e Autenticação

Sistema de autenticação com controle de acesso baseado em perfis e permissões. A plataforma suporta múltiplos níveis de acesso (Owner, Empresa, Funcionário e Cliente), garantindo isolamento e segurança entre os ambientes.

<p align="center">
  <img src="https://i.ibb.co/ZRFcj63d/Login-Auren.png" width="800">
</p>

### Dashboard

Painel estratégico com indicadores operacionais e financeiros em tempo real. Permite acompanhar faturamento, atendimentos realizados, desempenho da equipe e métricas de rentabilidade, incluindo análise percentual de lucro ou prejuízo para apoio à tomada de decisões.

<p align="center">
  <img src="https://i.ibb.co/Lzv2vMYP/visaogeral-auren.png" width="800">
</p>

### Agenda Inteligente

Central de gerenciamento de agendamentos com visualização por profissional, controle de disponibilidade e prevenção automática de conflitos de horário. Projetada para otimizar a ocupação da agenda e simplificar a operação diária do negócio.

<p align="center">
  <img src="https://i.ibb.co/8L3WHM6S/Agenda-auren.png" width="800">
</p>

### Landing Page

Página pública gerada automaticamente para cada empresa cadastrada na plataforma. Permite divulgação dos serviços, apresentação da equipe e conversão de visitantes em clientes através do agendamento online.

<p align="center">
  <img src="https://i.ibb.co/fzGTgSMk/landpage-auren.png" width="800">
</p>

### Área do Cliente

Portal de autoatendimento onde clientes podem realizar agendamentos online, consultar histórico de atendimentos e gerenciar suas informações, reduzindo a necessidade de contato manual com a empresa.

<p align="center">
  <img src="https://i.ibb.co/8FsJrv9/cliente-auren.png" width="400">
</p>

## Arquitetura

A plataforma adota uma arquitetura **SaaS multi-tenant** com isolamento de dados garantido no banco via Row Level Security. Uma camada de serviços desacopla a interface do backend (Supabase + PostgreSQL), enquanto o modelo de segurança Zero-Trust assegura que todas as decisões de permissão ocorram no servidor.

[Ver documentação técnica completa](docs/arquitetura.md)
