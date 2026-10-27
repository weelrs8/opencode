# ADR 0002 — Plataforma dentro do OpenCode web

- Data: 2026-10-27 (America/Cuiaba)
- Estado: Aceito pelo usuário na conversa
- Complementa: [ADR 0001](0001-opencode-base.md)

## Decisão

Construir a plataforma no fork `weelrs8/opencode`, evoluindo sua interface web,
backend e extensões existentes. Não iniciar outra aplicação com chat e cadastro
de projetos duplicados. Não extrair o harness nesta fase.

Separar responsabilidades de plataforma, agente e ambiente dentro da arquitetura
existente não implica criar três serviços. O agente mantém providers, sessões e
ferramentas do OpenCode. A plataforma organiza o trabalho contínuo por projeto.

## Ambiente nesta fase

Usar diretórios locais e worktrees reais. Não implementar provisionamento Docker
agora e não apresentar um diretório como ambiente isolado. Comandos e arquivos
devem funcionar de verdade; apenas a infraestrutura futura fica adiada.

## Experiência pretendida

Projeto reúne sessões, issues, specs, ADRs, estado dos MCPs e worktrees.
Uma issue pode originar uma worktree. Na sidebar, a worktree agrupa as sessões
e os agentes associados, com possibilidade de retomada. O terminal continua
disponível para uso manual.

Em incrementos seguintes: associar PR à branch da worktree, acompanhar checks
do GitHub, solicitar merge e oferecer remoção da worktree depois do merge.
Essas ações não devem ocorrer automaticamente por abrir uma tela.

## Prioridade

Primeiro: projeto → issue → worktree → sessões agrupadas.
Negação com justificativa, sudo/PTY, plan flexível e datasets permanecem no backlog,
mas deixam de ser o primeiro incremento. Provisionamento de ambientes vem depois.

## Consequências

- Reaproveitar seleção de projetos, sessões e criação/inventário de worktrees.
- Preservar limites Schema → Core/Protocol → Server e dependências do Client.
- Evitar um segundo banco de sessões ou execução paralela de agentes.
- Mudanças públicas em Protocol/HttpApi exigem regeneração do client conforme AGENTS.md.
- A fonte inicial de issues (interna ou GitHub) ainda precisa ser definida; não
  assumir que integrar PR/CI obriga a hospedar todas as issues no GitHub.
- Esta decisão substitui a sugestão anterior de uma plataforma em repositório separado.
