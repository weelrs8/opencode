# Backlog inicial

Priorização proposta para revisão; não representa compromisso de entrega.

| ID | Trabalho | Requisitos | Estado |
| --- | --- | --- | --- |
| B01 | Confirmar fork e preparar base documental | Todos | Fork criado; base documental: `dev` em `0b050ac5cdd995ba8165e0699d4150b3409cfe51`; checkout e setup validados em 27/10/2026 |
| B02 | Mapear PTY, execução, permissões, plan, web e exportação no código | 01–06 | Inspeção inicial concluída; ver mapeamento |
| B03 | Documentar o que já existe e as alterações necessárias | Todos | Mapeamento inicial documentado; validação de execução pendente |
| B04 | Especificar aprovação local, negação com motivo e cancelamento | 01, 04 | Pendente |
| B05 | Implementar primeiro fluxo local de autorização + PTY + web | 01, 05 | Pendente |
| B06 | Estender execução interativa a SSH e sudo remoto | 02 | Pendente |
| B07 | Ajustar política e instruções do modo plan | 03, 04 | Pendente |
| B08 | Expor permissões efetivas e origem global/projeto | 04 | Base em camadas confirmada; exposição da origem pendente |
| B09 | Implementar inspeção dos eventos disponíveis | 05 | Pendente |
| B10 | Especificar exportação e conversão de sessões existentes | 06 | Pendente |
| B11 | Integrar provisionamento, setup e tarefas/worktrees | 07, 08 | Pendente |
| B12 | Verificar preservação de providers e autenticação por assinatura | 09 | Pendente |

## Primeiro incremento histórico — substituído pelo percurso de projeto

Uma solicitação de comando local aguarda decisão na interface web. O usuário permite,
nega ou nega com motivo. Resultado chega ao agente. Se autorizado, execução interativa
funciona e pode ser cancelada. Comando, destino e decisão ficam inspecionáveis.
Usar um comando de teste inofensivo; nenhum acesso à produção é necessário nessa etapa.

Validar negação sem execução, desconexão durante espera, cancelamento, prompt interativo
e impedimento de repetição involuntária após retomada. Fluxo real de elevação é uma etapa
adicional que depende da configuração do ambiente.

## Perguntas abertas

- Resolvido: o package.json do fork declara versão 2.1.4.
- Resolvido: fork `weelrs8/opencode`.
- Quais caminhos de documentação, ferramentas MCP e comandos devem ser permitidos em plan?
- Como mesclar permissões e como estabelecer confiança por projeto?
- Como autenticar elevação local e remota sem expor credenciais ao modelo?
- Quais formatos e critérios de curadoria serão usados nos datasets?

## Registro de novas lembranças

Adicionar cada limitação com situação observada, comportamento atual, comportamento
desejado e exemplo reproduzível, se disponível. Não transformar relato em diagnóstico
do código sem verificação.

## Ajuste após inspeção do código

Ver [mapeamento com fontes](../research/code-map.md). A base declara versão 2.1.4.
PTY, exportação JSON, hierarquia global/projeto e feedback de negação já têm implementação.
A ferramenta shell do agente continua não interativa (`stdin: ignore`).

Na inspeção inicial, o primeiro incremento recomendado passou a ser **negar com justificativa na web**,
reutilizando API existente. O fluxo integrado de autorização + PTY descrito acima
permanece como marco seguinte, após especificar a ponte com o ambiente.
Não houve implementação funcional nem execução de testes nesta inspeção.

| ID | Trabalho adicional | Estado |
| --- | --- | --- |
| B13 | Adicionar justificativa opcional de negação no dock e modelo web | Pronto para detalhamento; API já aceita message |
| B14 | Especificar controle humano e ligação da execução do agente a PTY | Pendente; avaliar PTY comum e persistente |
| B15 | Verificar diferenças entre export web, export CLI e sessões antigas | Pendente; sanitização e formato diferem |

## Prioridade atual — decisão do usuário em 27/10/2026

A plataforma será a evolução do OpenCode web. Esta decisão substitui as prioridades
iniciais de PTY e negação com justificativa, que continuam no backlog.
Ver [ADR 0002](../architecture/adr/0002-platform-in-opencode.md) e
[critérios do percurso](project-workflow.md).

| Ordem | Entrega | Estado |
| --- | --- | --- |
| P01 | Projeto, grupos de worktrees e retomada de sessões | Bases localizadas; código e benchmark pendentes |
| P02 | Issues e criação de worktree vinculada | GitHub Issues definido; classificar tipo, prioridade e tamanho por labels |
| P03 | Terminal, status de MCPs, specs e ADRs no projeto | Pendente |
| P04 | PR, checks de CI, merge e remoção da worktree | Pendente |
| P05 | Provisionamento de ambientes | Adiado; usar diretórios locais nesta fase |

Atualização de 27/10/2026: checkout, instalação e validação do CLI/servidor concluídos.
O bloqueio de rede anterior foi resolvido. O benchmark de produção exigido antes de
alterar sessões/timeline continua pendente. Ver [ambiente de desenvolvimento](development.md).

Decisão de produto: usar GitHub Issues e labels para classificar tipo, prioridade e
tamanho da implementação separadamente. GitHub Projects pode complementar a visualização,
mas não é requisito da classificação inicial; ver
[ADR 0003](../architecture/adr/0003-github-issues.md).
