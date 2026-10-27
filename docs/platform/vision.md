# Visão e contexto

## Forma de trabalho

O usuário desenvolve sozinho produtos de evolução contínua, como ERPs e SaaS.
Uma entrega não encerra o projeto: bugs, melhorias e novas versões são recorrentes.
O tempo disponível varia, e o trabalho precisa continuar entre celular, MacBook e PC.
A stack habitual é Laravel, Vue, Inertia e PostgreSQL, com possibilidade de outras stacks.

## Motivações

- Liberdade de provider e aproveitamento das modalidades de acesso já contratadas.
- Serviços mencionados: Z.ai, OpenCode Go e ChatGPT Plus; custo combinado relatado de aproximadamente US$ 47/mês.
- OpenCode já é amplamente testado pelo usuário. Tentativas de integração encontraram limitações profundas.
- Orca IDE atende parte da organização, mas faltam chat gráfico, detalhes nas issues e coordenação confiável do setup.
- Caso relatado: o agente começa antes de terminar a preparação da worktree/ambiente.
- Projeto aberto; hospedagem, nome e política de contribuições ainda serão definidos.

## Continuidade

Projeto guarda conhecimento duradouro; tarefa guarda contexto de uma mudança;
sessão registra uma execução do agente. Uma tarefa pode atravessar várias sessões.
Trocar de dispositivo, modelo ou sessão não deve apagar o contexto do trabalho.

Memória, specs, ADRs e issues precisam ser separadas por projeto. A organização física
e a política de promoção de descobertas a conhecimento duradouro ainda serão definidas.

## Direção atual

Documentar necessidades e preparar um possível fork do OpenCode, preservando seu
harness e compatibilidade com múltiplos providers. Reavaliar a abordagem se a análise
concreta revelar custo de manutenção incompatível com o desenvolvimento individual.
