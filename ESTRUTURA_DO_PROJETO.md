# MedSync CRM — Estrutura do projeto

Mapa dos arquivos que existem neste repositório. Stack: React + JavaScript/JSX,
Vite, TanStack Start, Tailwind CSS v4, Lucide, Recharts. Backend: Lovable Cloud
(Postgres, Auth, RLS, Realtime).

---

## Raiz

| Arquivo | Descrição |
| --- | --- |
| `package.json` | Dependências e scripts `dev`, `build`, `preview`, `lint`, `format`. |
| `bun.lock` / `bunfig.toml` | Lockfile e registro do Bun (o projeto também roda com npm). |
| `tsconfig.json` | Alias `@/*` → `src/*` e opções do editor. |
| `vite.config.ts` | Vite + TanStack Start + Tailwind. |
| `components.json` | Configuração do shadcn/ui. |
| `eslint.config.js` | ESLint (flat config). |
| `.prettierrc` / `.prettierignore` | Formatação. |
| `.gitignore` | Ignora `node_modules`, `.output`, `*.tsbuildinfo`, etc. |
| `.env` | `VITE_SUPABASE_*` e `SUPABASE_*` (somente chaves publicáveis). |
| `README.md` | Visão geral, scripts, árvore e rotas. |
| `AGENTS.md` | Notas para agentes (histórico Lovable). |
| `ACESSOS.md` | E-mails das contas que existem **no Auth do Supabase**. |
| `roadmap.md` | Itens concluídos do produto. |
| `ESTRUTURA_DO_PROJETO.md` | Este arquivo. |

Pastas geradas (não versionar conteúdo de build): `node_modules/`, `.output/`, `.wrangler/`.

---

## `public/`

| Arquivo | Descrição |
| --- | --- |
| `favicon.png` | Ícone da aba. |
| `robots.txt` | Diretivas para crawlers. |

Não coloque código-fonte aqui: o Vite publica tudo o que estiver em `public/`.

---

## `src/assets/`

| Arquivo | Descrição |
| --- | --- |
| `medsync-logo.png` | Logo usada em `BrandMark`. |

---

## `src/styles.css`

Tokens de tema (paleta hospitalar, tipografia, sombras), imports do Tailwind v4
e utilitários globais (`surface-card`, etc.).

---

## `src/utils/`

| Arquivo | Descrição |
| --- | --- |
| `format.js` | Datas, CPF, telefone, iniciais, idade. |

---

## `src/data/`

| Arquivo | Descrição |
| --- | --- |
| `mock.js` | Seed fictício (pacientes, médicos, consultas, prontuários, prescrições, salas). Usado só quando o banco não responde, **em leitura**. |
| `navigation.js` | Itens da sidebar, ícones Lucide, papéis (RBAC) e rotas. |
| `clinic.jsx` | `ClinicProvider`: carrega tabelas no Supabase (timeout 10s), Realtime, operações `add*` / `updatePatient` / `deletePatient`. `dataSource` é `"cloud"` ou `"demo"`. Escritas nunca são fingidas. |

---

## `src/hooks/`

| Arquivo | Descrição |
| --- | --- |
| `useAuth.jsx` | Sessão Supabase (`getSession`, `onAuthStateChange`), `login`, `register`, `logout`, papéis em `user_roles`. Sem usuários hardcoded. |
| `use-mobile.jsx` | Detecta viewport estreita. |

---

## `src/lib/`

| Arquivo | Descrição |
| --- | --- |
| `utils.js` | `cn()` (clsx + tailwind-merge). |
| `error-capture.js` | Captura de erros. |
| `error-page.js` | Página/estado de erro. |
| `lovable-error-reporting.js` | Reporte de erros da plataforma. |

---

## `src/components/common/`

| Arquivo | Descrição |
| --- | --- |
| `BrandMark.jsx` | Logo MedSync. |
| `PageHeader.jsx` | Título + descrição + ações. |
| `StatCard.jsx` | Indicador numérico. |
| `StatusBadge.jsx` | Status colorido. |
| `EmptyState.jsx` | Lista vazia. |
| `LoadingState.jsx` | Skeletons. |

---

## `src/components/layout/`

| Arquivo | Descrição |
| --- | --- |
| `Sidebar.jsx` | Menu lateral filtrado por papel. |
| `Topbar.jsx` | Busca, notificações, perfil, logout. |
| `NotificationsMenu.jsx` | Dropdown de avisos. |

---

## `src/components/ui/`

Primitivos shadcn/ui (Radix + Tailwind). Um arquivo por componente
(`button.jsx`, `dialog.jsx`, `table.jsx`, `chart.jsx`, …). Não misturam regra
de negócio.

---

## `src/routes/`

Rotas por arquivo (TanStack Router).

| Arquivo | Rota | Descrição |
| --- | --- | --- |
| `__root.jsx` | layout raiz | Head, favicon, toaster, fontes, `<Outlet />`. |
| `index.jsx` | `/` | Login. |
| `cadastro.jsx` | `/cadastro` | Cadastro (Médico / Recepcionista). |
| `_authenticated.jsx` | layout | Exige sessão; `ClinicProvider`, sidebar, topbar; banner se `dataSource === "demo"`. |
| `_authenticated.dashboard.jsx` | `/dashboard` | Indicadores e gráficos. |
| `_authenticated.pacientes.index.jsx` | `/pacientes` | Lista, busca, cadastro. |
| `_authenticated.pacientes.$patientId.jsx` | `/pacientes/$patientId` | Detalhe; admin altera e exclui. |
| `_authenticated.agenda.jsx` | `/agenda` | Agenda semanal. |
| `_authenticated.medicos.jsx` | `/medicos` | Médicos; cadastro só admin. |
| `_authenticated.prontuarios.jsx` | `/prontuarios` | Prontuários. |
| `_authenticated.prescricoes.jsx` | `/prescricoes` | Prescrições. |
| `_authenticated.consultorios.jsx` | `/consultorios` | Salas. |
| `_authenticated.relatorios.jsx` | `/relatorios` | Relatórios. |
| `_authenticated.configuracoes.jsx` | `/configuracoes` | Preferências (parte simulada na UI). |
| `README.md` | — | Notas de rotas. |

---

## Núcleo em `src/`

| Arquivo | Descrição |
| --- | --- |
| `router.jsx` | Instância do TanStack Router. |
| `routeTree.gen.ts` | Gerado pelo plugin a partir de `src/routes/`. Não editar. |
| `start.js` | Middlewares TanStack Start (token nas server functions). |
| `server.js` | Entrada SSR. |

---

## `src/integrations/supabase/`

Cliente gerado pela integração Lovable. **A execução usa os `.js`** (o Vite
resolve `.js` antes de `.ts`).

| Arquivo | Descrição |
| --- | --- |
| `client.js` | Cliente no navegador (chave publicável + sessão). |
| `client.server.js` | Cliente no servidor. |
| `auth-middleware.js` | `requireSupabaseAuth`. |
| `auth-attacher.js` | Anexa bearer token. |
| `previewAuthStorage.js` | Storage de sessão no preview. |
| `cron-auth.js` | Auth de cron/callbacks. |
| `types.js` | Enums públicos (espelho mínimo). |
| `*.ts` | Originais TypeScript da integração; espelhar mudanças no `.js`. |

---

## `supabase/`

| Arquivo | Descrição |
| --- | --- |
| `config.toml` | `project_id`. |
| `migrations/20260908124109_*.sql` | Schema inicial: enums, tabelas (`profiles`, `user_roles`, `rooms`, `doctors`, `patients`, `appointments`, `medical_records`, `prescriptions`), RLS, trigger de usuário, seed. |
| `migrations/20260910131244_*.sql` | Permissões e Realtime. |
| `migrations/20260911120534_*.sql` e `20260911120556_*.sql` | Queixa atual, CPF único, exclusão com histórico. |
| `migrations/20260914115038_*.sql` e `20260914115058_*.sql` | Permissões administrativas de paciente. |
| `migrations/20260915140000_prevent_duplicate_appointments.sql` | Índice único médico + data + hora. |
| `migrations/20260915143000_expand_clinic_demo_data.sql` | Amplia seed de demonstração no banco. |

---

## Como executar

```powershell
npm.cmd install
npm.cmd run dev
npm.cmd run build
```

Aplicação em JavaScript/JSX. Arquivos `.ts` de config/integração são gerados.

## Contas (ver `ACESSOS.md`)

- `admin@medsync.com` / `medsync`
- `medico@medsync.com` / `medsync`
- `recepcao@medsync.com` / `medsync`

Só funcionam se o projeto em `.env` estiver no ar.
