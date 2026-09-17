# MedSync CRM

Sistema web de gestão hospitalar: pacientes, agenda, médicos, prontuários,
prescrições, consultórios e relatórios.

Stack: **React + JavaScript/JSX + Vite + TanStack Start + Tailwind CSS v4 +
Lucide + Recharts**, com autenticação e dados no **Lovable Cloud / Supabase**
(Postgres, Auth, RLS e Realtime).

## Recursos

- Login e cadastro reais (Administrador, Médico, Recepcionista).
- Cadastros gravados no banco; exclusão de paciente só para Administrador.
- CPF duplicado bloqueado; queixa atual e médico responsável no cadastro.
- Agenda, prontuários, prescrições, consultórios, indicadores e relatórios.
- Sem login local: se o banco não responder, o login falha.
- Depois do login, se o banco cair, a tela avisa e mostra dados de
  `src/data/mock.js` **somente para leitura**. Escritas não são simuladas.

## Como executar (Windows)

```powershell
npm.cmd install
npm.cmd run dev
```

Abra o endereço que o terminal mostrar (por padrão `http://localhost:8080`).

| Script | Função |
| --- | --- |
| `npm.cmd run dev` | Desenvolvimento (`0.0.0.0:8080`) |
| `npm.cmd run build` | Build de produção |
| `npm.cmd run preview` | Preview do build (`4173`) |
| `npm.cmd run lint` | ESLint + Prettier |
| `npm.cmd run format` | Formata o código |

## Variáveis de ambiente (`.env`)

| Variável | Uso |
| --- | --- |
| `VITE_SUPABASE_URL` | URL do projeto (Auth + API) |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | Chave publicável / anon |
| `VITE_SUPABASE_PROJECT_ID` | ID do projeto (opcional) |
| `SUPABASE_URL` / `SUPABASE_PUBLISHABLE_KEY` / `SUPABASE_PROJECT_ID` | Mesmos valores para SSR |

Não coloque chaves secretas (`service_role`) no cliente.

Para outro backend: atualize o `.env` e aplique `supabase/migrations/`.

## Contas

Documentadas em [`ACESSOS.md`](ACESSOS.md). Existem **só no Auth do Supabase**,
não no código. A tela de login apenas pré-preenche `admin@medsync.com`.

## Estrutura do repositório

```
MedSync-CRM/
├── public/                         # estáticos (favicon, robots.txt)
├── src/
│   ├── assets/                     # logo importada pelo código
│   ├── components/
│   │   ├── common/                 # marca, cabeçalho, cards, badges
│   │   ├── layout/                 # sidebar, topbar, notificações
│   │   └── ui/                     # shadcn/ui (Radix + Tailwind)
│   ├── data/
│   │   ├── clinic.jsx              # estado clínico + Supabase
│   │   ├── mock.js                 # seed / fallback só leitura
│   │   └── navigation.js           # menu e RBAC
│   ├── hooks/
│   │   ├── useAuth.jsx             # sessão Supabase + papéis
│   │   └── use-mobile.jsx
│   ├── integrations/supabase/      # cliente Auth/DB (use os .js)
│   ├── lib/                        # cn(), captura de erros
│   ├── routes/                     # páginas (TanStack file-based)
│   ├── utils/format.js
│   ├── router.jsx
│   ├── routeTree.gen.ts            # gerado — não editar
│   ├── start.js
│   ├── server.js
│   └── styles.css                  # design system Tailwind v4
├── supabase/
│   ├── config.toml
│   └── migrations/                 # schema, RLS, índices, seed
├── .env
├── package.json
├── vite.config.ts
├── ACESSOS.md
├── ESTRUTURA_DO_PROJETO.md         # descrição arquivo a arquivo
└── README.md
```

### Rotas (`src/routes/`)

| Arquivo | URL | Função |
| --- | --- | --- |
| `index.jsx` | `/` | Login |
| `cadastro.jsx` | `/cadastro` | Cadastro de usuário |
| `_authenticated.jsx` | (layout) | Shell autenticado |
| `_authenticated.dashboard.jsx` | `/dashboard` | Indicadores |
| `_authenticated.pacientes.index.jsx` | `/pacientes` | Lista e cadastro |
| `_authenticated.pacientes.$patientId.jsx` | `/pacientes/:id` | Detalhe / exclusão |
| `_authenticated.agenda.jsx` | `/agenda` | Consultas |
| `_authenticated.medicos.jsx` | `/medicos` | Corpo clínico |
| `_authenticated.prontuarios.jsx` | `/prontuarios` | Prontuários |
| `_authenticated.prescricoes.jsx` | `/prescricoes` | Prescrições |
| `_authenticated.consultorios.jsx` | `/consultorios` | Salas |
| `_authenticated.relatorios.jsx` | `/relatorios` | Relatórios |
| `_authenticated.configuracoes.jsx` | `/configuracoes` | Preferências |

Descrição de cada arquivo: [`ESTRUTURA_DO_PROJETO.md`](ESTRUTURA_DO_PROJETO.md).
Progresso: [`roadmap.md`](roadmap.md).
