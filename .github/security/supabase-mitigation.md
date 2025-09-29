# Supabase-based Mitigation

**Goals**
- Prevent accidental key exposure
- Enforce RLS (row-level security) by default
- Gate PRs that add `.env*` files or commit Supabase secrets


## RLS Template (example)
```sql
-- Deny-by-default
alter table public.example enable row level security;
create policy \"owner_can_read_write\" on public.example
  for all using (auth.uid() = owner_id) with check (auth.uid() = owner_id);
```

## Env/Secret Policy
- DO NOT commit `.env`, `.env.local`, or any keys to the repo.
- Store the following in GitHub Secrets: `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE`, `SUPABASE_ANON_KEX`.

## CI Gate
Configured in `.github/workflows/supabase-check.yml`.