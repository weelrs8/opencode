# Backlog inicial

Priorização proposta para revisão; não representa compromisso de entrega.

| ID | Trabalho | Requisitos | Estado |
| --- | --- | --- | --- |
| B01 | Confirmar fork e preparar base documental | Todos | Fork criado; base documental: `dev` em `0b050ac5cdd995ba8165e0699d4150b3409cfe51`; clone local bloqueado pelo proxy |
| B02 | Mapear PTY, execução, permissões, plan, web e exportação no código | 01–06 | Pendente |
| B03 | Documentar o que já existe e as alterações necessárias | Todos | Pendente |
| B04 | Especificar aprovação local, negação com motivo e cancelamento | 01, 04 | Pendente |
| B05 | Implementar primeiro fluxo local de autorização + PTY + web | 01, 05 | Pendente |
| B06 | Estender execução interativa a SSH e sudo remoto | 02 | Pendente |
| B07 | Ajustar política e instruções do modo plan | 03, 04 | Pendente |
| B08 | Expor permissões efetivas e origem global/projeto | 04 | Pendente de verificar suporte atual |
| B09 | Implementar inspeção dos eventos disponíveis | 05 | Pendente |
| B10 | Especificar exportação e conversão de sessões existentes | 06 | Pendente |
| B11 | Integrar provisionamento, setup e tarefas/worktrees | 07, 08 | Pendente |
| B12 | Verificar preservação de providers e autenticação por assinatura | 09 | Pendente |

## Primeiro incremento proposto

Uma solicitação de comando local aguarda decisão na interface web. O usuário permite,
nega ou nega com motivo. Resultado chega ao agente. Se autorizado, execução interativa
funciona e pode ser cancelada. Comando, destino e decisão ficam inspecionáveis.
Usar um comando de teste inofensivo; nenhum acesso à produção é necessário nessa etapa.

Validar negação sem execução, desconexão durante espera, cancelamento, prompt interativo
e impedimento de repetição involuntária após retomada. Fluxo real de elevação é uma etapa
adicional que depende da configuração do ambiente.

## Perguntas abertas

- “OpenCode 2” significa uma release, branch, interface ou outro repositório?
- Qual conta/organização receberá o fork e qual será seu nome?
- Quais caminhos de documentação, ferramentas MCP e comandos devem ser permitidos em plan?
- Como mesclar permissões e como estabelecer confiança por projeto?
- Como autenticar elevação local e remota sem expor credenciais ao modelo?
- Quais formatos e critérios de curadoria serão usados nos datasets?

## Registro de novas lembranças

Adicionar cada limitação com situação observada, comportamento atual, comportamento
desejado e exemplo reproduzível, se disponível. Não transformar relato em diagnóstico
do código sem verificação.
