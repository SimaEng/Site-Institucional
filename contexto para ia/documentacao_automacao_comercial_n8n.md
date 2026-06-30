# Documentação — Automação de Fluxo de Planilhas para Excel

## 1. Visão geral

Este projeto consiste em uma automação desenvolvida no **n8n** para o time Comercial, com o objetivo de eliminar o preenchimento manual de uma planilha final a partir de informações espalhadas em diferentes arquivos e abas.

Antes da automação, o processo dependia da leitura manual de várias fontes, cópia de dados repetitivos e preenchimento linha por linha em uma planilha consolidada. Esse fluxo era lento, repetitivo e sujeito a erros humanos, como inconsistência de valores, preenchimento incompleto, troca de campos ou uso de informações desatualizadas.

Com a automação, basta inserir os arquivos necessários na pasta correta do OneDrive/SharePoint. A partir disso, o workflow identifica os documentos, extrai as informações relevantes, trata os dados e atualiza automaticamente a planilha final no Excel.

---

## 2. Objetivo do projeto

Automatizar o processo de coleta, tratamento e preenchimento de dados comerciais em uma planilha final, reduzindo trabalho manual e aumentando a confiabilidade das informações.

### Objetivos principais

- Reduzir o tempo gasto pelo Comercial com preenchimento manual.
- Evitar inconsistências entre arquivos de origem e planilha final.
- Padronizar a forma como os dados são extraídos e registrados.
- Permitir que o fluxo seja iniciado automaticamente ao adicionar ou editar arquivos em uma pasta.
- Consolidar dados vindos de diferentes fontes, como planilhas, propostas em PDF/DOCX e abas específicas de Excel.

---

## 3. Problema resolvido

O processo anterior exigia que o usuário:

1. Abrisse diferentes arquivos manualmente.
2. Identificasse quais abas eram relevantes.
3. Procurasse valores específicos em cada fonte.
4. Copiasse dados para outra planilha.
5. Conferisse manualmente se os campos estavam corretos.
6. Repetisse o processo a cada nova proposta ou atualização.

Esse modelo gerava três problemas principais:

- **Baixa produtividade:** muito tempo era gasto em uma tarefa operacional.
- **Risco de erro humano:** dados podiam ser copiados incorretamente ou esquecidos.
- **Falta de padronização:** cada preenchimento podia variar conforme quem executava a tarefa.

A automação resolve isso criando um fluxo padronizado e repetível.

---

## 4. Escopo da automação

A automação cobre o fluxo desde a entrada dos arquivos na pasta até a atualização da planilha final.

### Dentro do escopo

- Monitorar arquivos criados ou alterados em uma pasta.
- Filtrar arquivos relevantes.
- Baixar arquivos de proposta.
- Extrair informações do nome do arquivo.
- Identificar tipo de proposta/cotação.
- Ler planilhas de apoio.
- Buscar abas específicas.
- Extrair valores de abas como:
  - Histograma
  - Despesas
  - Estudos
  - Pacotes
  - Resumo
- Extrair informações de propostas em PDF ou Word.
- Sanitizar texto antes de enviar para IA.
- Usar LLM para interpretar informações textuais da proposta.
- Agrupar informações extraídas.
- Localizar ou substituir registro existente.
- Inserir ou atualizar dados na planilha final.

### Fora do escopo atual

- Validação humana obrigatória antes de gravar na planilha.
- Interface própria para acompanhamento do fluxo.
- Auditoria completa de alterações campo a campo.
- Tratamento avançado de todos os formatos possíveis de documentos fora do padrão.

---

## 5. Tecnologias utilizadas

| Tecnologia | Uso no projeto |
|---|---|
| **n8n** | Orquestração do workflow |
| **Microsoft OneDrive / SharePoint** | Origem dos arquivos e gatilho do fluxo |
| **Microsoft Excel 365** | Consulta e atualização das planilhas |
| **JavaScript / Code Nodes** | Tratamento, filtros, agregações e padronização dos dados |
| **PDF Extract / Word2Text** | Extração textual de propostas |
| **LLM / Groq Chat Model** | Interpretação de informações não estruturadas |
| **Manual Trigger / Save Nodes** | Testes e armazenamento intermediário de informações |

---

## 6. Funcionamento geral

O fluxo funciona a partir de arquivos adicionados ou editados em uma pasta monitorada.

Quando um novo arquivo é detectado, o workflow executa uma sequência de etapas:

```txt
Arquivo criado/editado
→ Filtrar arquivos relevantes
→ Baixar proposta
→ Extrair dados do nome do arquivo
→ Buscar arquivos e planilhas relacionados
→ Extrair valores das abas necessárias
→ Extrair dados do documento da proposta
→ Tratar e consolidar informações
→ Atualizar planilha final
```

---

## 7. Gatilhos do workflow

Pelos prints, o projeto possui pelo menos dois cenários de entrada.

### 7.1. Quando um arquivo é criado

Este fluxo é acionado quando um novo arquivo é colado/adicionado na pasta correta.

Uso esperado:

- Nova proposta adicionada.
- Novo pacote de arquivos recebido.
- Novo arquivo comercial incluído para processamento.

### 7.2. Quando um arquivo é editado

Também existe um fluxo para quando um arquivo já existente é atualizado.

Uso esperado:

- Proposta revisada.
- Planilha complementar alterada.
- Arquivo reenviado com correções.
- Atualização de valores ou informações comerciais.

---

## 8. Estrutura macro do workflow

O workflow pode ser dividido em cinco grandes blocos:

```txt
1. Entrada e filtragem dos arquivos
2. Extração de informações pelo nome do arquivo
3. Extração de valores das planilhas
4. Extração de informações do documento da proposta
5. Consolidação e atualização da planilha final
```

---

## 9. Etapas detalhadas

### 9.1. Entrada e filtragem dos arquivos

O fluxo começa com um nó do **Microsoft OneDrive Trigger**, responsável por detectar a criação ou edição de arquivos.

Depois disso, o workflow passa por filtros para garantir que apenas os arquivos relevantes continuem no processo.

Nos prints aparecem nós como:

- `Microsoft OneDrive Trigger`
- `Filtrar Excel`
- `Filtrar arquivo de proposta`
- `Baixa os arquivos de proposta`

Essa etapa evita que arquivos irrelevantes sejam processados.

#### Responsabilidade desta etapa

- Detectar o evento inicial.
- Verificar se o arquivo pertence ao fluxo comercial.
- Separar arquivos Excel de arquivos de proposta.
- Fazer download do arquivo correto para leitura posterior.

---

### 9.2. Extração de informações pelo nome do arquivo

Após baixar o arquivo, o fluxo extrai informações a partir do próprio nome do documento.

Nos prints aparecem nós como:

- `Obter informações através do nome do...`
- `Ver o tipo de proposta`
- `Get a file`
- `Obter data de criação da pasta`
- `Guarda as informações obtidas pelo nome do...`

Essa etapa aproveita o padrão de nomenclatura dos arquivos para identificar dados estruturados sem precisar abrir todo o conteúdo do documento.

#### Exemplos de informações extraídas

- Número da proposta
- Cliente
- Revisão
- Tipo de proposta
- Escopo
- Fase
- Local
- Data de criação da pasta ou arquivo

#### Importância

Essa etapa cria uma base inicial de dados confiável, porque parte de uma estrutura padronizada de nome de arquivo.

---

### 9.3. Busca das planilhas necessárias

Depois de identificar a proposta e a pasta correspondente, o fluxo consulta os arquivos e abas necessários no Excel.

Nos prints aparecem nós como:

- `Get sheets`
- `Pegar só as folhas necessárias`
- `Busca todas as folhas`
- `Switch`
- `Aggregate`

O fluxo busca as abas relevantes e separa o processamento conforme o tipo de informação encontrada.

As abas visíveis no workflow são:

- **Histograma**
- **Despesas**
- **Estudos**
- **Pacotes**
- **Resumo**

Cada uma dessas abas segue um caminho próprio dentro do workflow.

---

### 9.4. Extração de valores das planilhas

O bloco **Extrair valores das planilhas** é responsável por abrir as abas específicas e coletar os valores necessários.

Pelos prints, o fluxo possui ramificações separadas para cada aba.

#### Histograma

Nós visíveis:

- `Baixar a folha histograma`
- `Extract from File`
- `Somatório`
- `Total HH`
- `Guarda os somatórios`

Responsabilidade:

- Ler a aba de histograma.
- Extrair os dados da planilha.
- Calcular somatórios.
- Consolidar o total de HH.

#### Despesas

Nós visíveis:

- `Baixar folha despesas`
- `Extract from File`
- `Pegar valor de despesas`

Responsabilidade:

- Ler a aba de despesas.
- Extrair os valores financeiros correspondentes.
- Retornar o valor tratado para consolidação.

#### Estudos

Nós visíveis:

- `Baixar folha estudo`
- `Extract from File`
- `Pegar valor dos estudos`

Responsabilidade:

- Ler a aba de estudos.
- Identificar os valores relacionados a estudos.
- Preparar o valor para compor a planilha final.

#### Pacotes

Nós visíveis:

- `Baixar folha pacotes`
- `Extract from File`
- `Pegar valor dos pacotes`

Responsabilidade:

- Ler a aba de pacotes.
- Extrair valores associados a pacotes comerciais.
- Encaminhar os dados para consolidação.

#### Resumo

Nós visíveis:

- `Baixar folha resumo`
- `Extract from File`
- `Pegar valores do resumo`

Responsabilidade:

- Ler a aba de resumo.
- Capturar informações consolidadas da proposta.
- Servir como fonte complementar para a planilha final.

---

### 9.5. Extração das informações do arquivo de proposta

Além das planilhas, o fluxo também lê o arquivo de proposta em si.

Nos prints aparece um bloco chamado:

```txt
Extrair informações a partir do arquivo de proposta
```

Dentro desse bloco aparecem nós como:

- `Get items in a folder`
- `Filtrar Word ou PDF`
- `Há pdf?`
- `Filtrar os arquivos corretos`
- `Loop Over Items`
- `Baixar proposta`
- `É PDF?`
- `Extract from File`
- `Word2Text`
- `Sanitização`
- `Basic LLM Chain`
- `Groq Chat Model`
- `Separar informações da proposta`
- `Obter País`

Essa etapa trata documentos semiestruturados ou não estruturados, como PDF e Word.

#### Funcionamento

1. O fluxo acessa a pasta relacionada à proposta.
2. Procura arquivos de proposta em formato PDF ou Word.
3. Verifica qual tipo de arquivo foi encontrado.
4. Faz download do arquivo.
5. Extrai o texto.
6. Sanitiza o conteúdo.
7. Envia o texto tratado para uma LLM.
8. Recebe as informações estruturadas.
9. Separa os campos úteis para a planilha final.

#### Por que usar IA nessa etapa

Algumas informações da proposta podem estar em texto corrido, tabelas variadas ou formatos que mudam entre documentos. Por isso, a LLM é usada como fallback ou camada de interpretação para transformar texto não estruturado em dados organizados.

---

## 10. Sanitização dos dados

Antes de enviar o conteúdo textual da proposta para a IA, o fluxo realiza uma etapa de sanitização.

Objetivo:

- Reduzir exposição desnecessária de dados sensíveis.
- Limpar ruídos do documento.
- Manter apenas as informações necessárias para extração.
- Melhorar a qualidade da resposta da LLM.

Exemplos de dados que podem ser tratados:

- URLs
- Telefones
- Documentos
- E-mails irrelevantes
- Trechos repetitivos
- Conteúdo fora do escopo da proposta

Essa etapa é importante porque protege o fluxo contra vazamento desnecessário de informações e também melhora a precisão da IA.

---

## 11. Consolidação dos dados

Depois que os valores das planilhas e as informações da proposta são extraídos, o fluxo junta tudo em uma estrutura única.

Nos prints aparecem nós como:

- `Merge`
- `Merge2`
- `Agrupar informações da nova proposta`
- `Extrai os valores da última proposta...`
- `Agrupar informações da nova proposta1`

Essa etapa combina dados vindos de fontes diferentes:

```txt
Dados do nome do arquivo
+ Dados das abas Excel
+ Dados extraídos da proposta
+ Dados calculados
= Registro final consolidado
```

---

## 12. Atualização da planilha final

No final do workflow, os dados consolidados são enviados para o Excel.

Nos prints aparecem nós como:

- `Get rows`
- `Localizar e substituir o registro`
- `Append or update a sheet`
- `Append rows to table`

Essa etapa verifica se o registro já existe e decide se deve atualizar uma linha existente ou inserir uma nova.

### Comportamento esperado

| Situação | Ação |
|---|---|
| Registro ainda não existe | Inserir nova linha |
| Registro já existe | Atualizar linha existente |
| Dados foram alterados | Substituir valores antigos |
| Arquivo foi reenviado | Reprocessar e atualizar |

Isso evita duplicidade e mantém a planilha final sincronizada com os arquivos de origem.

---

## 13. Fluxo resumido em etapas

```txt
1. Comercial cola ou atualiza arquivos na pasta correta.
2. OneDrive Trigger detecta a criação/edição.
3. O workflow filtra os arquivos relevantes.
4. O arquivo da proposta é baixado.
5. Informações iniciais são extraídas pelo nome do arquivo.
6. O workflow localiza arquivos e abas Excel relacionados.
7. As abas necessárias são baixadas e lidas.
8. Valores de Histograma, Despesas, Estudos, Pacotes e Resumo são extraídos.
9. O documento da proposta é localizado em PDF ou Word.
10. O texto da proposta é extraído.
11. O conteúdo é sanitizado.
12. A IA interpreta e estrutura as informações textuais.
13. Os dados das planilhas e da proposta são agrupados.
14. A planilha final é consultada.
15. O registro é localizado.
16. A linha é criada ou atualizada automaticamente.
```

---

## 14. Entradas do fluxo

### Arquivos de entrada

A automação depende de arquivos inseridos na pasta correta.

Tipos esperados:

- Planilhas Excel
- Arquivos PDF
- Arquivos Word
- Pastas de proposta
- Arquivos comerciais complementares

### Fontes de informação

| Fonte | Informação extraída |
|---|---|
| Nome do arquivo | Cliente, proposta, revisão, tipo, escopo ou dados estruturais |
| Aba Histograma | Total de HH / somatórios |
| Aba Despesas | Valores de despesas |
| Aba Estudos | Valores de estudos |
| Aba Pacotes | Valores de pacotes |
| Aba Resumo | Valores consolidados |
| PDF/DOCX da proposta | Dados textuais da proposta |
| Pasta/metadata | Data de criação e organização do arquivo |

---

## 15. Saída do fluxo

A saída principal é a **planilha final preenchida automaticamente**.

Essa planilha recebe os dados consolidados da proposta, evitando que o time Comercial precise transportar manualmente as informações.

### Resultado esperado

Uma linha nova ou atualizada contendo os dados da proposta processada.

A automação deve garantir que a planilha final fique preenchida com os dados mais recentes disponíveis nos arquivos da pasta.

---

## 16. Regras de negócio

### 16.1. Processar apenas arquivos relevantes

O fluxo não deve processar qualquer arquivo adicionado à pasta. Ele filtra os arquivos para encontrar apenas os documentos esperados.

### 16.2. Usar abas específicas

A automação considera apenas as folhas necessárias para o processo comercial:

- Histograma
- Despesas
- Estudos
- Pacotes
- Resumo

### 16.3. Separar o tratamento por tipo de aba

Cada aba possui uma lógica própria de extração.

Por exemplo:

- Histograma exige somatório de HH.
- Despesas exige extração de valor financeiro.
- Estudos e Pacotes exigem identificação de valores específicos.
- Resumo serve como base consolidada.

### 16.4. Aceitar proposta em PDF ou Word

O workflow está preparado para lidar com documentos em PDF e Word.

Se for PDF, usa extração direta do arquivo.

Se for Word, converte o conteúdo textual com `Word2Text`.

### 16.5. Atualizar registro existente quando aplicável

A automação não deve simplesmente adicionar linhas duplicadas. Ela busca registros existentes e decide entre inserir ou atualizar.

---

## 17. Benefícios gerados

### Produtividade

O time Comercial deixa de gastar tempo transportando dados manualmente entre arquivos.

### Padronização

Todos os registros seguem a mesma lógica de preenchimento.

### Redução de erros

A automação reduz falhas como:

- Campo preenchido errado.
- Valor copiado da aba errada.
- Esquecimento de informação.
- Duplicidade de registros.
- Inconsistência entre proposta e planilha final.

### Rastreabilidade

Como o fluxo é executado pelo n8n, é possível consultar execuções, identificar falhas e revisar o ponto onde o processo parou.

### Escalabilidade

O fluxo pode ser expandido para novas abas, novos tipos de documento e novas regras comerciais.

---

## 18. Pontos de atenção

### Dependência do padrão de arquivos

A automação depende de uma organização mínima das pastas e nomes de arquivos. Se os arquivos forem colados fora do padrão, o fluxo pode não localizar ou interpretar corretamente os dados.

### Mudanças na estrutura das planilhas

Se as abas mudarem de nome, posição ou estrutura interna, os nós de extração podem precisar de ajuste.

### Qualidade dos PDFs e DOCX

Propostas muito diferentes do padrão podem prejudicar a extração textual ou a interpretação pela IA.

### Dependência da IA

A etapa com LLM é útil para interpretar texto não estruturado, mas precisa de validação e prompts bem definidos para evitar respostas inconsistentes.

### Tratamento de duplicidade

A etapa de localizar/substituir registro precisa ter uma chave confiável, como número da proposta, cliente, revisão ou combinação desses campos.

---

## 19. Sugestões de melhoria futura

### 19.1. Criar log estruturado de execução

Registrar em uma aba ou banco auxiliar:

- Nome do arquivo processado
- Data/hora da execução
- Status
- Erro, se houver
- Linha atualizada
- Dados principais extraídos

### 19.2. Criar validação antes do append/update

Antes de atualizar a planilha final, validar campos obrigatórios como:

- Proposta
- Cliente
- Revisão
- Valor total
- Tipo de proposta
- Data

### 19.3. Criar coluna de status na planilha final

Exemplo:

| Status | Significado |
|---|---|
| `Processado` | Dados inseridos com sucesso |
| `Atualizado` | Registro existente foi substituído |
| `Pendente` | Faltou alguma informação |
| `Erro` | Fluxo falhou em alguma etapa |

### 19.4. Separar regras em Code Nodes nomeados

Manter os Code Nodes com responsabilidades bem específicas:

- Extrair dados do nome
- Padronizar nomes de abas
- Calcular somatórios
- Montar objeto final
- Validar campos obrigatórios
- Localizar chave única

### 19.5. Criar documentação de padrão de pasta

Definir claramente:

```txt
Onde colar os arquivos
Como nomear os arquivos
Quais arquivos são obrigatórios
Quais abas precisam existir
Quais formatos são aceitos
O que fazer em caso de erro
```

---

## 20. Nome sugerido para o projeto

Algumas opções de nome para documentar internamente:

- **Automação Comercial de Propostas**
- **Fluxo Comercial Automatizado**
- **Consolidador Comercial Excel**
- **Automação de Preenchimento da Planilha Comercial**
- **Pipeline Comercial OneDrive → Excel**

Nome mais claro:

```txt
Automação Comercial de Propostas — OneDrive para Excel
```

---

## 21. Resumo executivo

A automação criada no n8n transforma um processo manual e repetitivo do time Comercial em um fluxo automático, acionado pela criação ou edição de arquivos em uma pasta do OneDrive/SharePoint.

O workflow identifica os arquivos necessários, extrai dados do nome da proposta, lê abas específicas de planilhas, coleta valores comerciais, interpreta documentos PDF ou Word com apoio de IA e atualiza automaticamente a planilha final no Excel.

Com isso, o projeto reduz retrabalho, aumenta produtividade, evita inconsistências e cria uma base mais confiável para o controle comercial.
