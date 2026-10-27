# Primeiro percurso — projeto, issue, worktree e sessões

Estado: especificação inicial; implementação ainda não iniciada.
Direção: [ADR 0002](../architecture/adr/0002-platform-in-opencode.md).

## Entrega funcional pretendida

Abrir um projeto existente no OpenCode web, selecionar uma issue e iniciar trabalho
em uma worktree. Exibir a worktree como grupo navegável com suas sessões. Reabrir
uma sessão existente deve recuperar seu histórico, sem criar uma nova sessão.

## Caminho incremental

### P01 — Contexto do projeto e grupos de worktrees

Reaproveitar a seleção de projeto existente. Mostrar o diretório principal e as
worktrees do inventário existente. Agrupar sessões pela localização real, sem
inferir a worktree pelo título ou branch escrita no texto do chat.

Preservar a relação de subagentes com suas sessões-pai. Uma sessão não equivale
a um processo de agente independente; rótulos e indicadores devem refletir os
dados disponíveis. Não duplicar sessões ao expandir grupos.

Critérios de aceite:

- Selecionar um projeto não mostra sessões de outro projeto.
- Diretório principal e worktrees ficam distinguíveis e navegáveis.
- Sessão na worktree correta continua lá depois de recarregar a página.
- Selecionar sessão existente retoma seu identificador e histórico.
- Estados vazio, carregando e erro ficam visíveis, sem misturar dados de projetos.
- Funciona em largura de celular e computador.

### P02 — Issue como origem do trabalho

Apresentar lista e detalhes da issue com descrição e critérios de aceite.
Resolver fonte de dados antes de implementar: issues internas ou GitHub.
Manter referência estável à origem; título e número isolados não bastam para
distinguir repositórios/servidores. A associação deve persistir no backend,
não apenas no armazenamento de um navegador.

O usuário solicita “Iniciar trabalho”. O fluxo resolve o projeto, cria a worktree,
aguarda preparação obrigatória e associa a issue ao resultado. Só então cria ou
abre uma sessão. Preparação delegada ao agente é um modo explícito distinto.

Critérios de aceite:

- Worktree fica vinculada à issue e ao projeto corretos.
- Durante criação/preparação, a interface mostra progresso e impede duplicação
  involuntária da mesma ação por cliques repetidos.
- Se houver falha, mostrar a etapa e preservar o que já foi criado; não apagar
  uma worktree com arquivos sem ação explícita do usuário.
- Se a worktree já existe, oferecer retomada, não iniciar outra silenciosamente.

### P03 — Completar o espaço de trabalho

Integrar terminal manual, status de MCPs e acesso a specs/ADRs do projeto,
aproveitando os recursos existentes. Verificar quais configurações são do
projeto e quais são da localização/worktree; não presumir que sejam iguais.

### P04 — PR, CI, merge e limpeza

Resolver PR pela identidade do repositório e pela branch da worktree.
Mostrar estado da PR e checks disponíveis; tratar branch sem PR e permissões
insuficientes. Merge e remoção são ações distintas, iniciadas pelo usuário.
Após merge, oferecer remoção da worktree, verificando mudanças locais antes
de qualquer exclusão. Não apagar branch remota automaticamente.

## Pontos existentes identificados

Base inspecionada: `0b050ac5cdd995ba8165e0699d4150b3409cfe51`.

- `packages/app/src/home/projects/controller.tsx`: seleção e ações do projeto.
- `packages/app/src/home/projects/view.tsx`: apresentação da lista de projetos.
- `packages/app/src/home/sessions/index.ts`: índice de sessões, retenção por diretório
  e exclusão de sessões arquivadas/subsessões da lista principal.
- `packages/app/src/workspaces/create.ts`: criação de worktree e sync da localização
  antes de montar a sessão de destino.
- `packages/app/src/workspaces/inventory.ts`: cache/listagem por servidor e projectID,
  atualização por eventos e reconexão.
- `packages/core/src/worktree.ts`: criação e preparação configurada antes do retorno.

Não alterar a retenção/paginação das sessões apenas para obter agrupamento visual.
Verificar a camada de composição e a sidebar antes de escolher o arquivo de mudança.

## Validação exigida antes de mudar sessões/sidebar

`packages/app/AGENTS.md` exige baseline de benchmark de produção antes de mudanças
de sessão/timeline e comparação após a alteração. Registrar comando, configuração
e resultado reais. Não substituir benchmark por leitura de código.

Executar testes apropriados do pacote e `bun run check` conforme instruções do
repositório. Ler a skill local de testes correspondente antes de modificar testes.
Não implementar uma tela fictícia como se fosse esse percurso concluído.

## Fora desta entrega

Docker, provisionamento remoto, instalação instantânea de stacks, alteração do
harness, extração do agente, roteamento automático de providers e treinamento de modelos.
