# Review

Rode o revisor como subagente sobre o diff (`git diff` / `git diff <base>...HEAD`), nunca sobre o repo inteiro.
No máximo 2 revisores por rodada. Profundidade leve não tem review.
Os agentes abaixo são opcionais: se um não existir na instalação, use um subagente genérico com o mesmo prompt
e o foco da linha correspondente (ex.: "revise como especialista em React").

## Qual revisor
| Mudança predominante | Revisor |
|---|---|
| TypeScript/JavaScript (não React) | `typescript-reviewer` |
| React / Next.js (`.tsx`/`.jsx`) | `react-reviewer` |
| Python | `python-reviewer` |
| FastAPI | `fastapi-reviewer` |
| SQL, schema, migração, ORM | `database-reviewer` |
| Erros engolidos, fallbacks, try/catch suspeitos | `silent-failure-hunter` |
| Outros / misto | `code-reviewer` |
| Implicação de segurança (auth, input externo, segredos, pagamento, dados pessoais) | + `security-reviewer` |
| Acessibilidade de UI | `a11y-architect` |
| Excesso de código, abstração ou dependência | `/ponytail-review` |

## Prompt do revisor
Passe: objetivo da mudança, arquivos tocados, o comando do diff e o formato de retorno:
`[SEVERIDADE] arquivo:linha — problema — sugestão`, com severidade CRITICAL/HIGH/MEDIUM/LOW.
Peça que não edite arquivos. Os `*-reviewer` e o `silent-failure-hunter` não têm Write/Edit, mas têm Bash (para `git diff` e linters); o `a11y-architect` pode editar — ao usá-lo como revisor, diga explicitamente que é só para reportar.

## Depois do review
- Corrija CRITICAL e HIGH; reverifique o que a correção tocou.
- MEDIUM/LOW: corrija se for barato e dentro do escopo; senão, liste na entrega.
- Discorde com evidência quando o revisor estiver errado; não aplique sugestão que mude o escopo pedido.
