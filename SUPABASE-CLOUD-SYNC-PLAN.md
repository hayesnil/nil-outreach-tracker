# Supabase cloud-sync proposal

This branch is intentionally separate from `main`; the live GitHub Pages site is unchanged.

## Current finding
The current `index.html` uses browser `localStorage` only. It does not initialize Supabase, authenticate a user, or read/write the `public.outreach` table.

## Safe implementation checklist
- Add Supabase JS client with the project URL and publishable key.
- Add explicit sign-up/sign-in/sign-out UI; explain email confirmation when enabled.
- Scope all reads/writes to the authenticated user's `user_id`.
- Map fields to the confirmed table columns: `id`, `user_id`, `company`, `method`, `date_contacted`, `status`, `contact`, `notes`, `created_at`, `updated_at`.
- Preserve local browser data until the user is signed in and has chosen to import it; never silently overwrite cloud records.
- Handle fetch/network errors separately from database policy errors.
- Test sign-up, confirmation, sign-in, list, create, update, delete, and sign-out before merging.
- Keep `main` unchanged until the user approves the tested pull request.

## Important
Do not disable Row Level Security. Policies should enforce `auth.uid() = user_id` for select/insert/update/delete. A publishable key is designed for browser use, but database access must remain protected by RLS.
