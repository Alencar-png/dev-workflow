# dev-workflow

Um workflow padrão de desenvolvimento para agentes de código (Claude Code, Codex), empacotado como **uma skill mestra**: `dev`.

Em vez de uma pilha de regras sempre carregadas e vários comandos que competem entre si (plan, prd, tdd, review…), um **ciclo só**:

```
contexto → decidir verificação → implementar → verificar → revisar → entregar
```

O que muda de tarefa para tarefa é a **profundidade**, escolhida pelo **risco**, não pelo número de arquivos.

| Nível | Quando | O que o agente faz |
|---|---|---|
| **leve** | mudança local e óbvia | edita, roda a checagem mais próxima, entrega. Sem review, sem plano. |
| **normal** | comportamento novo/alterado, bug sem causa óbvia | plano curto, teste do comportamento, verificação do projeto, 1 revisor |
| **alta** | auth, pagamento, dados pessoais, migração, infra/CI, API pública, remoção de dados | normal + revisão especializada + confirmação de passos irreversíveis |
| **projeto** | feature grande ou ambígua | propõe um processo com plano persistente (ex.: `/aiox`) |

## Regras que não mudam

- **Verificação real**: roda os comandos que o projeto e o CI já usam. Nunca "deve funcionar".
- **Review independente**: quem implementou não aprova; o revisor roda em subagente sobre o diff.
- **Sem invenção**: requisito não pedido vira pergunta.
- **Bloqueio**: mesmo problema sem progresso após 3 tentativas → para, reavalia e relata.
- **Publicação só com pedido**: push, PR, merge, deploy e mensagens externas.
- **[ponytail](https://github.com/DietrichGebert/ponytail) sempre ativo**: a solução mais simples que funciona.

## Uso

O agente aciona a skill sozinho em pedidos de código ("corrige esse bug", "pode revisar isso?").
Para forçar um modo:

| Comando | O que faz |
|---|---|
| `/dev <pedido>` | ciclo completo, modo inferido |
| `/dev fix <bug>` | reproduz → teste de regressão → corrige |
| `/dev feat <ideia>` | comportamento novo com testes |
| `/dev change <spec>` | atualiza testes para a nova spec → implementa |
| `/dev refactor <alvo>` | reestrutura sem mudar comportamento, sempre verde |
| `/dev verify` | só roda a verificação do projeto e reporta |
| `/dev review` | só revisa o diff atual, sem editar |

Instalado como plugin, o nome fica com prefixo: `/dev-workflow:dev`.

## Instalação

### Requisito: ponytail

O workflow carrega o [ponytail](https://github.com/DietrichGebert/ponytail) antes de implementar. Instale primeiro:

```
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

### Claude Code (plugin)

Envie os dois comandos em prompts separados:

```
/plugin marketplace add Alencar-png/dev-workflow
```

```
/plugin install dev-workflow@dev-workflow
```

### Claude Code (skill pessoal, sem plugin)

Para usar como `/dev`, sem prefixo:

```bash
git clone https://github.com/Alencar-png/dev-workflow
cp -r dev-workflow/skills/dev ~/.claude/skills/dev
```

### Codex

```bash
git clone https://github.com/Alencar-png/dev-workflow
cp -r dev-workflow/skills/dev ~/.agents/skills/dev
```

## Estrutura

```
skills/dev/
├── SKILL.md            roteiro principal (~4 KB), carregado quando a skill é usada
└── references/         lidos só na fase que precisa
    ├── modes.md        fix / feat / change / refactor
    ├── verify.md       descobrir e rodar os comandos reais do projeto (CI → scripts → fallback)
    ├── review.md       qual revisor para qual mudança
    ├── git.md          branch, commit, Bitbucket vs GitHub, publicação
    └── testing.md      quando e como escrever testes
```

Mantém o `SKILL.md` curto e deixa o detalhe em `references/`, que o agente carrega só quando precisa.

## Opcionais

A skill funciona sozinha, mas aproveita estes recursos quando estão instalados:

- **Agentes revisores** (`typescript-reviewer`, `react-reviewer`, `python-reviewer`, `security-reviewer`…). Sem eles, usa um subagente genérico com o mesmo foco.
- **Skills de stack** (`react-testing`, `python-testing`, `e2e-testing`, `react-patterns`…), carregadas só para a stack tocada.
- **`/aiox`** para trabalho de nível "projeto".

## Licença

MIT
