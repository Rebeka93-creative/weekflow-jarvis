# Supabase Setup Guide

Run these SQL statements in your Supabase **SQL Editor** (supabase.com → your project → SQL Editor → New query).

## Step 1 — Create Tables

```sql
-- Projects table
create table public.projects (
  id          text primary key,
  user_id     uuid references auth.users(id) on delete cascade not null,
  name        text not null,
  color_idx   int not null default 0,
  created_at  timestamptz default now()
);

-- Items table
create table public.items (
  id           text primary key,
  user_id      uuid references auth.users(id) on delete cascade not null,
  project_id   text references public.projects(id) on delete set null,
  title        text not null,
  type         text not null default 'task',
  start_date   date,
  end_date     date,
  time_start   text,
  time_finish  text,
  duration     text,
  priority     text not null default 'medium',
  reminder_daily boolean default false,
  prep         text,
  notes        text,
  done         boolean default false,
  created_at   timestamptz default now(),
  updated_at   timestamptz default now()
);

-- User profile table
create table public.profiles (
  id           uuid references auth.users(id) on delete cascade primary key,
  name         text,
  role         text,
  hours        text default '09:00-18:00',
  timezone     text,
  pom_work     int default 25,
  pom_break    int default 5,
  notes        text,
  updated_at   timestamptz default now()
);
```

## Step 2 — Enable Row Level Security

```sql
-- Enable RLS on all tables
alter table public.projects  enable row level security;
alter table public.items     enable row level security;
alter table public.profiles  enable row level security;

-- Projects: users can only see/edit their own
create policy "projects_own" on public.projects
  for all using (auth.uid() = user_id);

-- Items: users can only see/edit their own
create policy "items_own" on public.items
  for all using (auth.uid() = user_id);

-- Profiles: users can only see/edit their own
create policy "profiles_own" on public.profiles
  for all using (auth.uid() = id);
```

## Step 3 — Auto-create profile on signup

```sql
create or replace function public.handle_new_user()
returns trigger as $$
begin
  insert into public.profiles (id, timezone)
  values (new.id, 'UTC');
  return new;
end;
$$ language plpgsql security definer;

create trigger on_auth_user_created
  after insert on auth.users
  for each row execute procedure public.handle_new_user();
```

## Step 4 — Auto-update updated_at timestamp

```sql
create or replace function public.set_updated_at()
returns trigger as $$
begin
  new.updated_at = now();
  return new;
end;
$$ language plpgsql;

create trigger items_updated_at
  before update on public.items
  for each row execute procedure public.set_updated_at();

create trigger profiles_updated_at
  before update on public.profiles
  for each row execute procedure public.set_updated_at();
```

## Step 5 — Get your credentials

Go to: **Project Settings → API**

Copy these two values into the app when prompted:
- **Project URL** — looks like `https://xxxxxxxxxxxx.supabase.co`
- **Anon public key** — the long `eyJ...` string under "Project API keys"

That's it. Run all 4 steps in order, then open the app.
