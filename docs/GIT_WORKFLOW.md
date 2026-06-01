# Git Workflow

## Branches

Use two long-lived branches:

- `dev`: active development branch.
- `main`: production/stable branch.

Rules:

- Build new work on `dev`.
- Keep `main` deployable.
- Do not commit experimental or broken work directly to `main`.
- Merge `dev` into `main` only after lint, typecheck, tests, and build pass.
- Development Supabase connects to `dev`.
- Production Supabase connects to `main`.

## Daily Development Commands

Switch to development:

```bash
git checkout dev
git pull origin dev
```

Commit work on development:

```bash
git add .
git commit -m "Message"
git push origin dev
```

Switch to production/stable:

```bash
git checkout main
git pull origin main
```

## Promote Dev To Main

Before promoting:

```bash
git checkout dev
git pull origin dev
```

Run verification commands after scaffold exists:

```bash
npm run lint
npm run typecheck
npm run test
npm run build
```

Merge into production/stable:

```bash
git checkout main
git pull origin main
git merge dev
git push origin main
```

Return to development:

```bash
git checkout dev
```

## Common Fixes

If `git push origiin main` is typed by mistake, rerun with the correct remote name:

```bash
git push origin main
```

If local branches are missing:

```bash
git fetch origin
git checkout dev
git checkout main
```

## Codex Rules

- Do not push to `main` unless explicitly asked.
- Prefer committing and pushing to `dev`.
- Before merging `dev` into `main`, check `git status`, run verification, and summarize what will be promoted.
- Never force-push `main` or `dev` unless the user explicitly requests it and understands the risk.

