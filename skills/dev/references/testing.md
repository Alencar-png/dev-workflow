# Testes

## Quando escrever primeiro
Teste antes da implementação quando o comportamento é bem definido: bug (regressão), `change` (spec nova),
regra de negócio com entradas/saídas claras. Em UI exploratória ou edição trivial, teste junto ou depois.

## O que testar
- Comportamento observável (saída, efeito, erro), não detalhes internos.
- Caminho feliz + os erros e bordas que importam (vazio, nulo, limite, permissão negada).
- Um comportamento por teste, com nome que descreve o caso: `rejeita login com senha expirada`.
- Estrutura Arrange–Act–Assert.

## Como
- Use o framework e os helpers que o projeto já tem (Vitest/Jest + Testing Library, pytest, Playwright).
- Mocke só fronteiras externas (rede, tempo, serviços pagos); não mocke o código sob teste.
- Sem `sleep`/timeouts arbitrários: espere por condição (`findBy*`, `waitFor`, `expect.poll`, fixtures async).
- Teste instável é bug: corrija ou isole e reporte; não reexecute até passar.
- Falha em teste existente: entenda antes de mudar. Mude o teste só se ele estiver errado ou se o comportamento mudou de propósito (`change`).

## Por stack (carregar só se precisar de detalhe)
- React/componentes/hooks → skill `react-testing`
- Python/pytest → skill `python-testing`
- Fluxos ponta a ponta → skill `e2e-testing` ou agente `e2e-runner`
