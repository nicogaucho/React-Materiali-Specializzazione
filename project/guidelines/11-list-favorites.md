# Favorites List

Possiamo gestire la creazione di una lista giochi preferiti dalla piattaforma.

Come primo passo avremo bisogno di una tabella nel database per salvare la lista creata dall'utente.

Avremo bisogno di generere la tabella ```public.favorites``` come abbiamo già fatto con ```public.profiles```.

Assicuriamoci di proteggere sempre la tabella associata agli utenti abilitando Row Level Security (RLS) e facendo riferimento alla tabella auth.users per garantire l'integrità dei dati. Useremo on delete cascade per mantenere le relazioni coerenti.

Per ottenere tutto questo dovremmo passare uno script direttamente nella dashboard nella pagina SQL Editor con le seguenti direttive:

```sql
-- Create a table for public favorites
create table public.favorites (
  id uuid not null default gen_random_uuid () primary key,
  user_id uuid references auth.users on delete cascade not null,
  game_id bigint null,
  game_name text null,
  game_image text null,
  updated_at timestamp with time zone
);
-- Set up Row Level Security (RLS)
-- See https://supabase.com/docs/guides/auth/row-level-security for more details.
alter table favorites
  enable row level security;

create policy "Public favorites are viewable by everyone." on favorites
  for select using (true);

create policy "Users can insert their own favorites items." on favorites
  for insert with check (auth.uid() = user_id);

create policy "Users can delete their own favorites itemes." on favorites
  for delete using (auth.uid() = user_id);
```

## Insert in Favorites

Una volta generata la tabella in supabase, andiamo sulla pagina del dettaglio del singolo gioco e tramite un elememto UI a discrezione dello studente ( un bottone, un'icona, altro... ) andremo ad aggiungere il gioco alla nostra lista favoriti.

/games/satisfactory/58806

/games/stalker-2/58386

/games/senuas-saga-hellblade-ii/398401

