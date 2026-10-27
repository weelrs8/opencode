# Plataforma de desenvolvimento contínuo

Documentação inicial de uma plataforma aberta baseada em um possível fork do OpenCode.
Nome provisório. Fork criado pelo usuário: https://github.com/weelrs8/opencode.
Nenhuma alteração funcional foi implementada nesta etapa.

Comece pelo [índice da documentação](README.md).

## Atualização — 2026-10-27 (America/Cuiaba)

Checkout, dependências e validação do CLI/servidor disponíveis; bloqueios de rede
anteriores resolvidos. Ver [ambiente de desenvolvimento](planning/development.md).
GitHub Issues foi definido como fonte do trabalho, com labels separadas para tipo,
prioridade e tamanho da implementação; ver
[ADR 0003](architecture/adr/0003-github-issues.md).
Nenhuma funcionalidade da plataforma foi implementada nesta atualização documental.

O objetivo é trabalhar continuamente em ERPs e SaaS, com múltiplos providers,
ambientes isolados e acesso web entre dispositivos. O harness existente do OpenCode
é uma base já utilizada pelo proponente; não precisamos repetir uma avaliação genérica dele.

## Histórico da preparação inicial (2026-10-24 a 2026-10-26)

- Requisitos da conversa registrados em Markdown.
- Upstream confirmado: https://github.com/anomalyco/opencode (branch padrão `dev`).
- Release estável consultada: `v1.19.4`, de 2026-10-22.
- Fork confirmado em 26/10/2026 (America/Cuiaba): https://github.com/weelrs8/opencode.
- Base documental: branch `dev`, commit `0b050ac5cdd995ba8165e0699d4150b3409cfe51`.
- Código ainda não clonado localmente por falha de conexão ao proxy.
- Acesso restabelecido: branch `platform-docs` criada para receber `docs/platform/`.
- Consulte [a preparação do fork](research/upstream-and-fork.md).
