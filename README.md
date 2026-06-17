# BeautyFlow SaaS

Plataforma SaaS multi-tenant para gestão de barbearias, salões de beleza e estúdios de estética.

O sistema permite que múltiplas empresas operem em ambientes totalmente isolados, com controle de agenda, financeiro, estoque, equipe e relacionamento com clientes.

> Código-fonte privado. Este repositório apresenta a arquitetura e as principais funcionalidades da plataforma.

## Principais Recursos

* Agenda inteligente com prevenção de conflitos
* Multi-tenancy com isolamento via Row Level Security (RLS)
* Gestão de serviços, profissionais e clientes
* Controle financeiro e metas de faturamento
* Gestão de estoque e comissões
* Landing pages automáticas para cada empresa
* Área exclusiva para clientes
* Dashboard analítico e relatórios
* Módulo fiscal baseado em Edge Functions

## Arquitetura

```text
React + TypeScript
        ↓
   Service Layer
        ↓
     Supabase
        ↓
 PostgreSQL + RLS
```

### Destaques Técnicos

* Arquitetura SaaS Multi-Tenant
* 4 níveis de acesso (Owner, Empresa, Funcionário e Cliente)
* Aproximadamente 73 migrations SQL
* Segurança baseada em políticas RLS
* Frontend desacoplado através de camada de serviços
* Abordagem Zero-Trust para controle de permissões

## Stack

**Frontend**

* React
* TypeScript
* Tailwind CSS
* TanStack Query
* React Hook Form + Zod

**Backend**

* Supabase
* PostgreSQL
* Edge Functions

## Demonstração

### Dashboard

*Screenshot*

### Agenda Inteligente

*Screenshot*

### Financeiro

*Screenshot*

### Área do Cliente

*Screenshot*
