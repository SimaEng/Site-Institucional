# Documentação — Juscelino

## 1. Visão geral

O **Juscelino** é um chatbot desenvolvido para facilitar a consulta de documentos enviados ao cliente e também apoiar o time interno da empresa na busca por informações técnicas e administrativas relacionadas aos projetos.

A solução funciona como um assistente de consulta documental: o usuário faz perguntas em linguagem natural e o chatbot responde com base nos arquivos indexados na base de conhecimento. Em vez de procurar manualmente em pastas, planilhas, PDFs, documentos Word ou arquivos técnicos, o usuário pode consultar o Juscelino e obter respostas mais rápidas, organizadas e contextualizadas.

O projeto utiliza uma arquitetura de **RAG** (*Retrieval-Augmented Generation*), combinando automação de ingestão de documentos, banco vetorial, extração de dados tabulares e interface conversacional.

---

## 2. Objetivo do projeto

Criar um chatbot corporativo capaz de consultar documentos enviados ao cliente, permitindo que usuários internos e externos encontrem informações com mais facilidade.

### Objetivos principais

- Facilitar a consulta de documentos técnicos enviados ao cliente.
- Reduzir o tempo gasto procurando informações em pastas e arquivos.
- Permitir perguntas em linguagem natural sobre documentos do projeto.
- Organizar o conhecimento gerado em uma base pesquisável.
- Apoiar clientes e equipes internas na localização de informações importantes.
- Centralizar a consulta de documentos em uma interface simples.

---

## 3. Problema resolvido

Antes do Juscelino, a consulta de informações dependia da busca manual em arquivos, pastas e documentos enviados ao cliente. Esse processo exigia que o usuário soubesse onde procurar, qual arquivo abrir e em qual trecho do documento a informação estava.

Esse modelo gerava alguns problemas:

- Demora para encontrar informações específicas.
- Dependência de pessoas que conhecem a estrutura das pastas.
- Dificuldade para consultar documentos extensos.
- Retrabalho ao responder perguntas recorrentes.
- Risco de ignorar informações presentes em arquivos já enviados.
- Baixa praticidade para clientes que precisam consultar dados rapidamente.

Com o Juscelino, os documentos passam a ser indexados e consultáveis por meio de uma interface conversacional.

---

## 4. Escopo funcional

O Juscelino cobre o fluxo de ingestão, organização, indexação e consulta dos documentos.

### Funcionalidades principais

- Monitorar arquivos adicionados ou atualizados em uma pasta.
- Baixar arquivos de origem automaticamente.
- Identificar o tipo de arquivo recebido.
- Extrair texto de documentos PDF.
- Extrair texto de documentos Word.
- Ler planilhas Excel e CSV.
- Tratar dados tabulares separadamente.
- Gerar metadados dos documentos.
- Atualizar registros antigos quando um documento é reenviado.
- Criar embeddings dos conteúdos textuais.
- Armazenar os documentos em banco vetorial.
- Consultar documentos por similaridade semântica.
- Responder perguntas por meio de um chatbot.
- Manter histórico da conversa para melhorar o contexto das respostas.

---

## 5. Tecnologias utilizadas

| Tecnologia | Uso no projeto |
|---|---|
| **n8n** | Orquestração dos fluxos de ingestão e consulta |
| **Microsoft OneDrive** | Origem dos documentos enviados e gatilho de atualização |
| **Open WebUI** | Interface de chatbot utilizada pelo usuário |
| **PostgreSQL** | Banco para armazenar metadados e dados estruturados |
| **pgvector** | Armazenamento vetorial para busca semântica |
| **Embeddings** | Transformação dos textos em vetores consultáveis |
| **LLM / Chat Model** | Geração das respostas em linguagem natural |
| **LangChain** | Apoio no tratamento e processamento dos documentos |
| **PDF Extract / Word2Text** | Extração de texto de arquivos PDF e Word |
| **Excel / CSV Parser** | Leitura de dados tabulares |

---

## 6. Arquitetura geral

A arquitetura do Juscelino é composta por dois fluxos principais:

```txt
1. Fluxo de ingestão de documentos
2. Fluxo de consulta via chatbot
```

O primeiro fluxo prepara a base de conhecimento. O segundo usa essa base para responder perguntas dos usuários.

```txt
Documento no OneDrive
→ Download do arquivo
→ Identificação do tipo
→ Extração de conteúdo
→ Tratamento do texto ou tabela
→ Geração de metadados
→ Criação de embeddings
→ Armazenamento no banco vetorial
→ Consulta pelo chatbot
→ Resposta ao usuário
```

---

# 7. Fluxo de ingestão de documentos

## 7.1. Detecção de arquivos

O workflow no n8n começa com gatilhos do **Microsoft OneDrive**, responsáveis por identificar arquivos adicionados ou atualizados na pasta monitorada.

Essa etapa permite que a base do Juscelino seja atualizada sempre que novos documentos forem disponibilizados.

### Responsabilidades

- Detectar novos arquivos.
- Identificar arquivos alterados.
- Iniciar automaticamente o processo de indexação.
- Garantir que a base acompanhe os documentos mais recentes.

---

## 7.2. Preparação do arquivo

Após detectar o arquivo, o fluxo percorre os itens encontrados e define o identificador do documento.

Nos prints do workflow aparecem etapas relacionadas a:

- Loop pelos arquivos encontrados.
- Definição do `File ID`.
- Exclusão de registros antigos.
- Inserção de metadados do documento.
- Download do arquivo.

Essa etapa é importante para evitar duplicidade. Quando um arquivo já existente é reenviado ou alterado, o fluxo remove dados antigos associados ao documento e reindexa a versão mais recente.

---

## 7.3. Tratamento por tipo de arquivo

Depois do download, o workflow usa uma etapa de decisão para direcionar o arquivo conforme seu formato.

### Tipos de arquivo tratados

| Tipo de arquivo | Tratamento aplicado |
|---|---|
| **PDF** | Extração textual do documento |
| **Word** | Conversão do conteúdo com Word2Text |
| **Excel** | Leitura das abas e extração de dados estruturados |
| **CSV** | Extração e organização dos dados tabulares |
| **Texto/JSON** | Conversão e tratamento para armazenamento |

Essa separação permite que cada tipo de documento seja processado da forma mais adequada.

---

## 7.4. Extração de textos

Para documentos textuais, como PDFs e arquivos Word, o fluxo extrai o conteúdo e prepara o texto para ser dividido em partes menores.

Esse processo permite que documentos grandes sejam quebrados em trechos pesquisáveis, facilitando a busca semântica posterior.

### Etapas envolvidas

- Extrair o texto do arquivo.
- Limpar e padronizar o conteúdo.
- Dividir o texto em blocos menores.
- Gerar embeddings para cada bloco.
- Enviar os blocos para o banco vetorial.

---

## 7.5. Extração de dados tabulares

Além de documentos textuais, o Juscelino também trata arquivos tabulares, como Excel e CSV.

No workflow, esse caminho inclui etapas como:

- Buscar abas da planilha.
- Baixar o arquivo da planilha.
- Extrair dados do Excel ou CSV.
- Agregar os registros.
- Gerar um resumo da estrutura dos dados.
- Definir o schema.
- Inserir linhas em tabelas do banco.
- Atualizar metadados do documento.

Esse tratamento é importante porque perguntas sobre tabelas exigem uma lógica diferente da busca em texto corrido. Em vez de apenas procurar similaridade em parágrafos, o sistema também precisa conseguir consultar linhas e colunas.

---

## 7.6. Metadados dos documentos

O fluxo cria e atualiza uma tabela de metadados para controlar os documentos indexados.

Os metadados ajudam o chatbot a saber quais arquivos existem, de onde vieram e como eles devem ser consultados.

### Exemplos de metadados possíveis

- Nome do arquivo.
- Identificador do arquivo.
- Tipo de documento.
- Origem no OneDrive.
- Data de atualização.
- Schema de dados, quando for planilha.
- Indicação de documento textual ou tabular.

---

## 7.7. Base vetorial

Depois que o conteúdo é extraído e dividido em blocos, o sistema gera embeddings e armazena os vetores no **Postgres com pgvector**.

Essa base permite que o chatbot encontre trechos relevantes mesmo quando a pergunta do usuário não usa exatamente as mesmas palavras do documento.

Exemplo:

```txt
Pergunta do usuário:
"Quais instrumentos novos aparecem nesse projeto?"

Busca vetorial:
Encontra trechos relacionados a instrumentos, listas, tags, fluxogramas ou documentos técnicos mesmo que o texto original use outra formulação.
```

---

# 8. Fluxo de consulta via chatbot

## 8.1. Interface de conversa

O usuário acessa o Juscelino por uma interface de chatbot no **Open WebUI**.

A interface permite que o usuário faça perguntas em linguagem natural, sem precisar conhecer a estrutura das pastas ou o nome exato dos arquivos.

Exemplos de perguntas possíveis:

```txt
Liste todos os instrumentos enviados.
Quais instrumentos novos aparecem nos documentos?
Quais são os fluxogramas disponíveis?
Me passe informações sobre determinado equipamento.
Quais documentos foram enviados para o cliente?
```

---

## 8.2. Entrada da pergunta

No n8n, o fluxo de conversa é acionado por uma mensagem recebida no chat ou por um webhook.

A pergunta passa por uma etapa de preparação antes de ser enviada ao agente RAG.

### Responsabilidades

- Receber a mensagem do usuário.
- Preparar os campos necessários.
- Enviar a pergunta para o agente.
- Retornar a resposta para a interface.

---

## 8.3. Agente RAG

O agente RAG é responsável por interpretar a pergunta, buscar informações relevantes e montar uma resposta baseada nos documentos disponíveis.

Ele utiliza ferramentas auxiliares para consultar diferentes fontes da base.

### Ferramentas do agente

| Ferramenta | Função |
|---|---|
| **Busca vetorial** | Localiza trechos textuais relevantes nos documentos |
| **Consulta de documentos** | Lista documentos disponíveis na base |
| **Conteúdo de arquivo** | Recupera informações de um documento específico |
| **Consulta de linhas** | Busca dados em tabelas estruturadas |
| **Memória de conversa** | Mantém contexto entre mensagens |

---

## 8.4. Geração da resposta

Após recuperar os trechos e dados relevantes, o modelo de linguagem gera uma resposta em linguagem natural.

A resposta deve ser baseada no conteúdo indexado e ser útil para o usuário final, evitando que ele precise abrir manualmente os arquivos.

Fluxo simplificado:

```txt
Pergunta do usuário
→ Busca nos documentos
→ Recuperação dos trechos relevantes
→ Consulta em tabelas, se necessário
→ Geração da resposta
→ Retorno no chat
```

---

# 9. Organização dos dados

O Juscelino trabalha com duas formas principais de conhecimento:

## 9.1. Conhecimento textual

Usado para documentos como:

- PDFs.
- Word.
- Textos exportados.
- Documentos técnicos com descrições.

Esse conteúdo é dividido em blocos e enviado para a base vetorial.

## 9.2. Conhecimento tabular

Usado para documentos como:

- Planilhas Excel.
- Arquivos CSV.
- Listas de instrumentos.
- Tabelas de documentos.
- Dados estruturados por linha e coluna.

Esse conteúdo é armazenado em tabelas para permitir consultas mais objetivas.

---

# 10. Principais casos de uso

## 10.1. Consulta de documentos enviados

O usuário pode perguntar quais documentos estão disponíveis ou solicitar informações sobre arquivos enviados ao cliente.

## 10.2. Consulta de instrumentos

O chatbot pode apoiar perguntas sobre instrumentos presentes nos documentos, como listagem, identificação e comparação.

## 10.3. Consulta de fluxogramas

O usuário pode perguntar quais fluxogramas estão disponíveis ou buscar informações associadas a eles.

## 10.4. Apoio ao cliente

O cliente pode consultar informações sem precisar navegar manualmente por pastas ou abrir vários arquivos.

## 10.5. Apoio ao time interno

A equipe interna pode usar o Juscelino para responder dúvidas recorrentes, localizar documentos e encontrar informações técnicas com mais rapidez.

---

# 11. Benefícios gerados

## Agilidade

O usuário consegue encontrar informações rapidamente por meio de perguntas diretas.

## Facilidade de acesso

Não é necessário conhecer a estrutura completa das pastas ou abrir vários arquivos manualmente.

## Redução de retrabalho

Perguntas recorrentes podem ser respondidas pelo chatbot com base nos documentos já enviados.

## Padronização da consulta

As informações passam a ser consultadas por uma interface única.

## Melhor experiência para o cliente

O cliente tem uma forma mais simples de acessar informações do projeto.

## Apoio ao time técnico

A equipe interna ganha uma ferramenta para localizar informações em documentos, listas e planilhas com mais eficiência.

---

# 12. Pontos de atenção

## Qualidade dos documentos indexados

A qualidade das respostas depende diretamente da qualidade dos documentos enviados para a base.

Arquivos mal formatados, incompletos ou com texto pouco legível podem prejudicar a recuperação das informações.

## Atualização da base

Sempre que um documento for alterado, ele precisa ser reprocessado para que o chatbot consulte a versão mais recente.

## Controle de acesso

Como o Juscelino pode ser usado por clientes e equipe interna, é importante garantir que cada usuário tenha acesso apenas aos documentos permitidos.

## Respostas baseadas nos documentos

O chatbot deve responder com base no conteúdo indexado. Quando a informação não estiver nos documentos, o ideal é indicar que não foi encontrada na base.

---

# 13. Fluxo resumido

```txt
1. Um documento é adicionado ou atualizado no OneDrive.
2. O n8n detecta o arquivo automaticamente.
3. O fluxo identifica o tipo do arquivo.
4. O arquivo é baixado.
5. O conteúdo é extraído conforme o formato.
6. Textos são divididos em blocos e convertidos em embeddings.
7. Planilhas e CSVs são tratados como dados tabulares.
8. Metadados do documento são salvos.
9. A base vetorial e as tabelas auxiliares são atualizadas.
10. O usuário acessa o Juscelino pelo chat.
11. O usuário faz uma pergunta em linguagem natural.
12. O agente RAG consulta os documentos e tabelas.
13. A resposta é gerada e enviada ao usuário.
```

---

# 14. Resultado esperado

O resultado final é um chatbot corporativo capaz de responder perguntas sobre documentos enviados ao cliente e apoiar a consulta de informações técnicas do projeto.

O Juscelino transforma uma base documental dispersa em uma experiência de consulta simples, centralizada e inteligente.

---

# 15. Resumo executivo

O **Juscelino** é um chatbot baseado em RAG criado para facilitar a consulta de documentos enviados ao cliente e apoiar o time interno da empresa. A solução automatiza a ingestão de arquivos, extrai conteúdos de PDFs, Word, Excel e CSV, organiza os dados em banco vetorial e tabelas estruturadas, e disponibiliza uma interface conversacional para perguntas em linguagem natural.

Com isso, o projeto reduz o tempo gasto em buscas manuais, melhora o acesso à informação, apoia a comunicação com clientes e torna mais eficiente a consulta a documentos técnicos e listas do projeto.
