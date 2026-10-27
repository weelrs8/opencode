# Mapeamento do código do fork

## Escopo e método

Inspeção estática do commit `0b050ac5cdd995ba8165e0699d4150b3409cfe51`, base de `platform-docs`.
Código consultado pelo conector GitHub. Nenhuma aplicação foi iniciada e nenhum teste foi
executado; comportamentos descritos como confirmados são evidências de implementação,
não validação de execução. Não houve mudança funcional nesta etapa.

Os links abaixo apontam para esse commit, para que o diagnóstico não mude com `dev`.

## Correção sobre OpenCode 2

O [package.json](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/package.json#L5)
declara versão **2.1.4**, Bun **1.4.4** e licença MIT. A referência do usuário ao OpenCode 2
é compatível com esta base. A consulta anterior a `/releases/latest` identificou uma
release estável v1.19.4, não a versão do código da branch. Não confundir as duas.
Não foi necessário trocar a base: o fork analisado já contém código identificado como 2.x.

## Matriz de cobertura

| Requisito | Evidência encontrada | Lacuna / próximo passo |
| --- | --- | --- |
| REQ-01: PTY e aprovação local | PTY comum e persistente; autorização antes do shell; respostas once/always/reject | Ferramenta shell do agente é não interativa; projetar associação entre execução autorizada, PTY e intervenção humana |
| REQ-02: SSH e sudo remoto | Infraestrutura genérica de processos e terminais | Fluxo dedicado de aprovação remota não foi comprovado; definir destino e autenticação antes de implementar |
| REQ-03: plan flexível | Plugin aplica permissões e injeta lembretes; permite escrita no diretório de planos | Tornar escopo documental configurável e alinhar os lembretes; verificar ordem dos transforms |
| REQ-04: global/projeto | Descoberta em camadas e regras avaliadas pela última correspondência | Já existe base; mapear precedência completa, incluindo agente, sessão, permissões salvas e políticas gerenciadas |
| REQ-05: modo nerd | Barra debug existente com métricas de interface e sessão | Inspeção completa dos eventos/contexto/ferramentas não demonstrada; ampliar a base existente |
| REQ-06: exportação | CLI e web exportam JSON; backend possui sanitização e importação | Definir dataset, curadoria, exportação em lote e compatibilidade de sessões antigas |
| REQ-07: ambientes | Abstração Environment com driver/spawner; worktree aguarda comando start | Provisionamento Docker e prontidão da aplicação ainda não demonstrados; PTY precisa respeitar a mesma fronteira |
| REQ-08: continuidade | Worktrees, sessões e transferência já fornecem peças úteis | Issues, memória, specs e ADRs como experiência integrada exigem análise adicional |
| REQ-09: providers/assinatura | Plugin OpenAI possui fluxos ChatGPT browser e device code | Preservar integração; verificar experiência cloud, limites e compatibilidade em execução |

## 1. Shell, PTY e intervenção humana

Fontes:

- [ShellTool](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/tool/plugin/shell.ts#L117)
- [Shell — execução não interativa](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/shell.ts#L302)
- [PTY comum](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/pty.ts)
- [PTY persistente](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/persistent-pty/index.ts)

`ShellTool.prepare` resolve diretórios, analisa o comando e chama `permission.assert`
antes de executar. O serviço Shell passa pelo `Environment.spawner`, captura stdout/stderr
em arquivo e configura explicitamente `stdin: "ignore"`. O schema de entrada da ferramenta
expõe command, workdir, timeout e background; não expõe escolha de execução interativa.

Já o serviço Pty fornece create/write/attach/resize, buffer e eventos. Seu estado comum
é mantido em memória e os processos são encerrados no teardown do serviço. Não assumir
persistência desse serviço após reiniciar o backend.

Existe também PersistentPty, associado a sessionID, com transporte por daemon e suporte
a handoff. O serviço depende de binário próprio e pode retornar UnavailableError.
Sua integração exata com a ferramenta do agente e suas garantias de recuperação ainda
precisam ser verificadas. “Persistente” não comprova retomada em qualquer cenário.

**Conclusão:** não falta implementar um terminal do zero. Falta conectar a execução
interativa autorizada do agente a uma das infraestruturas existentes. O Pty comum
faz spawn diretamente pelo backend de PTY; não presumir que ele acompanha automaticamente
um Environment remoto ou um container selecionado.

Não há evidência suficiente nesta inspeção para afirmar um fluxo pronto de sudo remoto.
PTY oferece transporte interativo; aprovação e autenticação de privilégios continuam distintas.

## 2. Negação com justificativa já existe no núcleo e no protocolo

Fontes:

- [Permission](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/permission.ts)
- [Payload HTTP](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/protocol/src/groups/permission.ts#L142)
- [Dock web](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/app/src/session/requests/session-permission-dock.tsx)
- [Modelo de requests web](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/app/src/session/requests/model.ts#L173)

O protocolo aceita `decision` e `message` opcional. `Permission.reply` diferencia reject
sem mensagem (DeclinedError) de reject com mensagem (CorrectedError com feedback).
O comentário do núcleo explicita que a correção deve permitir continuidade do modelo,
enquanto a recusa simples interrompe o caminho de execução. Uma recusa também rejeita
outras solicitações pendentes da mesma sessão; o feedback é aplicado ao lote.

O componente web inspecionado expõe somente once/always/reject. Seu callback não recebe
justificativa e o modelo web envia a decisão sem message. Portanto existe uma lacuna
concreta na interface para esse caminho, sem necessidade aparente de mudar a API pública.

`always` salva regras por projeto quando a solicitação possui recursos salvíveis.
Não rotular essa opção como “permitir só nesta sessão”: seu escopo é diferente.
Pedidos pendentes ficam em Map/Deferred e são encerrados no close; retomada de uma
aprovação pendente após reinício não está demonstrada.

## 3. Plan: há restrição real e reinjeção de instruções

Fonte: [PlanPlugin](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/plugin/plan.ts).

O plugin adiciona deny para edit em qualquer recurso e allow em `~/.opencode/plan/*`.
O lembrete permite escrever planos somente quando solicitado e somente nesse diretório.
O hook de contexto verifica e reinsere lembretes; alterar apenas uma mensagem inicial
não resolve. Há também tratamento específico para erros de edit/write/patch.

O plugin em si não estabelece uma proibição universal de shell e MCP. O acesso final
depende das permissões do agente e das políticas aplicáveis. Devemos separar essa questão
da restrição documental que está explícita no código.

Pontos de mudança: configuração de caminhos documentais, construção do lembrete,
permissões efetivas, tratamento de erro e testes existentes em `packages/core/test/plugin/plan.test.ts`.
É necessário conferir a ordem dos transforms antes de concluir que uma configuração
do usuário consegue sobrescrever as restrições adicionadas pelo plugin.

## 4. Configuração global e por projeto já têm base implementada

Fontes:

- [ConfigDiscovery](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/config/discovery.ts)
- [Ordem de carregamento](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/config.ts#L295)
- [Aplicação das permissões](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/config/plugin/agent.ts#L82)
- [Políticas gerenciadas](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/config/plugin/policy.ts)

Config carrega entradas globais antes dos arquivos diretos e diretórios de projeto.
ConfigAgentPlugin concatena permissões dos documentos e depois aplica permissões
específicas de agentes. Permission.evaluate usa findLast: última regra correspondente vence.

Isso sustenta global/projeto, mas não significa “projeto vence tudo”. Há configuração
explícita/content, settings gerenciados, regras específicas de agente, permissões de sessão
e políticas que podem negar execução. Permission também consulta aprovações salvas,
mas preserva negações configuradas antes de aplicá-las.

Próximo passo: exemplos pequenos de conflitos e exibição da regra efetiva/origem.
Não construir um segundo sistema de permissões em paralelo ao existente.

## 5. Depuração existente é uma base parcial

Fontes:

- [Debug extension](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/gui-extensions/src/debug/index.ts)
- [Debug bar](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/gui-extensions/src/debug/bar.tsx)
- [Renderer](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/gui-extensions/src/debug/renderer.tsx)

A extensão persiste visibilidade da barra e oferece métricas de interface, heap e sessão,
incluindo cálculo de tokens por segundo. Há recursos de motion e memória condicionados
ao build; não concluir que tudo está disponível em produção ou web sem executar.

Nas fontes inspecionadas, a barra não é um inspetor completo da execução do agente.
Mapear separadamente: eventos transitórios, histórico durável, requisição ao modelo,
contexto efetivo e argumentos/resultados das ferramentas. Nem tudo é exportado ou
persistido apenas por existir na interface. Não prometer raciocínio não exposto pelo provider.

## 6. Exportação já funciona como recurso de código

Fontes:

- [Export CLI](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/cli/src/commands/handlers/session/export.ts)
- [Export web](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/app/src/session/commands/export.ts)
- [Transferência](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/session/transfer.ts)

A CLI usa session.export e aceita sanitize. O backend exporta info e mensagens em ordem
ascendente, filtradas por isSettled. A web consulta info e pagina mensagens de 200 em 200,
salvando JSON; não usa esse mesmo caminho de sanitização nessa função.

A sanitização existente substitui conteúdo de prompts, ferramentas e metadados por
marcadores. Ela serve à redução de exposição, mas não produz automaticamente dados úteis
para treinar um modelo. Precisamos de uma política específica de curadoria e remoção de
segredos que preserve exemplos escolhidos. Não equiparar exportação de mensagens a
arquivo completo de todos os eventos, contexto efetivo e solicitações ao provider.

Exportação em lote, formato versionado de dataset e compatibilidade das centenas de
sessões antigas do usuário continuam pendentes. Não utilizar sessões reais sem seleção.

## 7. Ambiente e preparação de worktree

Fontes:

- [Environment](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/environment/environment.ts)
- [Driver](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/environment/driver.ts)
- [Worktree.create](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/worktree.ts#L204)

Environment oferece files e spawner, selecionando conexão de workspace ou driver local.
Existe uma fronteira útil para aproveitar. Não foi demonstrado provisionamento Docker
com imagens prontas ou contrato completo de readiness nas fontes analisadas.

Worktree.create aguarda a estratégia de criação e o comando start configurado no projeto;
requireSuccess verifica sua conclusão antes do retorno. Isso não prova prontidão dos
serviços iniciados em background nem a ordem adotada pelo chamador que inicia o agente.
Não atribuir automaticamente ao fork o problema relatado no Orca: são fluxos distintos.

## 8. Autenticação ChatGPT existente

Fonte: [OpenAI plugin](https://github.com/weelrs8/opencode/blob/0b050ac5cdd995ba8165e0699d4150b3409cfe51/packages/core/src/plugin/provider/openai.ts).

O plugin declara métodos OAuth chatgpt-browser e chatgpt-headless, rotulados legacy,
renovação de credenciais e ajuste de transporte para o backend Codex do ChatGPT.
Também ajusta disponibilidade de modelos e cabeçalhos conforme a conexão.
É uma integração implementada que deve ser preservada, não uma nova autenticação genérica
a ser inventada. Fluxo cloud, disponibilidade de modelos e condições de uso não foram
validados em execução nesta etapa.

## Sequência recomendada após o mapeamento

1. Primeiro incremento pequeno: oferecer **negar com justificativa na web**, aproveitando
   o payload message e CorrectedError existentes. Isso entrega parte de REQ-01 e REQ-04.
2. Especificar a ponte shell/PTY, identificando execução, destino, controle de entrada,
   cancelamento e alcance das aprovações. Escolher PTY comum ou persistente com evidências.
3. Implementar e validar interação local inofensiva; depois integrar elevação local.
4. Estender para SSH com identificação do host/usuário e autenticação remota definida.
5. Ajustar plan e configuração por projeto sobre a hierarquia existente.
6. Ampliar inspeção e preparar exportação/curadoria de datasets.

A ordem acima atualiza a proposta inicial do backlog com base no código. Não significa
que o incremento de interface já foi implementado ou que sudo sem senha foi resolvido.
