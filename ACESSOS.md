# Contas de acesso — MedSync CRM

Estas contas existem **somente no projeto Supabase** configurado em `.env`.
Não há mais login local: se o banco estiver inacessível, o login falha.

Senha padrão (enquanto o projeto original estiver no ar): **medsync**

| Perfil         | E-mail                | Senha   |
| -------------- | --------------------- | ------- |
| Administrador  | admin@medsync.com     | medsync |
| Médico         | medico@medsync.com    | medsync |
| Recepcionista  | recepcao@medsync.com  | medsync |

Novas contas podem ser criadas na tela de entrada, em "Criar conta", desde que o
backend esteja acessível.

Para apontar o app a outro projeto, envie:

- `VITE_SUPABASE_URL` (ex.: `https://xxxxx.supabase.co`)
- `VITE_SUPABASE_PUBLISHABLE_KEY` (chave publicável / anon)
- `VITE_SUPABASE_PROJECT_ID` (opcional)

Depois aplique as migrações de `supabase/migrations/` no novo projeto.
