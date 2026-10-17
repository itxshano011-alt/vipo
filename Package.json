#!/usr/bin/env bash
# =============================================================================
# VYRO — Short Video Social Platform — One-File Project Generator
# =============================================================================
# Usage:
#   chmod +x vyro-setup.sh
#   ./vyro-setup.sh
#   cd vyro
#   cp .env.example .env.local   # fill in your real Supabase credentials
#   npm install
#   npm run dev
#
# You MUST create a free Supabase project at https://supabase.com,
# run supabase/migrations/0001_init.sql in the SQL editor, create storage
# buckets as instructed in README.md, and fill .env.local. Nothing here
# fakes a backend — without real Supabase credentials the app will show
# real connection errors, not fake success.
# =============================================================================

set -e

ROOT="vyro"
echo "==> Creating VYRO project in ./$ROOT"
mkdir -p "$ROOT"
cd "$ROOT"

mkdir -p supabase/migrations
mkdir -p src/lib/supabase
mkdir -p src/components
mkdir -p src/app/\(auth\)/login
mkdir -p src/app/\(auth\)/register
mkdir -p src/app/\(auth\)/forgot-password
mkdir -p src/app/\(auth\)/reset-password
mkdir -p src/app/\(app\)/feed
mkdir -p src/app/\(app\)/explore
mkdir -p src/app/\(app\)/upload
mkdir -p src/app/\(app\)/inbox/\[conversationId\]
mkdir -p src/app/\(app\)/profile/\[username\]
mkdir -p src/app/\(app\)/settings
mkdir -p src/app/\(app\)/notifications
mkdir -p src/app/\(app\)/hashtag/\[tag\]
mkdir -p src/app/api/videos/\[id\]/like
mkdir -p src/app/api/videos/\[id\]/comments
mkdir -p src/app/api/upload/sign
mkdir -p src/app/api/follow
mkdir -p src/app/api/users/\[username\]
mkdir -p src/app/api/search
mkdir -p src/app/api/conversations/\[id\]/messages
mkdir -p src/app/api/notifications
mkdir -p src/app/api/block
mkdir -p src/app/api/report
mkdir -p src/app/api/username/check
mkdir -p src/app/api/account/delete
mkdir -p public

# -----------------------------------------------------------------------------
echo "==> package.json"
cat > package.json <<'EOF'
{
  "name": "vyro",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "typecheck": "tsc --noEmit"
  },
  "dependencies": {
    "next": "14.2.15",
    "react": "18.3.1",
    "react-dom": "18.3.1",
    "@supabase/supabase-js": "^2.45.4",
    "@supabase/ssr": "^0.5.1",
    "zod": "^3.23.8",
    "react-hook-form": "^7.53.0",
    "@hookform/resolvers": "^3.9.0",
    "next-themes": "^0.3.0",
    "lucide-react": "^0.447.0",
    "clsx": "^2.1.1",
    "date-fns": "^3.6.0"
  },
  "devDependencies": {
    "typescript": "^5.6.2",
    "@types/node": "^20.14.15",
    "@types/react": "^18.3.5",
    "@types/react-dom": "^18.3.0",
    "tailwindcss": "^3.4.10",
    "postcss": "^8.4.47",
    "autoprefixer": "^10.4.20",
    "eslint": "^8.57.0",
    "eslint-config-next": "14.2.15"
  }
}
EOF

# -----------------------------------------------------------------------------
echo "==> tsconfig.json"
cat > tsconfig.json <<'EOF'
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": false,
    "skipLibCheck": true,
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [{ "name": "next" }],
    "baseUrl": ".",
    "paths": { "@/*": ["src/*"] }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
EOF

# -----------------------------------------------------------------------------
echo "==> next.config.js"
cat > next.config.js <<'EOF'
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  images: {
    remotePatterns: [
      { protocol: "https", hostname: "**.supabase.co" }
    ]
  },
  async headers() {
    return [
      {
        source: "/:path*",
        headers: [
          { key: "X-Frame-Options", value: "DENY" },
          { key: "X-Content-Type-Options", value: "nosniff" },
          { key: "Referrer-Policy", value: "strict-origin-when-cross-origin" },
          { key: "Permissions-Policy", value: "camera=(self), microphone=(self), geolocation=()" }
        ]
      }
    ];
  }
};

module.exports = nextConfig;
EOF

# -----------------------------------------------------------------------------
echo "==> tailwind.config.ts"
cat > tailwind.config.ts <<'EOF'
import type { Config } from "tailwindcss";

const config: Config = {
  darkMode: "class",
  content: ["./src/**/*.{ts,tsx}"],
  theme: {
    extend: {
      colors: {
        bg: { DEFAULT: "#0b0b12", light: "#ffffff" },
        surface: { DEFAULT: "#15151f", light: "#f4f4f8" },
        border: { DEFAULT: "#262635", light: "#e4e4ef" },
        brand: {
          blue: "#3d8bff",
          violet: "#8b5cf6",
          pink: "#ec4899"
        }
      },
      borderRadius: {
        xl: "1rem",
        "2xl": "1.5rem"
      }
    }
  },
  plugins: []
};

export default config;
EOF

cat > postcss.config.js <<'EOF'
module.exports = {
  plugins: { tailwindcss: {}, autoprefixer: {} }
};
EOF

# -----------------------------------------------------------------------------
echo "==> .env.example"
cat > .env.example <<'EOF'
# Copy this file to .env.local and fill in real values from your Supabase project.
# Settings > API in your Supabase dashboard.

NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=

# SERVER-ONLY. Never expose this in client code or NEXT_PUBLIC_ variables.
SUPABASE_SERVICE_ROLE_KEY=

NEXT_PUBLIC_APP_NAME=VYRO
NEXT_PUBLIC_SITE_URL=http://localhost:3000

# Limits (configurable)
NEXT_PUBLIC_MAX_VIDEO_MB=100
NEXT_PUBLIC_MAX_VIDEO_SECONDS=180
EOF

cat > .gitignore <<'EOF'
node_modules
.next
.env.local
.env*.local
dist
*.log
EOF

# -----------------------------------------------------------------------------
echo "==> SQL migration (schema + RLS + triggers)"
cat > supabase/migrations/0001_init.sql <<'EOF'
-- =============================================================================
-- VYRO initial schema, constraints, indexes, triggers and Row Level Security
-- Run this entire file in Supabase SQL editor (or supabase db push).
-- =============================================================================

create extension if not exists "pgcrypto";

-- ------------------------------------------------------------------ profiles
create table if not exists public.profiles (
  id uuid primary key references auth.users(id) on delete cascade,
  username text unique not null check (char_length(username) between 3 and 24 and username ~ '^[a-z0-9_.]+$'),
  display_name text not null check (char_length(display_name) between 1 and 50),
  bio text default '' check (char_length(bio) <= 300),
  avatar_path text,
  website_url text check (website_url is null or char_length(website_url) <= 200),
  is_private boolean not null default false,
  is_verified boolean not null default false,
  role text not null default 'user' check (role in ('user','admin')),
  followers_count int not null default 0,
  following_count int not null default 0,
  videos_count int not null default 0,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);
create index if not exists idx_profiles_username on public.profiles (username);

-- -------------------------------------------------------------------- videos
create table if not exists public.videos (
  id uuid primary key default gen_random_uuid(),
  creator_id uuid not null references public.profiles(id) on delete cascade,
  caption text default '' check (char_length(caption) <= 2200),
  storage_path text not null,
  thumbnail_path text,
  duration numeric,
  visibility text not null default 'public' check (visibility in ('public','followers','private')),
  processing_status text not null default 'pending' check (processing_status in ('pending','processing','published','failed')),
  moderation_status text not null default 'approved' check (moderation_status in ('approved','pending_review','removed')),
  likes_count int not null default 0,
  comments_count int not null default 0,
  views_count int not null default 0,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);
create index if not exists idx_videos_feed on public.videos (created_at desc) where processing_status = 'published' and moderation_status = 'approved' and visibility = 'public';
create index if not exists idx_videos_creator on public.videos (creator_id, created_at desc);

-- ------------------------------------------------------------------- follows
create table if not exists public.follows (
  follower_id uuid not null references public.profiles(id) on delete cascade,
  followed_id uuid not null references public.profiles(id) on delete cascade,
  status text not null default 'accepted' check (status in ('pending','accepted')),
  created_at timestamptz not null default now(),
  primary key (follower_id, followed_id),
  constraint no_self_follow check (follower_id <> followed_id)
);
create index if not exists idx_follows_followed on public.follows (followed_id);
create index if not exists idx_follows_follower on public.follows (follower_id);

-- --------------------------------------------------------------- video_likes
create table if not exists public.video_likes (
  user_id uuid not null references public.profiles(id) on delete cascade,
  video_id uuid not null references public.videos(id) on delete cascade,
  created_at timestamptz not null default now(),
  primary key (user_id, video_id)
);

-- ------------------------------------------------------------------ comments
create table if not exists public.comments (
  id uuid primary key default gen_random_uuid(),
  video_id uuid not null references public.videos(id) on delete cascade,
  author_id uuid not null references public.profiles(id) on delete cascade,
  body text not null check (char_length(body) between 1 and 500),
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now(),
  deleted_at timestamptz
);
create index if not exists idx_comments_video on public.comments (video_id, created_at desc);

-- ----------------------------------------------------------------- hashtags
create table if not exists public.hashtags (
  id uuid primary key default gen_random_uuid(),
  tag text unique not null check (char_length(tag) between 1 and 50 and tag ~ '^[a-z0-9_]+$')
);

create table if not exists public.video_hashtags (
  video_id uuid not null references public.videos(id) on delete cascade,
  hashtag_id uuid not null references public.hashtags(id) on delete cascade,
  primary key (video_id, hashtag_id)
);
create index if not exists idx_video_hashtags_hashtag on public.video_hashtags (hashtag_id);

-- ------------------------------------------------------------- conversations
create table if not exists public.conversations (
  id uuid primary key default gen_random_uuid(),
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

create table if not exists public.conversation_participants (
  conversation_id uuid not null references public.conversations(id) on delete cascade,
  user_id uuid not null references public.profiles(id) on delete cascade,
  joined_at timestamptz not null default now(),
  last_read_at timestamptz,
  primary key (conversation_id, user_id)
);
create index if not exists idx_participants_user on public.conversation_participants (user_id);

-- ------------------------------------------------------------------ messages
create table if not exists public.messages (
  id uuid primary key default gen_random_uuid(),
  conversation_id uuid not null references public.conversations(id) on delete cascade,
  sender_id uuid not null references public.profiles(id) on delete cascade,
  message_type text not null default 'text' check (message_type in ('text','image','video_link')),
  body text check (char_length(body) <= 2000),
  attachment_path text,
  created_at timestamptz not null default now(),
  edited_at timestamptz,
  deleted_at timestamptz
);
create index if not exists idx_messages_conversation on public.messages (conversation_id, created_at desc);

create table if not exists public.message_reads (
  message_id uuid not null references public.messages(id) on delete cascade,
  user_id uuid not null references public.profiles(id) on delete cascade,
  read_at timestamptz not null default now(),
  primary key (message_id, user_id)
);

-- ------------------------------------------------------------- notifications
create table if not exists public.notifications (
  id uuid primary key default gen_random_uuid(),
  recipient_id uuid not null references public.profiles(id) on delete cascade,
  actor_id uuid references public.profiles(id) on delete cascade,
  type text not null check (type in ('follow','like','comment','message','mention')),
  related_resource_id uuid,
  read_at timestamptz,
  created_at timestamptz not null default now()
);
create index if not exists idx_notifications_recipient on public.notifications (recipient_id, created_at desc);

-- -------------------------------------------------------------------- blocks
create table if not exists public.blocks (
  blocker_id uuid not null references public.profiles(id) on delete cascade,
  blocked_id uuid not null references public.profiles(id) on delete cascade,
  created_at timestamptz not null default now(),
  primary key (blocker_id, blocked_id),
  constraint no_self_block check (blocker_id <> blocked_id)
);

-- ------------------------------------------------------------------- reports
create table if not exists public.reports (
  id uuid primary key default gen_random_uuid(),
  reporter_id uuid not null references public.profiles(id) on delete cascade,
  target_type text not null check (target_type in ('user','video','comment')),
  target_id uuid not null,
  reason text not null check (char_length(reason) between 1 and 500),
  status text not null default 'open' check (status in ('open','reviewed','actioned','dismissed')),
  created_at timestamptz not null default now(),
  reviewed_at timestamptz
);
create index if not exists idx_reports_status on public.reports (status, created_at desc);

-- =============================================================================
-- HELPER FUNCTIONS
-- =============================================================================

create or replace function public.is_admin(uid uuid)
returns boolean language sql stable as $$
  select exists(select 1 from public.profiles where id = uid and role = 'admin');
$$;

create or replace function public.is_blocked(a uuid, b uuid)
returns boolean language sql stable as $$
  select exists(
    select 1 from public.blocks
    where (blocker_id = a and blocked_id = b) or (blocker_id = b and blocked_id = a)
  );
$$;

create or replace function public.is_following(a uuid, b uuid)
returns boolean language sql stable as $$
  select exists(
    select 1 from public.follows where follower_id = a and followed_id = b and status = 'accepted'
  );
$$;

create or replace function public.is_video_visible(v public.videos)
returns boolean language sql stable as $$
  select
    case
      when v.processing_status <> 'published' or v.moderation_status <> 'approved' then
        v.creator_id = auth.uid()
      when v.visibility = 'public' then true
      when v.visibility = 'followers' then (auth.uid() = v.creator_id or public.is_following(auth.uid(), v.creator_id))
      when v.visibility = 'private' then auth.uid() = v.creator_id
      else false
    end
    and not public.is_blocked(auth.uid(), v.creator_id);
$$;

create or replace function public.set_updated_at()
returns trigger language plpgsql as $$
begin
  new.updated_at = now();
  return new;
end;
$$;

create trigger trg_profiles_updated before update on public.profiles
  for each row execute function public.set_updated_at();
create trigger trg_videos_updated before update on public.videos
  for each row execute function public.set_updated_at();
create trigger trg_comments_updated before update on public.comments
  for each row execute function public.set_updated_at();

-- Auto-create a profile when a new auth user signs up
create or replace function public.handle_new_user()
returns trigger language plpgsql security definer set search_path = public as $$
declare
  base_username text;
  final_username text;
  suffix int := 0;
begin
  base_username := lower(regexp_replace(coalesce(new.raw_user_meta_data->>'username', split_part(new.email,'@',1)), '[^a-z0-9_.]', '', 'g'));
  if base_username is null or length(base_username) < 3 then
    base_username := 'user' || substr(new.id::text, 1, 8);
  end if;
  final_username := base_username;
  while exists(select 1 from public.profiles where username = final_username) loop
    suffix := suffix + 1;
    final_username := base_username || suffix::text;
  end loop;

  insert into public.profiles (id, username, display_name)
  values (new.id, final_username, coalesce(new.raw_user_meta_data->>'display_name', base_username));
  return new;
end;
$$;

drop trigger if exists on_auth_user_created on auth.users;
create trigger on_auth_user_created
  after insert on auth.users
  for each row execute function public.handle_new_user();

-- Counters: followers/following
create or replace function public.handle_follow_counts()
returns trigger language plpgsql security definer set search_path = public as $$
begin
  if tg_op = 'INSERT' then
    update public.profiles set following_count = following_count + 1 where id = new.follower_id;
    update public.profiles set followers_count = followers_count + 1 where id = new.followed_id;
    insert into public.notifications (recipient_id, actor_id, type, related_resource_id)
    values (new.followed_id, new.follower_id, 'follow', new.follower_id);
  elsif tg_op = 'DELETE' then
    update public.profiles set following_count = greatest(following_count - 1, 0) where id = old.follower_id;
    update public.profiles set followers_count = greatest(followers_count - 1, 0) where id = old.followed_id;
  end if;
  return null;
end;
$$;

drop trigger if exists trg_follow_counts on public.follows;
create trigger trg_follow_counts
  after insert or delete on public.follows
  for each row execute function public.handle_follow_counts();

-- Counters: videos_count
create or replace function public.handle_video_counts()
returns trigger language plpgsql security definer set search_path = public as $$
begin
  if tg_op = 'INSERT' then
    update public.profiles set videos_count = videos_count + 1 where id = new.creator_id;
  elsif tg_op = 'DELETE' then
    update public.profiles set videos_count = greatest(videos_count - 1, 0) where id = old.creator_id;
  end if;
  return null;
end;
$$;

drop trigger if exists trg_video_counts on public.videos;
create trigger trg_video_counts
  after insert or delete on public.videos
  for each row execute function public.handle_video_counts();

-- Counters + notification: likes
create or replace function public.handle_like_counts()
returns trigger language plpgsql security definer set search_path = public as $$
declare
  v_creator uuid;
begin
  if tg_op = 'INSERT' then
    update public.videos set likes_count = likes_count + 1 where id = new.video_id returning creator_id into v_creator;
    if v_creator is not null and v_creator <> new.user_id then
      insert into public.notifications (recipient_id, actor_id, type, related_resource_id)
      values (v_creator, new.user_id, 'like', new.video_id);
    end if;
  elsif tg_op = 'DELETE' then
    update public.videos set likes_count = greatest(likes_count - 1, 0) where id = old.video_id;
  end if;
  return null;
end;
$$;

drop trigger 
