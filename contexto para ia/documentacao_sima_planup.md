# Documentação — SIMA PlanUp

## 1. Visão geral

O **SIMA PlanUp** é um aplicativo desenvolvido para apoiar o monitoramento de projetos de engenharia, integrando cronogramas, documentos e informações de planejamento em uma interface única de consulta, análise e geração de relatórios.

O projeto surgiu para tornar o acompanhamento dos projetos mais ágil, organizado e confiável, reduzindo tarefas manuais e facilitando a comunicação entre equipe interna, planejamento, coordenação e clientes.

A ferramenta permite importar cronogramas em **MS Project**, visualizar atividades, aplicar filtros, gerar gráficos, simular impactos de atraso, consolidar múltiplos cronogramas e produzir boletins informativos com dados relevantes do projeto.

---

## 2. Contexto do projeto

Em projetos de engenharia, o acompanhamento de prazos, atividades, disciplinas, recursos e documentos costuma depender de várias fontes de informação. Normalmente, parte dos dados está em cronogramas do MS Project, parte em planilhas de controle e parte em documentos de acompanhamento do projeto.

Esse modelo gera dificuldade de consulta para pessoas que não possuem licença do MS Project, aumenta o esforço manual do planejador e torna a comunicação mais lenta, principalmente quando é necessário montar boletins, gráficos ou relatórios periódicos.

O SIMA PlanUp centraliza essas informações em uma aplicação própria, permitindo que os dados do cronograma sejam consultados, filtrados, simulados e transformados em relatórios de forma mais simples.

---

## 3. Objetivo do projeto

O objetivo do SIMA PlanUp é automatizar e facilitar o monitoramento de projetos de engenharia, oferecendo uma interface acessível para consulta e análise de cronogramas.

### Objetivos principais

- Integrar cronogramas do MS Project com informações complementares de documentos e planilhas.
- Permitir que a equipe consulte dados de planejamento sem depender diretamente da licença do MS Project.
- Gerar relatórios e boletins informativos de forma automatizada.
- Melhorar a comunicação interna e externa sobre o andamento dos projetos.
- Apoiar a identificação de conflitos de prazo e recursos entre projetos.
- Permitir simulações pontuais sem alterar o arquivo original do cronograma.
- Reduzir falhas causadas por preenchimentos e consolidações manuais.

---

## 4. Problema resolvido

Antes da ferramenta, o acompanhamento de cronogramas e atividades dependia de consultas manuais, extrações em planilhas e montagem manual de relatórios.

Esse processo tinha alguns pontos críticos:

- Nem toda a equipe tinha acesso direto ao MS Project.
- A visualização das atividades dependia do planejador ou de arquivos exportados.
- A criação de relatórios exigia esforço manual recorrente.
- Simulações de atraso podiam afetar o arquivo original caso fossem feitas diretamente no cronograma.
- A comunicação sobre próximas atividades e status do projeto era menos dinâmica.
- A análise de múltiplos cronogramas era mais trabalhosa.

O SIMA PlanUp resolve esses pontos ao criar uma camada de consulta, análise e relatório sobre os cronogramas, sem alterar a origem oficial dos dados.

---

## 5. Público usuário

A ferramenta atende principalmente equipes envolvidas no acompanhamento e execução de projetos de engenharia.

Usuários principais:

- Planejadores.
- Coordenadores de projeto.
- Líderes de disciplina.
- Equipes técnicas.
- Equipes de documentação.
- Gestores que precisam acompanhar avanço, prazos e pendências.
- Clientes que recebem boletins ou informes gerados a partir da ferramenta.

---

## 6. Funcionamento geral

O fluxo básico do SIMA PlanUp parte da importação de um ou mais cronogramas do MS Project. A partir disso, o aplicativo organiza as tarefas em uma interface de consulta, permitindo aplicar filtros, visualizar status, gerar gráficos e emitir relatórios.

Fluxo resumido:

```txt
Cronograma MS Project
→ Importação no SIMA PlanUp
→ Organização das tarefas por projeto, WBS, disciplina, recurso e datas
→ Aplicação de filtros e visualizações
→ Geração de gráficos e relatórios
→ Boletim informativo para acompanhamento interno ou externo
```

O aplicativo também permite trabalhar com mais de um cronograma ao mesmo tempo, possibilitando análises de interface entre projetos e identificação de possíveis conflitos.

---

## 7. Fontes de dados

A ferramenta utiliza informações vindas principalmente de cronogramas e arquivos de apoio.

| Fonte | Uso no projeto |
|---|---|
| Cronograma MS Project | Base de atividades, datas, WBS, recursos, progresso e dependências |
| Planilhas Excel | Informações complementares para boletins e relatórios |
| Documentos de controle | Dados associados ao acompanhamento documental do projeto |
| Inputs manuais pontuais | Pontos de atenção e planos de ação no boletim |

---

## 8. Principais módulos da ferramenta

O SIMA PlanUp pode ser entendido em alguns módulos principais:

```txt
1. Importação de cronogramas
2. Visualização e organização das atividades
3. Filtros de planejamento
4. Gráficos e indicadores
5. Simulação de atraso
6. Visualização múltipla de cronogramas
7. Geração de relatórios e boletins
```

---

# 9. Funcionalidades detalhadas

## 9.1. Importação de cronogramas

A ferramenta permite adicionar cronogramas por meio do botão **Importar MPP**.

Após a importação, as atividades do cronograma passam a ser exibidas na interface do aplicativo, mantendo informações como:

- WBS.
- Nome da tarefa.
- Recurso ou disciplina.
- Data de início.
- Data de término.
- Término baseline.
- Percentual concluído.
- Status crítico.
- Atividade ativa ou pendente.

Essa funcionalidade permite que a equipe visualize o cronograma sem abrir diretamente o MS Project.

---

## 9.2. Organização das atividades

A interface segue uma lógica parecida com ferramentas de planejamento já conhecidas, facilitando a adaptação dos usuários.

Recursos de organização:

- Abrir todas as subtarefas.
- Fechar todas as subtarefas.
- Reordenar colunas arrastando para a posição desejada.
- Personalizar a visualização com colunas importantes.
- Exibir colunas extras temporariamente.
- Retornar para a visualização principal com o botão de colunas importantes.

A personalização de colunas pode ser mantida para usos futuros, deixando a visualização adequada à rotina do usuário.

---

## 9.3. Filtros por data

O aplicativo possui filtros voltados ao acompanhamento de prazos e atividades por período.

Filtros disponíveis:

| Filtro | Função |
|---|---|
| **Início hoje** | Mostra atividades com data de início no dia atual |
| **Terminando hoje** | Mostra atividades com data de término no dia atual |
| **Próx Dias** | Mostra tarefas com término dentro do período escolhido pelo usuário |
| **Início** | Filtra tarefas por uma data específica de início |
| **Fim** | Filtra tarefas por uma data específica de término |

Esses filtros ajudam a equipe a identificar o que precisa começar, terminar ou ser acompanhado em determinado intervalo.

---

## 9.4. Filtros por estrutura, disciplina e status

Além dos filtros de data, a ferramenta possui filtros voltados à estrutura do cronograma e às disciplinas envolvidas.

Filtros disponíveis:

| Filtro | Função |
|---|---|
| **Nível WBS** | Permite escolher o nível da EAP que será exibido |
| **Recurso/Disc** | Filtra por recurso ou disciplina |
| **Cor** | Permite realçar atividades e filtrar por cores aplicadas |
| **Mostrar tarefas pendentes** | Mostra tarefas com percentual concluído diferente de 100% |

Os filtros de WBS, recurso/disciplina e cor podem ser combinados com outros filtros, permitindo análises mais específicas.

---

## 9.5. Gráficos

O SIMA PlanUp permite gerar gráficos em navegador, possibilitando ampliação de trechos e exportação de imagens para compartilhamento.

Gráficos disponíveis:

| Gráfico | Finalidade |
|---|---|
| **Gráfico Gantt** | Visualizar o cronograma em formato semelhante ao MS Project |
| **Curva S** | Comparar avanço baseline x avanço real do projeto |
| **Recursos** | Exibir quantidade de tarefas por disciplina |
| **Horas/Recurso** | Mostrar horas utilizadas por disciplina por dia ou semana |

Esses gráficos ajudam na análise visual do andamento do projeto e facilitam a comunicação com equipe e cliente.

---

## 9.6. Simulação de atraso

Uma das funcionalidades centrais da ferramenta é a simulação de atraso.

Com ela, o usuário pode alterar pontualmente uma atividade e visualizar o impacto nas atividades sucessoras, sem modificar o cronograma original.

Funcionamento:

```txt
Atividade selecionada
→ Simulação de atraso
→ Reprogramação visual das sucessoras
→ Atualização dos gráficos no modo simulado
→ Desativação da simulação
→ Retorno ao cronograma original
```

Essa funcionalidade é útil para avaliar impactos antes de qualquer decisão formal de reprogramação.

Importante: o arquivo original do MS Project não é alterado. As modificações feitas no modo de simulação são descartadas quando a simulação é desabilitada.

---

## 9.7. Visualização múltipla de cronogramas

A ferramenta permite adicionar mais de um cronograma ao mesmo tempo.

Essa funcionalidade possibilita:

- Comparar projetos diferentes.
- Avaliar interfaces entre cronogramas.
- Identificar conflitos de prazo.
- Identificar conflitos de recursos.
- Analisar tarefas de projetos diferentes em uma visão consolidada.
- Consultar projetos separadamente em abas específicas.

Para isso, o usuário pode utilizar o botão **Adicionar MPP** e selecionar os cronogramas desejados.

---

## 9.8. Relatórios de próximas atividades

O aplicativo permite gerar relatórios de tarefas com base em um período definido pelo usuário.

A lógica do relatório considera:

- Tarefas com início no dia em que o relatório foi gerado.
- Tarefas com término até a data selecionada.
- Tarefas organizadas por projeto e disciplina.
- Consolidação das atividades em uma planilha resumo.
- Separação dos cronogramas em abas individuais quando houver múltiplos projetos.

Essa função melhora a comunicação semanal e facilita o alinhamento entre as equipes.

---

## 9.9. Boletim informativo

A ferramenta também foi projetada para gerar automaticamente um boletim informativo semanal.

O boletim pode conter:

- Atividades relevantes do projeto.
- Datas importantes.
- Curva de avanço.
- Controle de documentação.
- Atividades desenvolvidas no período.
- Atividades planejadas para a semana seguinte.
- Pontos de atenção.
- Planos de ação.
- Logo do projeto ou cliente.

Para montar esse boletim, a ferramenta integra dados do cronograma e de um arquivo Excel, consolidando as informações em um relatório customizado.

Dois campos permanecem manuais no boletim:

- Pontos de atenção.
- Planos de ação.

Esses campos continuam sob responsabilidade da equipe, pois dependem de análise qualitativa e decisão de gestão.

---

# 10. Indicadores e informações acompanhadas

A ferramenta permite acompanhar diferentes indicadores do projeto, incluindo:

- Indicadores de prazo.
- Datas baseline.
- Datas previstas.
- Status por etapa.
- Controle de comentários.
- Número de documentos previstos.
- Número de documentos emitidos.
- Número de documentos aprovados.
- GRDs emitidas.
- Curva de avanço.
- Atividades realizadas.
- Planejamento de atividades futuras.
- Pontos de atenção.
- Planos de ação.

Essas informações podem ser utilizadas tanto para acompanhamento interno quanto para comunicação com clientes.

---

# 11. Regras de negócio

## 11.1. O cronograma original não deve ser alterado

O arquivo do MS Project continua sendo a fonte oficial do planejamento e deve ser atualizado apenas pelo planejador responsável.

As simulações realizadas dentro do SIMA PlanUp não alteram o arquivo original.

---

## 11.2. A visualização pode ser personalizada

O usuário pode alterar a ordem das colunas e selecionar quais informações são importantes para sua rotina.

Quando colunas são adicionadas à visualização principal, a formatação pode ser mantida para usos futuros.

---

## 11.3. Colunas extras podem ser temporárias

Caso o usuário queira consultar informações adicionais sem alterar a visualização principal, pode utilizar a opção de colunas extras.

Ao retornar para a visualização de colunas importantes, a interface volta para a configuração principal.

---

## 11.4. Filtros podem ser combinados

Filtros como nível WBS, recurso/disciplina e cor podem ser usados junto com filtros de prazo, permitindo consultas mais específicas.

Exemplo:

```txt
Disciplina = Tubulação
+ Próximos dias = 7
+ Tarefas pendentes
```

Resultado esperado: visualizar atividades pendentes de Tubulação com término previsto nos próximos 7 dias.

---

## 11.5. Simulações são descartáveis

Ao desabilitar o modo de simulação, todas as alterações temporárias são esquecidas e o aplicativo retorna à visualização original do cronograma.

---

## 11.6. O boletim pode ser customizado

O boletim foi pensado a partir de um modelo de dashboard de engenharia, mas pode ser adaptado conforme a necessidade do projeto, cliente ou equipe.

Também é possível escolher um logo para compor o relatório.

---

# 12. Entradas e saídas do sistema

## Entradas

| Entrada | Descrição |
|---|---|
| Arquivo MPP | Cronograma do projeto em MS Project |
| Arquivo Excel | Base complementar para boletins e relatórios |
| Configuração de filtros | Datas, disciplinas, WBS, cores e status |
| Inputs manuais | Pontos de atenção e planos de ação |
| Logo | Imagem utilizada para personalização do boletim |

## Saídas

| Saída | Descrição |
|---|---|
| Visualização consolidada | Lista de tarefas organizada no aplicativo |
| Gráfico Gantt | Representação visual do cronograma |
| Curva S | Comparativo de avanço baseline x real |
| Gráfico de recursos | Distribuição de tarefas por disciplina |
| Gráfico de horas/recurso | Horas utilizadas por disciplina por período |
| Relatório de tarefas | Atividades filtradas por período e disciplina |
| Boletim informativo | Documento de acompanhamento do projeto |

---

# 13. Benefícios gerados

## Acesso facilitado

A equipe pode consultar informações de planejamento sem depender diretamente da licença do MS Project, bastando ter acesso ao local onde o cronograma atualizado está salvo.

## Comunicação mais ágil

Relatórios, gráficos e boletins tornam a comunicação com a equipe e com o cliente mais rápida e visual.

## Menos trabalho manual

Como parte das consultas, gráficos e relatórios é gerada automaticamente, o planejador reduz o tempo gasto em atividades operacionais.

## Redução de falhas

Ao diminuir inputs manuais e retrabalho de consolidação, a ferramenta reduz inconsistências e erros de transcrição.

## Melhor tomada de decisão

Filtros, gráficos, simulações e visualização múltipla ajudam a identificar conflitos, atrasos, gargalos e impactos antes que eles comprometam o projeto.

## Foco estratégico

Com menos esforço em montagem manual de informações, o planejador pode direcionar mais tempo para análise, tomada de decisão e ações de maior valor.

---

# 14. Exemplo de uso prático

Um coordenador precisa verificar quais atividades de determinada disciplina estão pendentes para a próxima semana.

Fluxo de uso:

```txt
1. Importar o cronograma MPP atualizado.
2. Filtrar pela disciplina desejada.
3. Aplicar o filtro de próximos dias.
4. Marcar tarefas pendentes.
5. Gerar relatório ou gráfico.
6. Compartilhar o resultado com a equipe ou incluir no boletim.
```

Resultado: a equipe recebe uma visão objetiva das atividades que precisam de atenção no período.

---

# 15. Nome do projeto

Nome utilizado:

```txt
SIMA PlanUp
```

Descrição curta sugerida:

```txt
Aplicativo de monitoramento, simulação e comunicação de cronogramas de engenharia.
```

Descrição executiva sugerida:

```txt
Ferramenta interna da SIMA para integração de cronogramas, análise de prazos, geração de gráficos, simulações e boletins informativos de projetos de engenharia.
```

---

# 16. Resumo executivo

O SIMA PlanUp é uma ferramenta desenvolvida para melhorar o acompanhamento de projetos de engenharia por meio da integração de cronogramas MS Project, arquivos Excel e informações de planejamento.

A solução permite importar cronogramas, visualizar atividades, aplicar filtros por data, WBS, disciplina e status, gerar gráficos de Gantt, Curva S, recursos e horas, além de simular atrasos sem alterar o arquivo original.

Também possibilita a visualização de múltiplos cronogramas e a geração de relatórios e boletins informativos, apoiando a comunicação interna e externa dos projetos.

Com isso, o projeto reduz tarefas manuais, melhora a confiabilidade das informações, amplia o acesso da equipe aos dados de planejamento e oferece suporte à tomada de decisão em projetos de engenharia.
