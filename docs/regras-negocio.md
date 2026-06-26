# Regras de Negócio

## RN01 – Apenas professores podem criar e publicar atividades

Somente usuários com perfil de professor poderão cadastrar, editar e disponibilizar atividades para as turmas sob sua responsabilidade.

**Possíveis impactos:**

**User Story:** Como professor, desejo criar e publicar atividades para disponibilizar conteúdos e avaliações aos estudantes.

**Caso de Uso:** Criar Atividade.ffgfud

**BPMN:** publicação realizada exclusivamente pelo professor.

**Arquitetura:** necessidade de controle de autenticação e autorização.

**Requisito Não Funcional:** segurança. 


## RN02 – Apenas estudantes matriculados em uma turma podem acessar seus materiais e entregar atividades

O acesso aos conteúdos, materiais e atividades será restrito aos estudantes vinculados à respectiva turma. 

**Possíveis impactos:**

**User Story:** Como estudante matriculado, desejo acessar materiais e entregar atividades da minha turma para acompanhar meu aprendizado. 

**Caso de Uso:** Entregar Atividade.

**BPMN:** validação de matrícula antes da liberação do acesso.

**Arquitetura:** controle de acesso baseado em perfil.

**Requisito Não Funcional:** segurança. 


## RN03 – Toda atividade deve possuir prazo de entrega definido pelo professor
No momento da criação de uma atividade, o professor deverá informar obrigatoriamente uma data limite para submissão. 

**Possíveis impactos:**

**User Story:** Como professor, desejo definir um prazo de entrega para cada atividade a fim de organizar o cronograma da disciplina. 

**Caso de Uso:** Criar Atividade. 

**BPMN:** verificação automática do prazo durante o processo de entrega.

**Arquitetura:** armazenamento e validação das datas de entrega. 

**Outros impactos:** armazenamento da data limite no banco de dados. 

## RN04 – O sistema deve registrar todas as entregas realizadas pelos estudantes
Cada submissão deverá ser armazenada juntamente com a data e horário de envio para fins de acompanhamento e auditoria. 

**Possíveis impactos:**

**User Story:** Como professor, desejo visualizar o histórico de entregas para acompanhar a participação dos estudantes. 

**Caso de Uso:** Entregar Atividade.

**BPMN:** registro automático da submissão realizada.

**Arquitetura:** armazenamento de histórico e rastreabilidade.

**Requisito Não Funcional:** auditabilidade.

**Outros impactos:** histórico de atividades e rastreabilidade das submissões.

## RN05 – Apenas o professor responsável pela turma pode corrigir atividades e atribuir notas
Professores não poderão alterar notas ou correções de turmas que não estejam sob sua responsabilidade. 

**Possíveis impactos:**

**User Story:** Como professor, desejo corrigir apenas as atividades das minhas turmas para garantir a integridade das avaliações. 

**Caso de Uso:** Corrigir Atividade.

**Caso de Uso:** Atribuir Nota.

**BPMN:** validação do responsável pela turma antes da correção.

**Arquitetura:** controle de permissões.

**Requisito Não Funcional:** segurança.

## RN06 – O sistema deve notificar estudantes sobre novos materiais, atividades e prazos próximos
O sistema deverá enviar notificações sempre que houver publicação de novos materiais, novas atividades ou aproximação de prazos de entrega. 

**Possíveis impactos:**

**User Story:** Como estudante, desejo receber notificações sobre atividades e prazos para não perder compromissos acadêmicos. 

**Caso de Uso:** Receber Notificação.

**BPMN:** envio automático de notificações.

**Arquitetura:** serviço de notificações.

**Requisito Não Funcional:** disponibilidade.

## RN07 – O estudante poderá cadastrar suas preferências de organização acadêmica
As informações fornecidas pelo estudante serão utilizadas pelo sistema para personalizar recomendações e priorizações de atividades. As preferências poderão incluir:

- Priorizar atividades mais difíceis;
- Priorizar atividades mais fáceis;
- Priorizar atividades com prazo mais próximo;
- Utilizar estratégia equilibrada.

**Possíveis impactos:**

**User Story:** Como estudante, desejo definir minhas preferências de organização para receber recomendações adequadas ao meu perfil. 

**Caso de Uso:** Configurar Preferências de Estudo.

**BPMN:** utilização das preferências durante a geração do planejamento. 

**Arquitetura:** armazenamento do perfil do estudante.

**Outros impactos:** planejamento personalizado e funcionalidade inovadora. 

## RN08 – O estudante poderá informar disciplinas nas quais possui maior facilidade ou dificuldade
As informações fornecidas serão utilizadas pelo sistema para personalizar recomendações e prioridades de estudo. 

**Possíveis impactos:**

**User Story:** Como estudante, desejo informar minhas facilidades e dificuldades para receber um planejamento mais adequado às minhas necessidades.

**Caso de Uso:** Gerenciar Perfil Acadêmico.

**BPMN:** utilização do perfil acadêmico na geração das recomendações.

**Arquitetura:** armazenamento do perfil acadêmico.

**Outros impactos:** planejamento personalizado e funcionalidade inovadora.

## RN09 – O sistema deverá calcular automaticamente a prioridade das atividades
O cálculo deverá considerar: 
- prazo de entrega;
- dificuldade da atividade;
- preferências do estudante;
- perfil acadêmico do estudante;
- quantidade de atividades pendentes.

**Possíveis impactos:**

**User Story:** Como estudante, desejo visualizar minhas atividades priorizadas para saber em qual tarefa devo focar primeiro.

**Caso de Uso:** Gerar Prioridades.

**BPMN:** cálculo automático das prioridades.

**Arquitetura:** módulo de recomendação.

**Requisito Não Funcional:** desempenho.

**Outros impactos:** funcionalidade inovadora.

## RN10 – O sistema deverá gerar uma agenda semanal personalizada com base na disponibilidade informada pelo estudante
O planejamento deverá considerar:
- prazos das atividades;
- preferências de organização do estudante;
- disciplinas em que possui maior facilidade ou dificuldade;
- disponibilidade de tempo informada pelo estudante.
A agenda deverá distribuir as atividades ao longo da semana considerando:
- horas disponíveis por dia;
- prioridades calculadas;
- prazo das atividades;
- perfil acadêmico do estudante.

**Possíveis impactos:**

**User Story:** Como estudante, desejo receber uma agenda semanal personalizada para organizar melhor meu tempo de estudo.

**Caso de Uso:** Gerar Agenda de Estudos.

**BPMN:** geração automática do planejamento semanal.

**Arquitetura:** módulo de planejamento acadêmico personalizado.

**Requisito Não Funcional:** desempenho.

**Outros impactos:** funcionalidade inovadora.