# Escolha do Estilo Arquitetural
Opções Avaliadas
Conforme orientação do trabalho, quatro estilos foram avaliados como candidatos:
Em Camadas 	(Layered Architecture);
Microserviços;
Publish-Subscribe 	(orientada a eventos);
Híbrida 	(combinação de estilos).


**Estilo Escolhido: Arquitetura Híbrida**
Proposta: Arquitetura em Camadas para o núcleo acadêmico (gestão de turmas, atividades, entregas e notas), combinada com Publish-Subscribe para o subsistema de eventos, que engloba notificações e o módulo de recomendação/planejamento personalizado.

**Por que não um estilo "puro":**
Microserviços - rejeitado. O escopo do MVP (RN01 a RN06: CRUD de turmas, atividades, entregas e notas) não possui volume de tráfego, times distintos ou necessidade de deploy independente que justifiquem o custo operacional de service discovery, orquestração e observabilidade distribuída. Para um MVP, microserviços gerariam overhead de infraestrutura desproporcional ao ganho.
Camadas - rejeitado. A RN06 (notificações sobre prazos e materiais) e as RN09–RN10 (cálculo de prioridade e geração de agenda) são processos assíncronos e orientados a eventos, que não se encaixam bem em uma cadeia síncrona request-response típica de arquitetura em camadas estrita. Forçar esse comportamento em camadas puras criaria acoplamento temporal desnecessário, por exemplo, a criação de uma atividade ficaria "esperando" o disparo da notificação.

### Por que a Arquitetura Híbrida se encaixa

| Parte do Sistema | Estilo Aplicado | Justificativa |
|------------------|-----------------|---------------|
| Gestão de turmas, atividades, entregas e notas (**RN01–RN08**) | Camadas (Apresentação, aplicação, domínio, persistência) | Fluxos CRUD clássicos e transacionais, com regras de autorização bem definidas (professor × estudante), organizadas de forma isolada. |
| Notificações (**RN06**) | Publish-Subscribe | Múltiplos eventos disparam notificações (nova atividade, novo material, prazo próximo). O modelo Pub-Sub desacopla quem gera o evento de quem consome (notificação por push/e-mail), permitindo novos "assinantes" no futuro sem alterar o núcleo. |
| Recomendação e planejamento (**RN07–RN10**) | Módulo desacoplado, consumidor de eventos | É uma funcionalidade inovadora do produto. Isolar o módulo permite evoluí-lo (trocar algoritmo, adicionar aprendizado futuramente) sem mexer no núcleo acadêmico, reagindo a eventos e usando parâmetros fornecidos pelas notificações. |


Essa combinação é frequentemente chamada de "monolito modular com espinha dorsal de eventos", um estágio intermediário maduro entre o monolito puro e os microserviços completos. É uma escolha tecnicamente defensável para um MVP que pode evoluir: cada módulo já nasce com fronteira clara, o que facilita uma futura extração para microserviços independentes.

# Decisões arquiteturais  (ADRs) 	
As decisões arquiteturais relevantes foram documentadas no formato ADR (Architectural Decision Record), padrão amplamente utilizado na prática profissional de Engenharia de Software para registrar contexto, decisão, alternativas consideradas e consequências.

**ADR 01 - Estilo Arquitetural: Híbrido (Camadas - Publish-Subscribe)**

| Item | Descrição |
|------|-----------|
| **Contexto** | O sistema possui dois perfis de funcionalidade distintos: operações transacionais síncronas (CRUD de turmas, atividades, entregas e notas) e processos reativos/assíncronos (notificações, priorização e geração de agenda). |
| **Decisão** | Adotar arquitetura em camadas para o núcleo acadêmico e **Publish-Subscribe** para o subsistema de eventos (notificações e módulo de recomendação). |
| **Alternativas Consideradas** | **Microserviços** — rejeitado por gerar overhead de infraestrutura desnecessário para o escopo do MVP.<br><br>**Camadas puro** — rejeitado por não suportar bem RN06/RN09/RN10 sem acoplamento temporal forte. |
| **Consequências** | Ganha-se desacoplamento entre "o que aconteceu" (evento) e "o que fazer a respeito" (reação), com custo de complexidade extra, pois é necessário um broker de eventos, mesmo que simples. Facilita a futura extração de um Serviço de Recomendação como microserviço independente. |


**ADR 02 - Mecanismo de Autenticação e Autorização: JWT com RBRAC**

| Item | Descrição |
|------|-----------|
| **Contexto** | **RN01**, **RN02** e **RN05** exigem distinção rígida entre professor e estudante, incluindo, no caso do professor, restrição de correção e atribuição de notas apenas às turmas sob sua responsabilidade. |
| **Decisão** | Autenticação via **JWT (JSON Web Token)**, emitido no login, contendo o papel do usuário (role: professor, estudante, coordenador ou administrador) e *claims* de vínculo (turmas lecionadas ou em que está matriculado). Autorização aplicada via **RBAC** combinado com checagem de posse de recurso, por exemplo: "é professor E é o professor responsável por esta turma". |
| **Alternativas Consideradas** | **Sessão com cookie server-side** — rejeitada por dificultar a escalabilidade horizontal e exigir estado no servidor.<br><br>**OAuth de terceiros** como mecanismo único — mantido apenas como complementar, pois a instituição precisa de controle próprio sobre os papéis acadêmicos. |
| **Consequências** | Mecanismo *stateless* e escalável horizontalmente, mas que exige cuidado com expiração e revogação de token, por exemplo, quando um professor é removido de uma turma durante uma sessão ativa. |



**ADR 03 - Estratégia de Notificações: Event-Driven via Message Broker (Pub-Sub)**

| Item | Descrição |
|------|-----------|
| **Contexto** | A **RN06** exige notificações disparadas por múltiplos eventos de origens diferentes: criação de atividade, publicação de novo material e proximidade de prazo. Este último não é uma ação direta do usuário, mas sim um evento temporal/agendado. |
| **Decisão** | Implementar um barramento de eventos (por exemplo, **RabbitMQ** ou, em uma versão mais simples de MVP, **Redis Pub-Sub**), no qual os módulos publicam eventos de domínio (**AtividadeCriada**, **MaterialPublicado**, **PrazoProximo**). Um Serviço de Notificação dedicado consome esses eventos e despacha mensagens para os canais configurados (push e e-mail). |
| **Alternativas Consideradas** | **Chamada direta/síncrona** do módulo de Atividades para o módulo de Notificação — rejeitada, pois acopla os dois módulos e prejudica o requisito não funcional de disponibilidade (uma falha na notificação não pode derrubar a criação da atividade).<br><br>**Polling pelo cliente** — rejeitado por ser ineficiente e não atender bem a prazos "próximos", que exigem um job agendado. |
| **Consequências** | Maior resiliência, já que uma falha na notificação não afeta a operação principal. Exige um job/scheduler adicional para verificar prazos próximos, publicando o evento **PrazoProximo** periodicamente (por exemplo, diariamente). |


**ADR 04 - Integração Núcleo Acadêmico - Módulo de Recomendação: Eventos - API de Leitura**

| Item | Descrição |
|------|-----------|
| **Contexto** | As **RN09** e **RN10** precisam de dados de múltiplas origens (atividades, prazos, perfil do estudante e preferências) para calcular a prioridade e montar a agenda semanal. Esse módulo é também o mais sujeito a mudanças futuras, por ser a funcionalidade inovadora do produto. |
| **Decisão** | O **Módulo de Recomendação** escuta eventos do núcleo acadêmico (como **AtividadeCriada** e **PerfilAcademicoAtualizado**) para manter sua própria visão dos dados necessários, e expõe uma API própria de leitura (**GET /prioridades**, **GET /agenda-semanal**), consumida diretamente pelo frontend. O módulo não acessa diretamente o banco de dados do núcleo acadêmico. |
| **Alternativas Consideradas** | **Acesso direto ao banco de dados compartilhado** — rejeitado, pois cria acoplamento forte entre módulos e dificulta a evolução ou extração futura para um microserviço independente.<br><br>**Cálculo síncrono sob demanda**, consultando outros módulos a cada requisição — rejeitado por prejudicar o desempenho, requisito não funcional explícito nas RN09 e RN10. |
| **Consequências** | Maior autonomia e desempenho de leitura para o módulo de recomendação, em troca de consistência eventual: os dados podem ficar levemente desatualizados entre a publicação do evento e seu processamento. Esse trade-off é aceitável nesse domínio, pois a recomendação não exige consistência forte ou imediata. |


**ADR 05 - Persistência: Banco Relacional único com caminho de evolução**

| Campo | Descrição |
| :--- | :--- |
| **Contexto** | O núcleo acadêmico possui dados fortemente relacionais (turma–estudante–atividade–nota), enquanto o módulo de recomendação tem leitura intensiva e um modelo de dados mais flexível (perfil do estudante e scores calculados). |
| **Decisão** | Utilizar um único banco de dados relacional (por exemplo, PostgreSQL) para o núcleo no MVP. O módulo de recomendação utiliza tabelas próprias dentro do mesmo banco no MVP, com o design já preparado para migrar futuramente para um banco ou cache dedicado (por exemplo, Redis, para armazenar os scores calculados), caso o sistema precise escalar. |
| **Alternativas Consideradas** | |
| **Consequências** | Simplicidade operacional no MVP, com um caminho de evolução claramente documentado, o que demonstra maturidade arquitetural na avaliação técnica do projeto. |


**Rastreabilidade com as Regras do Negócio**
A tabela a seguir resume como cada grupo de Regras de Negócio se relaciona com os containers da arquitetura proposta, reforçando a justificativa técnica apresentada nas seções anteriores.

| Regras do Negócio | Containers/Componente | Observação |
| :--- | :--- | :--- |
| RN01 - RN05 | Núcleo Acadêmico (Arquitetura em Camadas) | Criação e publicação de atividades, matrícula/acesso, prazos, registro de entregas e correção/notas, todos fluxos síncronos e transacionais. |
| RN06 | Barramento de Eventos + Serviço de Notificação | Notificações de novos materiais, atividades e prazos próximos, processadas de forma assíncrona via Pub-Sub. |
| RN07 – RN08 | Núcleo Acadêmico (dados) + Módulo de Recomendação (consumo) | Preferências e perfil acadêmico são armazenados no núcleo e consumidos via evento pelo Módulo de Recomendação. |
| RN09 – RN10 | Módulo de Recomendação (Pub-Sub + API própria de leitura) | Cálculo de prioridades e geração da agenda semanal personalizada, funcionalidade inovadora, isolada do núcleo. |




# Matriz de Tecnologias Sugerida
A definição da stack tecnológica do OrganizAí foi guiada estritamente pelas necessidades da arquitetura híbrida (Camadas + Publish-Subscribe) e pela viabilidade técnica de implementação do MVP.
- **Java com Spring Boot (Backend):** Escolhido por sua maturidade de mercado na implementação de arquiteturas corporativas em camadas. Através do Spring Security, o framework resolve nativamente o controle de acesso baseado em papéis (RBAC) e a checagem de posse de recurso (garantindo que professores alterem notas apenas de suas próprias turmas). Além disso, o ecossistema Spring simplifica a criação da espinha dorsal de eventos por meio de ferramentas nativas de mensageria e agendamento de tarefas.

- **React (Frontend):** Adotado para a construção de uma interface de usuário no formato Single Page Application (SPA), garantindo uma experiência fluida, reativa e dinâmica para professores e estudantes.

- **PostgreSQL (Banco de Dados):** Fornece a robustez relacional e a consistência ácida necessárias para salvaguardar os dados críticos do núcleo acadêmico, como o histórico de entregas e atribuição de notas.

- **RabbitMQ e Redis (Mensageria e Cache):** O RabbitMQ atua como o broker que viabiliza o desacoplamento assíncrono do Serviço de Notificações e do Módulo de Recomendação. O Redis complementa a infraestrutura funcionando como uma camada de cache para os scores e prioridades calculadas, blindando o requisito não funcional de alto desempenho do sistema.

Em suma, essa combinação de tecnologias reduz o esforço de configuração no MVP, garante alta performance de leitura e deixa as fronteiras dos módulos prontas para uma futura evolução.
