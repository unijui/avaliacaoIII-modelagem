# OrganizAí – Funcionalidades e Priorização

## 1. Funcionalidades Essenciais (MVP)

As funcionalidades essenciais compõem o núcleo da plataforma, sem as quais o produto não cumpre sua proposta de valor básica: centralizar a gestão de sala de aula entre professores e estudantes.

---

### 1.1 Controle de Acesso por Perfil

**Descrição:** O sistema deve autenticar e autorizar usuários de acordo com seu perfil (professor, estudante, coordenador, administrador), garantindo que cada um acesse apenas o que lhe é permitido.

**Base nas Regras de Negócio:** RN01, RN02, RN05

**Funcionalidades incluídas:**
- Cadastro e autenticação de usuários
- Controle de permissões por perfil
- Vinculação de estudantes a turmas específicas
- Restrição de acesso a materiais e atividades por matrícula

---

### 1.2 Gestão de Atividades pelo Professor

**Descrição:** O professor deve poder criar, editar e publicar atividades para suas turmas, definindo obrigatoriamente um prazo de entrega para cada uma.

**Base nas Regras de Negócio:** RN01, RN03

**Funcionalidades incluídas:**
- Criação e edição de atividades
- Definição de prazo de entrega (campo obrigatório)
- Publicação de atividades para turmas específicas
- Gerenciamento de materiais de apoio

---

### 1.3 Entrega de Atividades pelo Estudante

**Descrição:** Estudantes matriculados devem conseguir visualizar e entregar atividades dentro do prazo, com registro automático da data e horário de cada submissão.

**Base nas Regras de Negócio:** RN02, RN03, RN04

**Funcionalidades incluídas:**
- Visualização das atividades da turma
- Submissão de atividades com validação de prazo
- Registro automático de data e horário de entrega
- Histórico de submissões do estudante

---

### 1.4 Correção de Atividades e Atribuição de Notas

**Descrição:** O professor responsável por uma turma deve poder corrigir as atividades entregues e atribuir notas, sendo impedido de alterar avaliações de turmas que não são suas.

**Base nas Regras de Negócio:** RN04, RN05

**Funcionalidades incluídas:**
- Visualização das entregas por atividade e por estudante
- Correção com atribuição de nota e feedback
- Restrição de edição de notas ao professor responsável pela turma
- Acompanhamento do progresso da turma

---

### 1.5 Notificações Automáticas

**Descrição:** O sistema deve enviar notificações automáticas aos estudantes sobre publicação de novos materiais, novas atividades e aproximação de prazos de entrega.

**Base nas Regras de Negócio:** RN06

**Funcionalidades incluídas:**
- Notificação de novo material publicado
- Notificação de nova atividade disponível
- Alerta de prazo de entrega próximo
- Canal de entrega de notificações (in-app e/ou e-mail)

---

## 2. Funcionalidades Futuras

As funcionalidades futuras representam diferenciais inovadores do OrganizAí. Elas agregam valor ao produto, mas dependem da estabilidade do núcleo essencial para serem implementadas com qualidade.

---

### 2.1 Perfil Acadêmico do Estudante

**Descrição:** O estudante poderá cadastrar suas preferências de organização acadêmica (estratégia de priorização) e informar em quais disciplinas possui maior facilidade ou dificuldade.

**Base nas Regras de Negócio:** RN07, RN08

**Funcionalidades incluídas:**
- Configuração de estratégia de priorização (mais difíceis, mais fáceis, prazo mais próximo, equilíbrio)
- Registro de facilidades e dificuldades por disciplina
- Armazenamento e edição do perfil acadêmico

---

### 2.2 Priorização Automática de Atividades

**Descrição:** Com base no perfil do estudante e nas características das atividades, o sistema calculará automaticamente uma ordem de prioridade para as tarefas pendentes.

**Base nas Regras de Negócio:** RN09

**Funcionalidades incluídas:**
- Cálculo de prioridade considerando: prazo, dificuldade, preferências, perfil acadêmico e volume de pendências
- Exibição da lista de atividades ordenada por prioridade
- Atualização dinâmica da priorização conforme novas atividades são publicadas

---

### 2.3 Agenda Semanal Personalizada

**Descrição:** O sistema gerará automaticamente uma agenda semanal de estudos distribuindo as atividades ao longo dos dias, com base na disponibilidade de tempo informada pelo estudante e nas prioridades calculadas.

**Base nas Regras de Negócio:** RN10

**Funcionalidades incluídas:**
- Cadastro de disponibilidade de tempo por dia da semana
- Geração automática de agenda semanal personalizada
- Distribuição de atividades considerando horas disponíveis, prioridades e prazos
- Visualização semanal do planejamento de estudos

---

## 3. Justificativa da Priorização

### 3.1 Critérios Adotados

A priorização das funcionalidades foi realizada com base em três critérios principais:

**Valor central do produto:** funcionalidades que atendem diretamente à proposta de valor declarada na Visão do Produto — centralizar a comunicação e gestão entre professores e estudantes — foram classificadas como essenciais.

**Dependências técnicas:** as funcionalidades inovadoras (priorização e agenda personalizada) dependem de dados gerados pelas funcionalidades essenciais (atividades criadas, prazos definidos, entregas registradas). Portanto, não podem ser desenvolvidas antes do núcleo estar estável.

**Risco e complexidade:** as funcionalidades futuras envolvem módulos de maior complexidade (motor de recomendação, algoritmo de planejamento), que exigem mais tempo de design, validação e testes, justificando um desenvolvimento incremental.

---

### 3.2 Por que as funcionalidades essenciais vêm primeiro?

| Funcionalidade | Justificativa |
|---|---|
| Controle de Acesso | Pré-requisito de segurança para todas as demais funções; sem ele, nenhuma outra regra de negócio pode ser aplicada corretamente. |
| Gestão de Atividades | É o ponto de partida de toda a jornada do professor na plataforma; sem atividades, o produto não tem conteúdo. |
| Entrega de Atividades | É o ponto de partida da jornada do estudante; sem ela, o produto não cumpre seu objetivo central. |
| Correção e Notas | Fecha o ciclo pedagógico e garante que professores e estudantes vejam resultado concreto do uso da plataforma. |
| Notificações | Reduz o risco de abandono por esquecimento, aumentando engajamento mesmo no MVP. |

---

### 3.3 Por que as funcionalidades inovadoras são futuras?

As funcionalidades de **perfil acadêmico**, **priorização automática** e **agenda semanal** representam o diferencial competitivo do OrganizAí frente a outras plataformas de gestão de sala de aula. No entanto, sua entrega prematura traria riscos:

- **Dependência de dados reais:** o algoritmo de priorização (RN09) e a geração de agenda (RN10) só têm utilidade quando há atividades, prazos e entregas cadastrados — elementos que só existem após o MVP estar em uso.
- **Complexidade de validação:** recomendar uma ordem de estudos personalizada requer validação com usuários reais para que os resultados sejam percebidos como úteis e não como ruídos.
- **Custo de desenvolvimento elevado:** o módulo de recomendação e planejamento acadêmico envolve lógica mais sofisticada, aumentando o risco técnico se desenvolvido sem uma base estável.

Portanto, essas funcionalidades são priorizadas para uma segunda fase do produto, quando a plataforma já estiver em uso e houver dados suficientes para torná-las genuinamente úteis.