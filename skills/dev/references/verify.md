# Verificação

A fonte da verdade é o que o projeto e o CI já rodam. Não invente ferramenta nem meta que o projeto não tem.

## Descobrir os comandos (nesta ordem)
1. **CI**: `bitbucket-pipelines.yml`, `.github/workflows/*.yml`, `.gitlab-ci.yml` → os passos que o CI roda são o alvo.
2. **Scripts**: `package.json` (`lint`, `typecheck`, `type-check`, `test`, `build`, `e2e`, `check`), `pyproject.toml`
   (ruff, mypy/pyright, pytest), `Makefile`, `justfile`, `noxfile.py`/`tox.ini`.
3. **Gerenciador**: use o do lockfile (`pnpm-lock.yaml` → pnpm, `yarn.lock` → yarn, `bun.lockb` → bun, `package-lock.json` → npm; `uv.lock` → uv, `poetry.lock` → poetry).
4. **Fallback**, só se nada acima existir: `npx tsc --noEmit`, `npx eslint <arquivos>`, `ruff check`, `pytest`.

## Ordem de execução
Do mais barato ao mais caro, parando no primeiro erro que precisa de correção:
lint/format check → typecheck → testes dos arquivos tocados → suíte relacionada → build → E2E (só se a mudança afeta fluxo coberto por E2E).

- Durante o trabalho, limite o escopo aos arquivos tocados quando a ferramenta permitir.
- Ao fechar uma tarefa de profundidade normal/alta, rode a suíte completa que o CI roda (ou diga por que não).
- Cobertura: reporte se o projeto mede; não persiga número.

## Relato
Para cada comando: o comando exato, passou/falhou e, se falhou, as linhas relevantes da saída.
Nunca "deve funcionar". Se algo não pôde rodar (falta env, serviço, credencial), diga isso explicitamente.

## Checagens rápidas extras (profundidade alta)
- `git diff` não contém segredo, `.env`, credencial, token ou chave.
- Nenhum `console.log`/`print` de debug esquecido.
- Migrações: existe caminho de rollback ou está explicado por que não.
