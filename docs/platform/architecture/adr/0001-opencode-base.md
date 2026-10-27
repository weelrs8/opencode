# ADR 0001 — Aproveitar o OpenCode como base

- Data: 2026-10-24
- Estado: Proposto

## Contexto

O usuário já validou o OpenCode extensivamente. Construir um harness próprio apresentou
dificuldades em tentativas anteriores. As necessidades atuais envolvem controle profundo
de execução, permissões, planejamento, observabilidade e interface web.

## Proposta

Preparar um fork do upstream confirmado, analisar os pontos de extensão e manter mudanças
pequenas e separadas por requisito. Não iniciar reconstrução integral do harness.
Escolher a referência base explicitamente após esclarecer a menção a “OpenCode 2”.

## Consequências

- Aproveitamos comportamento e integrações existentes.
- Precisamos acompanhar mudanças upstream e preservar licença e atribuições.
- Plugins ou configurações existentes devem ser preferidos quando resolverem um requisito.
- Fork remoto criado pelo usuário: https://github.com/weelrs8/opencode.
- Clone local pendente por falha do proxy; documentação preparada sobre `dev` do fork.

## Pendências

Licença MIT conferida. Proprietário/nome: `weelrs8/opencode`. Base documental fixada
em `0b050ac5cdd995ba8165e0699d4150b3409cfe51` (`dev`). Mapear funcionalidades
existentes e custo das alterações; esclarecer a referência “OpenCode 2”.
