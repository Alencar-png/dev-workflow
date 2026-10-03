# Modos

## fix — corrigir defeito
1. Reproduza: comando, entrada ou teste que mostra a falha. Sem reprodução, investigue antes de mudar código.
2. Escreva um teste de regressão que falha pelo motivo certo (quando viável; se não for, explique por quê).
3. Corrija a causa, não o sintoma. Mudança mínima.
4. O teste passa; a suíte relacionada continua verde.

## feat — capacidade nova
1. Confirme o comportamento esperado (entradas, saídas, erros). Ambiguidade vira pergunta.
2. Procure no repo o que já resolve parte disso (helpers, componentes, serviços).
3. Escreva os testes do comportamento novo junto com a implementação (antes, quando o contrato está claro).
4. Implemente o mínimo que atende; nada especulativo.

## change — alterar comportamento existente
1. Localize os testes que descrevem o comportamento atual.
2. Atualize-os para o novo comportamento primeiro (eles devem falhar).
3. Altere a implementação até ficarem verdes.
4. Procure outros consumidores do comportamento antigo (grep por chamadas, rotas, tipos).

## refactor — reestruturar sem mudar comportamento
1. Confirme verde antes de começar. Se não houver testes cobrindo o trecho, escreva testes de caracterização ou avise o risco.
2. Mude em passos pequenos, rodando os testes entre eles.
3. Nenhuma mudança de comportamento, API pública ou formato de dados. Se aparecer necessidade disso, vira `change`.
4. Termina verde, com o mesmo resultado observável.
