# Documentação — Sistema de Gerenciamento de Dados

## 1. Visão geral

O **Sistema de Gerenciamento de Dados** é uma solução interna desenvolvida para centralizar, organizar e automatizar o fluxo de trabalho relacionado a instrumentos de processo.

A solução funciona dentro do ambiente do **Microsoft Excel**, utilizando macros/VBA para transformar dados extraídos de um **P&ID parametrizado** em informações estruturadas, editáveis e reutilizáveis. A partir de um arquivo TXT gerado pelo AutoCAD, o sistema importa a lista de instrumentos, organiza os dados por TAG, permite ajustes controlados, gera folhas de dados padronizadas e possibilita a exportação das informações de volta para o P&ID.

O projeto reduz retrabalho, aumenta a padronização dos documentos entregues e melhora o controle das revisões, evitando que a equipe precise copiar, reorganizar ou reformatar manualmente informações repetitivas.

---

## 2. Objetivo do projeto

Automatizar o gerenciamento de dados de instrumentos, conectando o fluxo de informações entre **AutoCAD**, **P&ID parametrizado**, **Excel** e **folhas de dados**.

### Objetivos principais

- Importar automaticamente dados de instrumentos extraídos do AutoCAD.
- Centralizar os dados em uma aba mestre no Excel.
- Criar abas individuais por instrumento ou TAG.
- Permitir edição e organização dos atributos dos instrumentos.
- Gerar folhas de dados padronizadas a partir de templates.
- Controlar o ciclo de revisões das folhas de dados.
- Exportar os dados editados em TXT para reimportação no AutoCAD.
- Reduzir retrabalho e inconsistências entre documentos técnicos.

---

## 3. Problema resolvido

Antes do sistema, o fluxo de dados dos instrumentos dependia de várias etapas manuais:

1. Extrair informações do P&ID.
2. Organizar os dados em planilhas.
3. Separar instrumentos por tipo ou TAG.
4. Copiar informações para folhas de dados.
5. Ajustar documentos manualmente.
6. Controlar revisões de forma descentralizada.
7. Atualizar novamente o P&ID com as alterações feitas.

Esse processo gerava riscos como:

- Divergência entre P&ID e documentos gerados.
- Erros de cópia e preenchimento.
- Falta de padronização nas folhas de dados.
- Perda de histórico de revisão.
- Retrabalho em alterações simples.
- Dificuldade para reaproveitar dados já tratados.

O sistema resolve esse problema criando um fluxo mais integrado e automatizado, no qual os dados saem do AutoCAD, são tratados no Excel e podem retornar ao P&ID de forma estruturada.

---

## 4. Tecnologias e ferramentas utilizadas

| Tecnologia / Ferramenta | Uso no projeto |
|---|---|
| **Microsoft Excel** | Ambiente principal do sistema |
| **VBA / Macros** | Automação das funções internas |
| **AutoCAD** | Origem e destino dos dados dos instrumentos |
| **Express Tools** | Exportação e importação de atributos no AutoCAD |
| **TXT estruturado** | Formato intermediário para troca de dados |
| **Templates Excel (.xlsx)** | Base para geração das folhas de dados |
| **Arquivos .xlsm** | Folhas de dados geradas com macros e controle de revisão |
| **Aba CONFIG** | Armazenamento dos caminhos configurados |
| **Aba Dados_Geral** | Base mestre de dados importados |
| **Aba Planilha_Funções** | Painel principal de operação |

---

## 5. Pré-requisitos

Para utilizar o sistema corretamente, o usuário precisa atender aos seguintes requisitos:

- Utilizar **Excel 2019 ou superior**.
- Preferencialmente utilizar **Microsoft 365**.
- Habilitar macros ao abrir o arquivo.
- Habilitar o acesso ao projeto VBA.
- Configurar os caminhos iniciais na aba `CONFIG`.
- Ter os blocos do AutoCAD devidamente parametrizados.
- Utilizar arquivos de template compatíveis com o sistema.

### Configuração de segurança necessária no Excel

No Excel, é necessário habilitar o acesso ao modelo de objeto do projeto VBA:

```txt
Arquivo → Opções → Central de Confiabilidade → Configurações de Macro → Confiar no acesso ao modelo de objeto do projeto VBA
```

Essa permissão é necessária para que o sistema consiga executar corretamente as rotinas automatizadas.

---

## 6. Funcionamento geral

O fluxo do sistema pode ser resumido da seguinte forma:

```txt
P&ID parametrizado no AutoCAD
→ Exportação dos atributos para TXT
→ Importação do TXT no Excel
→ Organização dos dados na aba Dados_Geral
→ Criação de abas por instrumento ou lista
→ Edição e sincronização dos atributos
→ Geração de folhas de dados a partir de templates
→ Controle de revisão das folhas geradas
→ Exportação de TXT atualizado
→ Reimportação dos dados no AutoCAD
```

O Excel funciona como o centro do processo, conectando os dados extraídos do P&ID com os documentos técnicos gerados.

---

## 7. Estrutura principal do sistema

O arquivo principal do sistema é o `Gerenciamento_Dados.xlsm`.

Ele concentra o painel de funções, as macros, as abas de controle e as rotinas de geração de documentos.

### Principais áreas do arquivo

| Área | Função |
|---|---|
| `Planilha_Funções` | Painel principal com os botões de operação |
| `CONFIG` | Guarda os caminhos das pastas configuradas |
| `Dados_Geral` | Aba mestre com os dados importados |
| Abas individuais | Abas geradas por instrumento ou TAG |
| Lista de instrumentos | Aba consolidada com instrumentos e atributos |
| Folhas de dados geradas | Documentos finais criados a partir dos templates |

---

## 8. Painel de funções

O painel principal apresenta os botões usados pelo usuário para executar as etapas do processo.

### Botões principais

| Botão | Função |
|---|---|
| **Importar Dados** | Abre o TXT extraído do AutoCAD e importa os dados para a aba mestre |
| **Criar Abas** | Cria abas individuais para os instrumentos selecionados |
| **Sincronizar Dados** | Salva alterações feitas nas abas e sincroniza com a base principal |
| **Salvar TXT** | Exporta os dados tratados para um novo TXT estruturado |
| **Editar Colunas** | Permite adicionar ou remover atributos das abas |
| **Gerar FD** | Gera folhas de dados com base em templates |
| **Editar FD** | Abre uma folha de dados existente para edição e avanço de revisão |
| **Configurar Caminhos** | Salva os caminhos das pastas utilizadas pelo sistema |

Esse painel reduz a necessidade de o usuário acessar diretamente macros ou código VBA. A operação fica concentrada em botões com funções específicas.

---

## 9. Configuração inicial de caminhos

Antes do primeiro uso, o usuário precisa configurar os caminhos das pastas utilizadas pelo sistema.

### Processo

1. Acessar a aba `Planilha_Funções`.
2. Clicar em **Configurar Caminhos**.
3. Selecionar a pasta onde as folhas de dados serão salvas.
4. Selecionar a pasta onde estão os templates `.xlsx`.
5. Confirmar a seleção.
6. Os caminhos são gravados automaticamente na aba `CONFIG`.

Essa configuração precisa ser feita uma vez por computador, desde que os caminhos das pastas não sejam alterados.

---

## 10. Extração de dados pelo AutoCAD

A primeira etapa operacional ocorre no AutoCAD.

Para que o sistema funcione, os blocos do P&ID precisam estar parametrizados com os atributos necessários.

### Etapas no AutoCAD

1. Garantir que os blocos estejam parametrizados.
2. Selecionar os instrumentos que devem ser extraídos.
3. Acessar a aba **Express Tools**.
4. Utilizar a função **Export Attributes**.
5. Selecionar a pasta onde o TXT será salvo.
6. Gerar o arquivo TXT com as informações dos instrumentos.

Esse TXT será a entrada principal do sistema no Excel.

---

## 11. Importar Dados

A função **Importar Dados** importa para o Excel a lista de instrumentos extraída do P&ID parametrizado.

### Funcionamento

1. O usuário clica em **Importar Dados**.
2. O sistema abre o explorador de arquivos.
3. O usuário seleciona o arquivo TXT de origem.
4. O sistema lê o conteúdo do TXT.
5. As colunas são mapeadas automaticamente pelo nome.
6. Os dados são inseridos na aba `Dados_Geral`.

### Resultado

Ao final da importação, a aba `Dados_Geral` passa a conter a base mestre dos instrumentos importados.

Essa base será usada pelas próximas funções do sistema.

---

## 12. Criar Abas

A função **Criar Abas** gera abas individuais para instrumentos listados na base mestre.

### Objetivo

Organizar os dados por instrumento ou TAG, facilitando a visualização, edição e controle dos atributos.

### Funcionamento

1. O usuário clica em **Criar Abas**.
2. Seleciona o tipo de instrumento ou TAG desejada.
3. O sistema localiza os instrumentos correspondentes.
4. As abas são criadas automaticamente.
5. Os atributos são distribuídos conforme os dados da aba mestre.

### Resultado

O usuário passa a ter abas específicas para trabalhar com grupos de instrumentos, sem precisar manipular diretamente toda a base geral.

---

## 13. Editar Colunas

A função **Editar Colunas** permite personalizar os atributos exibidos em cada aba criada.

### Uso esperado

Essa função é utilizada quando uma aba precisa mostrar, ocultar, adicionar ou remover atributos específicos.

### Funcionamento

1. O usuário clica em **Editar Colunas**.
2. Seleciona a aba que deseja editar.
3. Seleciona o atributo que será adicionado ou removido.
4. O sistema atualiza a estrutura da aba.

### Benefício

Permite que diferentes tipos de instrumento tenham visualizações adequadas, sem obrigar todos os instrumentos a seguirem exatamente a mesma composição de colunas.

---

## 14. Sincronizar Dados

A função **Sincronizar Dados** salva as alterações realizadas nas abas individuais e atualiza a base principal.

### Objetivo

Garantir que as edições feitas em abas específicas retornem para a base consolidada do sistema.

### Resultado

As alterações deixam de ficar isoladas em uma aba e passam a compor novamente a estrutura mestre usada para gerar documentos e exportações.

---

## 15. Lista de Instrumentos

O sistema também permite gerar uma lista consolidada de instrumentos.

### Funcionamento

1. O usuário clica em **Criar Abas**.
2. Seleciona a opção de **Lista de Instrumentos**.
3. O sistema apresenta todos os instrumentos listados e seus atributos.
4. O usuário pode revisar, editar e preencher informações.
5. Ao final, pode gerar o documento final da lista de instrumentos.

### Resultado

A lista de instrumentos centraliza os dados necessários para consulta, conferência e emissão documental.

---

## 16. Gerar FD

A função **Gerar FD** cria folhas de dados a partir de templates padronizados.

### Funcionamento

1. O usuário clica em **Gerar FD**.
2. Seleciona o template desejado em formato `.xlsx`.
3. Seleciona os instrumentos que devem compor a folha de dados.
4. Clica em **Gerar**.
5. O sistema preenche automaticamente os dados dos instrumentos.
6. A revisão inicial é definida como `A`.
7. A data é preenchida automaticamente.
8. As abas são protegidas.
9. O arquivo é salvo como `.xlsm` na pasta configurada.

### Resultado

O sistema gera uma folha de dados pronta para revisão, aprovação ou envio, seguindo o padrão definido no template.

---

## 17. Editar FD

A função **Editar FD** controla a edição de folhas de dados já geradas.

### Funcionamento

1. O usuário clica em **Editar FD**.
2. Seleciona uma folha de dados `.xlsm` existente.
3. Informa a descrição das alterações.
4. O sistema desbloqueia as abas para edição.
5. A revisão é avançada automaticamente.
6. A nova revisão é registrada na tabela da `CAPA`, com data e descrição.
7. O usuário faz as alterações necessárias e salva o arquivo normalmente.

### Controle de revisão

O sistema avança as revisões automaticamente, por exemplo:

```txt
A → B → C → D
```

Após 7 revisões, a revisão mais antiga é substituída automaticamente para manter o histórico dentro do espaço disponível na CAPA.

### Benefício

Essa função evita controle manual de revisão e reduz o risco de documentos serem enviados com revisão incorreta ou histórico incompleto.

---

## 18. Salvar TXT

A função **Salvar TXT** exporta os dados tratados no Excel para um arquivo TXT estruturado.

### Objetivo

Permitir que as alterações feitas no Excel sejam levadas de volta para o P&ID parametrizado no AutoCAD.

### Funcionamento

1. O sistema lê os dados da base mestre.
2. Organiza as informações no formato esperado pelo AutoCAD.
3. Gera um arquivo TXT estruturado.
4. O TXT pode ser utilizado na função de importação de atributos do AutoCAD.

---

## 19. Importação do TXT no AutoCAD

Depois que o TXT atualizado é gerado pelo Excel, ele pode ser importado novamente no AutoCAD.

### Etapas no AutoCAD

1. Acessar a aba **Express Tools**.
2. Clicar em **Import Attributes**.
3. Selecionar o arquivo TXT gerado pelo sistema.
4. Confirmar a importação.
5. Os atributos dos blocos parametrizados são atualizados no P&ID.

Essa etapa fecha o ciclo de ida e volta dos dados entre AutoCAD e Excel.

---

## 20. Fluxo resumido em etapas

```txt
1. Parametrizar os blocos no P&ID.
2. Exportar os atributos pelo AutoCAD em TXT.
3. Abrir o Gerenciamento_Dados.xlsm.
4. Configurar os caminhos, caso seja o primeiro uso.
5. Importar o TXT para a aba Dados_Geral.
6. Criar abas por instrumento, TAG ou lista de instrumentos.
7. Editar atributos e colunas conforme necessário.
8. Sincronizar os dados editados com a base mestre.
9. Gerar folhas de dados a partir de templates.
10. Editar folhas de dados existentes quando houver revisão.
11. Salvar TXT atualizado.
12. Importar o TXT no AutoCAD para atualizar os atributos do P&ID.
```

---

## 21. Entradas do sistema

| Entrada | Descrição |
|---|---|
| TXT exportado do AutoCAD | Contém os atributos extraídos dos blocos parametrizados |
| Templates `.xlsx` | Modelos usados para gerar folhas de dados |
| Dados editados nas abas | Ajustes feitos pelo usuário no Excel |
| Descrição de revisão | Texto informado ao editar uma FD existente |
| Caminhos configurados | Locais de salvamento e templates usados pelo sistema |

---

## 22. Saídas do sistema

| Saída | Descrição |
|---|---|
| Aba `Dados_Geral` | Base mestre importada e organizada |
| Abas por instrumento | Visualizações individuais por TAG ou tipo |
| Lista de instrumentos | Documento consolidado dos instrumentos |
| Folhas de dados `.xlsm` | Documentos finais gerados a partir dos templates |
| Histórico de revisão | Registro na CAPA da folha de dados |
| TXT atualizado | Arquivo para reimportação no AutoCAD |

---

## 23. Regras de negócio

### 23.1. Os blocos precisam estar parametrizados

A extração depende dos atributos configurados nos blocos do AutoCAD. Se o bloco não estiver parametrizado, os dados não serão exportados corretamente.

### 23.2. O TXT é o formato intermediário do processo

A troca de dados entre AutoCAD e Excel é feita por arquivos TXT estruturados.

### 23.3. As colunas são mapeadas pelo nome

Na importação, o sistema identifica as colunas pelo nome, reduzindo a dependência de posição fixa dos campos.

### 23.4. A aba `Dados_Geral` é a base mestre

As informações importadas e sincronizadas devem retornar para essa base para garantir consistência.

### 23.5. As folhas de dados são geradas a partir de templates

Os documentos finais dependem dos modelos previamente configurados.

### 23.6. A revisão inicial é `A`

Ao gerar uma nova folha de dados, o sistema aplica automaticamente a revisão inicial `A`.

### 23.7. Editar uma FD avança a revisão

Ao abrir uma FD existente para edição, o sistema incrementa automaticamente a revisão e registra a descrição da alteração.

### 23.8. O histórico de revisão tem limite

Após 7 revisões, a mais antiga é substituída automaticamente.

---

## 24. Benefícios gerados

## Padronização

O sistema garante que as folhas de dados e listas sejam geradas a partir de modelos controlados, reduzindo variações entre documentos.

## Produtividade

A equipe deixa de preencher manualmente campos repetitivos, acelerando a geração de documentos técnicos.

## Redução de erros

A importação e exportação automatizada reduzem erros de digitação, cópia e inconsistência entre sistemas.

## Controle de revisão

As revisões das folhas de dados passam a ser controladas pelo próprio sistema, com avanço automático e registro na CAPA.

## Integração AutoCAD ↔ Excel

O fluxo permite que os dados circulem entre o P&ID parametrizado e o Excel, mantendo os documentos técnicos mais alinhados.

## Reaproveitamento de dados

Uma vez importados e organizados, os dados podem ser usados para abas individuais, listas, folhas de dados e exportação de retorno.

---

## 25. Pontos de atenção

### Macros precisam estar habilitadas

Sem macros, os botões e automações não funcionarão corretamente.

### Acesso ao VBProject deve estar liberado

A configuração de confiança no acesso ao modelo de objeto VBA é necessária para o funcionamento completo das rotinas.

### Os caminhos precisam estar corretos

Se as pastas de folhas de dados ou templates forem movidas, a configuração deve ser refeita.

### Templates devem seguir o padrão esperado

Alterações estruturais nos templates podem exigir ajustes nas macros.

### Parametrização do AutoCAD deve ser consistente

O sistema depende da qualidade dos atributos exportados do P&ID.

### Revisões devem ser abertas pelo botão correto

Para que o avanço de revisão seja registrado corretamente, folhas existentes devem ser abertas pela função **Editar FD**.

---

## 26. Nome sugerido para o projeto

Nome mais claro para documentação interna:

```txt
Sistema de Gerenciamento de Dados de Instrumentos — AutoCAD para Excel
```

Outras opções possíveis:

- **Gerenciamento de Dados de Instrumentos**
- **Automação de Folhas de Dados**
- **Pipeline P&ID → Excel → Folhas de Dados**
- **Sistema de Integração AutoCAD e Excel para Instrumentação**

---

## 27. Resumo executivo

O **Sistema de Gerenciamento de Dados** automatiza o fluxo de informações de instrumentos entre AutoCAD e Excel. A partir de um P&ID parametrizado, o usuário exporta atributos para TXT, importa os dados no Excel, organiza os instrumentos em abas ou listas, edita atributos, gera folhas de dados padronizadas e controla revisões automaticamente.

Além disso, o sistema permite exportar os dados tratados para TXT e reimportá-los no AutoCAD, fechando o ciclo de atualização entre documentação técnica e P&ID. Com isso, o projeto reduz retrabalho, melhora a padronização, diminui inconsistências e aumenta a confiabilidade dos documentos entregues.
