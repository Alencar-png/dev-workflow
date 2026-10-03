# Git e publicação

## Host
`git remote -v` decide as ferramentas:
- **Bitbucket**: sem `gh`. PR pela interface ou API do Bitbucket, só quando pedido. Pipeline em `bitbucket-pipelines.yml`.
- **GitHub**: `gh` liberado para consultar; criar PR/issue/comentário só quando pedido.

## Branch
- Profundidade normal/alta: nunca direto em `main`/`master`. Nome: `<tipo>/<slug>` (ex.: `fix/login-timeout`).
- Se o usuário já está numa branch de trabalho, use-a.

## Commit (só quando pedido)
- Conventional commits: `feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `chore:`, `perf:`, `ci:`.
- Assunto curto no imperativo; corpo explica o porquê quando não for óbvio.
- Revise `git diff --staged` antes: sem segredo, `.env`, artefato de build ou arquivo fora do escopo.
- Nunca `--no-verify`. Se o hook do repo falhar, corrija a causa.

## Publicar (só com pedido ou autorização explícita)
Push, PR, merge, tag, release, deploy, comentários em PR/issue e mensagens externas.
Nunca `--force` em branch compartilhada nem reescrever histórico publicado sem pedido explícito.
