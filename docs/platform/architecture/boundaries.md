# Fronteiras da arquitetura

Estado: direção aceita para plataforma dentro do OpenCode web; ver [ADR 0002](adr/0002-platform-in-opencode.md).

A plataforma será desenvolvida no fork existente, sem aplicação separada ou extração
do harness. Ambiente inicial é um diretório local com worktrees reais; Docker fica
para uma fase posterior. As seções abaixo descrevem responsabilidades, não três serviços.

## Ambiente

Responsável por arquivos, worktree, terminal/PTY, processos, dependências, serviços,
portas e recursos de execução. Pode continuar existindo sem uma sessão ativa do agente.
Execução remota via SSH precisa de identidade de destino e política própria.

## Harness

Responsável por comunicação com modelos, contexto, ferramentas, permissões,
cancelamento e histórico de execução. Direção inicial: aproveitar o OpenCode.
Não duplicar seu loop antes de identificar limitações concretas no código.

## Plataforma

Responsável por projetos, tarefas/issues, documentação, associação entre sessões e ambientes,
interface web responsiva e continuidade entre dispositivos.
Não há decisão de usar Laravel, TypeScript, LangGraph, AI SDK ou Pi como nova base.

## Fluxos a detalhar

1. Criar worktree e provisionar ambiente.
2. Executar setup obrigatório ou delegar preparação ao agente, conforme configuração explícita.
3. Iniciar/retomar sessão vinculada à tarefa.
4. Solicitar autorização, aguardar decisão, executar e registrar resultado.
5. Revisar diff, executar verificações e abrir PR.

Autorização deve ficar vinculada à execução apresentada. Repetição, mudança de destino
e expiração precisam de semântica definida. Detecção de sudo em texto de shell, sozinha,
não constitui um mecanismo completo de controle de privilégios.

Eventos persistidos podem atender inspeção e exportação. Esquema, retenção,
redação de dados e garantias de recuperação ainda serão especificados.
