# Jornadas da Aplicação FOOTFANATIC

**Status:** rascunho para validação | **Base:** objetivos descritos nas personas Administrador, Investidor e Torcedor
**Escopo:** comportamento esperado no frontend e no backend, sem escolha de tecnologia ou desenho de telas.

## Convenções

- **Frontend:** o que a pessoa inicia, informa, consulta e recebe como feedback na aplicação.
- **Backend:** o que o sistema valida, consulta, calcula, grava ou impede para concluir a ação.
- Dados ausentes devem ser apresentados como indisponíveis, nunca como zero.
- Uma pessoa só pode executar ações pertencentes ao seu nível de acesso; esconder um comando na interface não substitui a validação no backend.

## JRN-01 — Criar conta de torcedor e escolher time favorito

**Persona:** Torcedor | **Objetivo:** criar uma conta personalizada para acompanhar um time.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Iniciar cadastro | Apresenta os campos de cadastro e a seleção obrigatória de time favorito. | Disponibiliza apenas times elegíveis para seleção; não permite que o cadastro público escolha papel privilegiado. |
| 2. Informar dados e time | Permite preencher os dados e buscar/selecionar o time. Indica campos obrigatórios e erros de validação. | Valida os dados recebidos, verifica se o time selecionado existe e pode ser escolhido e impede duplicidade de conta conforme a identidade cadastrada. |
| 3. Confirmar cadastro | Exibe confirmação ou mantém o formulário com os erros que precisam de correção. | Cria a conta com perfil de Torcedor e persiste a relação com o time favorito somente se o cadastro for válido. |
| 4. Prosseguir | Encaminha o torcedor para sua página inicial. | Retorna identidade e contexto de acesso de Torcedor; não concede acesso de Administrador ou Investidor. |

**Exceções:** se os dados forem inválidos ou o time deixar de estar disponível, o cadastro não é concluído; os dados preenchidos que ainda forem seguros de preservar não devem ser perdidos sem necessidade.

## JRN-02 — Entrar ou recuperar acesso

**Personas:** Administrador, Investidor e Torcedor | **Objetivo:** acessar a área correspondente ao perfil ou recuperar credenciais.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Entrar | Apresenta os campos de autenticação e ação de recuperação de senha. | Valida as credenciais e o estado da conta. Conta bloqueada ou credencial inválida não recebe sessão autorizada. |
| 2. Abrir área da persona | Direciona para o painel compatível com o perfil autenticado. | Retorna o perfil e as permissões vigentes; não confia em papel enviado ou alterado pelo cliente. |
| 3. Solicitar recuperação | Recebe o dado de contato e confirma que a solicitação foi processada sem revelar se a conta existe. | Processa a solicitação de recuperação sem expor a existência da conta a terceiros. |
| 4. Redefinir acesso | Permite informar e confirmar a nova credencial a partir de uma recuperação válida. | Verifica validade e uso da recuperação, atualiza a credencial e invalida o mecanismo de recuperação utilizado. |

**Exceções:** erros de acesso não mostram dados de perfil; uma conta bloqueada não pode usar a recuperação para contornar o bloqueio.

## JRN-03 — Consultar a página inicial e o time favorito

**Persona:** Torcedor | **Objetivo:** entender rapidamente o estado atual e o próximo compromisso do time.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Abrir página inicial | Solicita a experiência personalizada do torcedor autenticado. | Identifica o time favorito vinculado à própria conta e prepara os dados publicados relacionados. |
| 2. Ver resumo | Apresenta time e escudo, próximo jogo, último jogo e resultado, classificação, forma recente, notícias e principais jogadores quando disponíveis. | Consulta os jogos e resultados pertinentes, classificação, forma das partidas concluídas, notícias relacionadas e dados de jogadores. |
| 3. Aprofundar | Permite abrir jogos, classificação, estatísticas, jogadores ou notícias a partir do resumo. | Retorna o detalhe solicitado respeitando a publicação e a disponibilidade dos dados. |
| 4. Tratar falta de dados | Mostra estado vazio ou “sem dado” no bloco correspondente, sem exibir informação de outro time como se fosse a do favorito. | Diferencia ausência de informação de valor numérico zero e não cria resultados por inferência. |

**Exceções:** sem time favorito, a aplicação direciona para completar essa preferência; sem partidas ou classificação disponíveis, informa a ausência sem marcar o time como derrotado ou sem pontuação.

## JRN-04 — Acompanhar jogos, classificação e desempenho

**Persona:** Torcedor | **Objetivo:** consultar resultados e avaliar o desempenho do time ao longo de uma competição.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Abrir jogos ou classificação | Permite alternar entre próximos jogos, partidas anteriores e tabela da competição/temporada disponível. | Busca partidas e classificação no escopo escolhido; distingue estados de jogo e não trata partidas ainda não concluídas como resultado final. |
| 2. Consultar desempenho geral | Apresenta jogos, vitórias, empates, derrotas, aproveitamento, gols marcados/sofridos, saldo e média de gols. | Fornece ou calcula indicadores a partir de dados disponíveis para o mesmo time, competição, temporada e período. |
| 3. Consultar forma recente | Apresenta os últimos resultados concluídos como `V`, `E` ou `D`, em ordem cronológica. | Seleciona somente jogos concluídos e determina o resultado do ponto de vista do time favorito. |
| 4. Consultar estatísticas | Organiza métricas ofensivas, defensivas e disciplinares em grupos identificáveis. | Retorna somente métricas existentes para o recorte; preserva a distinção entre dado ausente e zero. |
| 5. Consultar elenco | Permite abrir o elenco, o perfil de um jogador e suas estatísticas disponíveis. | Retorna jogadores associados ao time e dados do jogador no escopo temporal/competitivo aplicável. |

**Exceções:** caso existam várias competições ou temporadas, o recorte selecionado deve ficar visível; uma métrica sem cobertura não deve ser apresentada como zero.

## JRN-05 — Descobrir notícias relevantes

**Persona:** Torcedor | **Objetivo:** encontrar notícias do time e também acompanhar o restante do futebol.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Abrir notícias | Exibe o feed e suas categorias disponíveis. | Consulta apenas notícias publicadas e elegíveis para leitura. |
| 2. Receber personalização | Prioriza notícias do time favorito, de seus jogadores, do mercado da bola, do campeonato e notícias gerais, nessa ordem de relevância. | Usa as associações editoriais da notícia com times, jogadores e competições; inclui notícias gerais sem associação específica. |
| 3. Abrir notícia ou entidade | Permite ler a notícia e navegar para time, jogador ou campeonato associado. | Retorna conteúdo publicado e relações válidas; não expõe rascunhos ou conteúdo removido. |
| 4. Encontrar feed vazio | Apresenta estado vazio e acesso às categorias disponíveis, sem simular recomendações. | Retorna lista vazia quando não há conteúdo elegível, sem inventar associações. |

**Exceções:** uma notícia sem associação com o time não deve ser rotulada como notícia do time; notícia em rascunho não aparece ao torcedor.

## JRN-06 — Analisar indicadores como Investidor

**Persona:** Investidor | **Objetivo:** avaliar crescimento, acessos, popularidade e desempenho sem alterar os dados do sistema.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Abrir dashboard | Apresenta resumo de usuários, acessos, retenção, popularidade e performance esportiva. | Fornece métricas agregadas compatíveis com o perfil Investidor; não retorna dados pessoais individuais. |
| 2. Selecionar período e filtros | Permite filtrar por período, campeonato, time e região quando disponível. | Valida os filtros e calcula novamente as métricas no mesmo recorte, indicando quando uma dimensão não está disponível. |
| 3. Investigar acessos | Mostra acessos por dia, semana e mês, acessos por time, visualizações de páginas de times e notícias mais acessadas. | Consulta os eventos necessários e agrega resultados sem revelar a navegação individual de um torcedor. |
| 4. Comparar popularidade | Mostra ranking com time, seguidores, acessos, visualizações e crescimento. | Calcula ou consulta cada indicador para o período/filtros selecionados; não mistura recortes incompatíveis. |
| 5. Comparar performance | Mostra jogos, vitórias, empates, derrotas, gols marcados/sofridos, saldo, aproveitamento, faltas, cartões, finalizações e escanteios em gráficos. | Retorna estatísticas agregadas da competição/período e informa indisponibilidade quando não houver cobertura. |
| 6. Tentar alterar dados | Não apresenta ações administrativas. | Rejeita qualquer comando de escrita feito por uma identidade Investidor, ainda que enviado fora da interface. |

**Exceções:** sem dados para o período, o painel apresenta estado vazio; denominadores nulos, métricas ausentes e dimensões indisponíveis não são convertidos em zero.

## JRN-07 — Administrar usuários

**Persona:** Administrador | **Objetivo:** localizar e gerenciar contas e acompanhar sua situação na plataforma.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Abrir usuários | Apresenta lista, busca/detalhe e totais de usuários ativos e novos. | Autoriza a consulta administrativa e retorna dados de conta e contagens correspondentes. |
| 2. Criar ou editar | Permite inserir ou corrigir os campos de usuário disponíveis. | Valida os dados e persiste a criação/edição quando a ação é autorizada. |
| 3. Bloquear ou excluir | Permite iniciar o bloqueio ou a exclusão e apresenta confirmação para a ação. | Valida a autorização e executa a ação solicitada conforme a política de exclusão definida para o produto. |
| 4. Consultar resultado | Atualiza a listagem e mostra confirmação ou motivo pelo qual a ação não foi concluída. | Retorna o estado atualizado ou erros de validação/autorização sem expor credenciais. |

**Exceções:** uma pessoa que não seja Administrador não pode abrir os dados administrativos nem executar essas ações diretamente.

## JRN-08 — Administrar times, jogadores, campeonatos e partidas

**Persona:** Administrador | **Objetivo:** manter o catálogo de futebol e os dados das competições utilizáveis por torcedores e investidores.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Abrir um cadastro esportivo | Apresenta lista e ações de criação, edição, consulta e exclusão para times, jogadores e campeonatos. | Autoriza operações administrativas e fornece os registros e vínculos existentes. |
| 2. Manter times e jogadores | Permite cadastrar/editar times e jogadores e associar jogadores a times. | Valida os registros e associações antes de persistir; permite consultar estatísticas de jogador disponíveis. |
| 3. Manter campeonatos | Permite criar/editar campeonatos e temporadas, associar times e cadastrar/atualizar classificação. | Valida a competição, temporada, participantes e classificação correspondentes antes de aceitar a alteração. |
| 4. Agendar partida | Permite selecionar competição/temporada, adversários e data/horário. | Valida os dados informados e registra a partida no recorte selecionado. |
| 5. Registrar partida | Permite registrar resultado e eventos como gols, cartões e faltas, além de atualizar estatísticas. | Valida os dados recebidos e retorna o estado/resultados atualizados para consulta. |
| 6. Confirmar alterações | Exibe confirmação ou erros e permite reabrir o registro atualizado. | Persiste somente operações autorizadas e válidas. |

**Exceções:** associação inválida ou dados incompletos impedem a gravação correspondente; partidas e estatísticas não devem ser exibidas como definitivas antes de terem esse estado confirmado.

## JRN-09 — Criar e publicar notícias

**Persona:** Administrador | **Objetivo:** publicar conteúdo editorial associado às entidades certas do futebol.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Criar ou editar notícia | Permite informar conteúdo e categoria e associar time, jogador ou campeonato. | Valida campos necessários e relações selecionadas antes de salvar. |
| 2. Revisar | Permite consultar o conteúdo e suas associações antes da publicação. | Mantém notícia não publicada fora das consultas dos torcedores. |
| 3. Publicar ou excluir | Permite publicar ou solicitar exclusão da notícia. | Valida permissão, aplica o estado solicitado e disponibiliza aos leitores somente o conteúdo publicado. |
| 4. Confirmar | Mostra o resultado da operação e o estado atual da notícia. | Retorna confirmação ou causa da rejeição, sem divulgar conteúdo não publicado a usuários finais. |

**Exceções:** notícia sem os dados obrigatórios não é publicada; associações inválidas não são aceitas.

## Pontos pendentes de validação
## JRN-10 — Registrar acessos para análise

**Personas:** Torcedor e Investidor | **Objetivo:** permitir que acessos a times e notícias componham os indicadores agregados da plataforma.

| Etapa | Frontend | Backend |
|---|---|---|
| 1. Consultar página relevante | O Torcedor abre uma página de time ou notícia publicada; a navegação ocorre normalmente. | Recebe a informação de acesso à página e identifica a entidade relacionada quando disponível. |
| 2. Registrar ocorrência | Não interrompe a leitura nem exige ação adicional para uma visualização comum. | Registra a ocorrência com a dimensão temporal necessária aos relatórios e, quando disponível, a referência ao time ou notícia. |
| 3. Agregar para análise | Não apresenta dados individuais de navegação ao Investidor. | Disponibiliza contagens agregadas por período, time e notícia; política de identificação, consentimento e retenção permanece pendente de decisão. |
| 4. Consultar indicadores | O Investidor seleciona filtros e consulta métricas agregadas na sua área. | Aplica os filtros às ocorrências registradas e retorna somente resultados agregados autorizados. |

**Limite:** esta jornada cobre visualizações necessárias aos indicadores solicitados. Não define ações como curtir, salvar ou seguir; os tipos de interação além de visualização dependem de decisão de produto.

## Pontos pendentes de validação

1. O torcedor poderá consultar notícias e dados públicos sem entrar na conta ou a aplicação exigirá autenticação para toda leitura?
2. O que significa excluir um usuário no produto: remoção definitiva, desativação ou anonimização? Qual histórico precisa permanecer?
3. Quais definições e janelas serão usadas para usuário ativo, crescimento, retenção, acesso, visualização e seguidores?
4. Qual granularidade de região estará disponível para análise e poderá ser apresentada ao Investidor?
5. Como serão escolhidos “principais jogadores” e qual competição/temporada aparece por padrão quando há mais de uma?
6. Quais interações do Torcedor, se houver, entram no produto além de escolher o time favorito?
7. Quais estatísticas, classificação e atualizações de partidas estarão disponíveis na fonte de dados escolhida?