---
name: dev
description: Workflow padrão de desenvolvimento. Use para implementar, corrigir, refatorar, testar ou revisar código, investigar falhas, ou alterar configuração/CI em projetos de software — inclusive quando o pedido vem como pergunta ("pode corrigir este bug?") ou com uma transcrição como contexto. Também via /dev, /dev fix, /dev feat, /dev change, /dev refactor, /dev verify, /dev review. Não use para apenas transcrever ou resumir reuniões, redigir documentos ou responder perguntas gerais sem trabalho no projeto.
argument-hint: "[fix|feat|change|refactor|verify|review] <pedido>"
---

# dev — workflow padrão

Um ciclo só. O que muda é a profundidade, escolhida pelo **risco**, não pelo número de arquivos.

```
contexto → decidir verificação → implementar → verificar → revisar → entregar
```

## 1. Contexto (sempre, barato)
Antes de editar: instruções do repo (CLAUDE.md/AGENTS.md do projeto), `git status` e branch, stack,
scripts reais de lint/typecheck/test/build e o remote. Convenções do repo vencem esta skill.

## 2. Profundidade — anuncie em uma linha: `Profundidade: <nível> — <motivo>`

| Nível | Quando | O que fazer |
|---|---|---|
| **leve** | mudança local e óbvia, sem mudar contrato | editar → checagem mais próxima (teste do arquivo, lint, tsc) → entregar. Sem review, sem plano escrito. |
| **normal** | comportamento novo ou alterado, vários arquivos, bug sem causa óbvia | plano curto no chat (3–8 itens) → teste do comportamento (bug: regressão que falha antes) → implementar → verificação do projeto → 1 revisor |
| **alta** | auth/permissão, pagamento, dados pessoais, migração/schema, infra/CI/deploy, API pública, remoção de dados | tudo do normal + revisão especializada pertinente (`security-reviewer` só com implicação de segurança) + confirmar passos irreversíveis ainda não autorizados |
| **projeto** | feature grande, requisito ambíguo, vários dias | oferecer `/aiox` se instalado; senão, propor ao usuário um plano escrito antes de implementar. Não gerar documentos por conta própria. |

O usuário pode impor outro nível. Na dúvida entre dois, use o maior só se o risco justificar.
Se o plano envolver decisão de produto/arquitetura ou ambiguidade, pare e confirme antes de implementar.

## 3. Modo
`fix`, `feat`, `change`, `refactor` — pelo argumento ou inferido. Se o modo não for óbvio, leia `references/modes.md`.

## 4. Implementar
- **`ponytail` sempre ativo**: carregue a skill `ponytail` (nível `full`, salvo se o usuário escolheu outro) antes de
  escrever código; ela governa a implementação em qualquer profundidade — solução mais simples que funciona,
  reuso no repo, stdlib, nada de dependência ou abstração especulativa. É requisito deste workflow
  (github.com/DietrichGebert/ponytail); se não estiver instalada, aplique esse princípio e avise o usuário.
- Testes: ao escrever ou ajustar testes, leia `references/testing.md` e, se precisar e estiver instalada, a skill da
  stack (`react-testing`, `python-testing`, `e2e-testing`). Carregue só a da stack tocada.
- Padrões de stack sob demanda, se instalados: `react-patterns`, `backend-patterns`, `api-design`, `python-patterns`,
  `fastapi-patterns`, `docker-patterns`, `database-migrations`, `security-checklist`.

## 5. Verificar
Profundidade normal/alta: leia `references/verify.md` e rode os comandos reais do projeto, espelhando o CI.
Hook nunca conta como prova. Se não deu para rodar algo, diga o quê e por quê.

## 6. Revisar
Profundidade normal/alta: leia `references/review.md`, rode o revisor em subagente sobre o diff (máx. 2),
corrija CRITICAL/HIGH e liste o resto. Quem implementou não aprova o próprio trabalho.

## 7. Entregar — contrato de conclusão
- O que mudou (arquivos e comportamento).
- Como foi verificado: comando + resultado. Falha se relata com a saída.
- O que ficou pendente ou fora do escopo.
Commit, branch, push e PR: leia `references/git.md`. Publicar (push/PR/merge/deploy/mensagens) só com pedido ou autorização.

## Regras
- **Bloqueio**: o mesmo problema sem progresso após 3 tentativas → pare, reavalie a hipótese e relate. Não é "3 erros quaisquer".
- **Sem invenção**: requisito que não foi pedido vira pergunta, não código.
- **Autorização**: o que o usuário já autorizou nesta tarefa não se pergunta de novo.
- `/dev verify` e `/dev review` **não implementam**: só rodam e reportam.
