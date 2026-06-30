# Documentação — Stella

## 1. Visão geral

A **Stella** é um chatbot interno desenvolvido para apoiar o time de **Recursos Humanos** da SIMA na consulta de informações corporativas e documentos internos.

O conceito da solução é semelhante ao Juscelino, mas com um escopo diferente: enquanto o Juscelino é voltado à consulta de documentos enviados ao cliente, a Stella é direcionada ao uso interno, com foco em facilitar o acesso a informações de RH, políticas, comunicados, benefícios, procedimentos, documentos orientativos e demais materiais usados pela empresa.

A solução permite que usuários internos façam perguntas em linguagem natural e recebam respostas baseadas nos documentos previamente indexados. Em vez de procurar manualmente informações em pastas, arquivos PDF, documentos Word, planilhas ou comunicados, o usuário pode consultar a Stella por meio de uma interface de chat.

O projeto utiliza uma arquitetura de **RAG** (*Retrieval-Augmented Generation*), combinando automação de ingestão de documentos, extração de conteúdo, armazenamento em banco vetorial e consulta por chatbot.

---

## 2. Objetivo do projeto

Criar uma assistente interna para centralizar e facilitar a consulta de informações relacionadas ao RH, tornando o acesso ao conhecimento mais rápido, simples e padronizado.

### Objetivos principais

- Facilitar a consulta de documentos e informações internas de RH.
- Reduzir o tempo gasto procurando informações em arquivos e pastas.
- Permitir que colaboradores façam perguntas em linguagem natural.
- Apoiar o RH no atendimento de dúvidas recorrentes.
- Organizar documentos internos em uma base pesquisável.
- Melhorar a padronização das respostas fornecidas aos colaboradores.
- Diminuir retrabalho em consultas manuais sobre políticas, benefícios e procedimentos.

---

## 3. Problema resolvido

Em muitos casos, informações de RH ficam distribuídas em documentos, planilhas, comunicados ou arquivos internos. Quando alguém precisa consultar uma regra, benefício, orientação ou procedimento, é necessário saber onde o arquivo está salvo, abrir o documento correto e procurar manualmente o trecho relevante.

Esse processo gera alguns problemas:

- Demora para encontrar informações específicas.
- Dependência do time de RH para responder dúvidas repetitivas.
- Dificuldade em localizar documentos internos corretos.
- Risco de consultar arquivos antigos ou incompletos.
- Falta de centralização das informações.
- Retrabalho para responder perguntas recorrentes.

Com a Stella, os documentos são indexados em uma base de conhecimento e podem ser consultados diretamente por meio de um chatbot.

---

## 4. Escopo funcional

A Stella cobre o processo de ingestão, organização, indexação e consulta de documentos internos.

### Funcionalidades principais

- Monitorar arquivos adicionados ou atualizados em uma pasta.
- Baixar documentos automaticamente.
- Identificar o tipo de arquivo recebido.
- Extrair texto de documentos PDF.
- Extrair texto de documentos Word.
- Ler planilhas Excel e arquivos CSV.
- Tratar dados tabulares de forma separada.
- Criar metadados dos documentos processados.
- Remover registros antigos quando um documento é atualizado.
- Criar embeddings dos conteúdos extraídos.
- Armazenar os conteúdos em banco vetorial.
- Consultar documentos por similaridade semântica.
- Responder perguntas em linguagem natural pela interface de chat.
- Manter contexto da conversa para melhorar a experiência do usuário.

---

## 5. Público-alvo

A Stella foi pensada para uso interno da empresa, principalmente para apoiar consultas relacionadas ao RH.

### Usuários principais

- Time de Recursos Humanos.
- Colaboradores internos.
- Lideranças que precisam consultar informações de pessoas, políticas ou procedimentos.
- Equipes administrativas que dependem de documentos internos.

---

## 6. Exemplos de uso

A Stella pode ser utilizada para perguntas como:

```txt
Quais benefícios a empresa oferece?
```

```txt
Como funciona o processo de solicitação de férias?
```

```txt
Quais documentos preciso entregar para admissão?
```

```txt
Existe alguma orientação sobre banco de horas?
```

```txt
Me explique a política de reembolso.
```

```txt
Onde encontro informações sobre integração de novos colaboradores?
```

Esses exemplos representam o tipo de consulta que o chatbot pode atender, desde que os documentos correspondentes estejam indexados na base de conhecimento.

---

## 7. Tecnologias utilizadas

| Tecnologia | Uso no projeto |
|---|---|
| **n8n** | Orquestração dos fluxos de ingestão e consulta |
| **Microsoft OneDrive** | Origem dos documentos internos e gatilho de atualização |
| **Open WebUI** | Interface de chatbot utilizada pelos usuários |
| **PostgreSQL** | Banco para armazenar metadados e dados estruturados |
| **pgvector** | Armazenamento vetorial para busca semântica |
| **Embeddings** | Transformação dos textos em vetores consultáveis |
| **LLM / Chat Model** | Geração das respostas em linguagem natural |
| **LangChain** | Apoio no processamento dos documentos e integração RAG |
| **PDF Extract / Word2Text** | Extração de texto de arquivos PDF e Word |
| **Excel / CSV Parser** | Leitura de dados tabulares |

---

## 8. Arquitetura geral

A arquitetura da Stella é composta por dois fluxos principais:

```txt
1. Fluxo de ingestão de documentos
2. Fluxo de consulta via chatbot
```

O primeiro fluxo prepara e atualiza a base de conhecimento. O segundo usa essa base para responder perguntas dos usuários.

```txt
Documento interno no OneDrive
→ Detecção do arquivo
→ Download
→ Identificação do tipo de arquivo
→ Extração de conteúdo
→ Tratamento do texto ou tabela
→ Criação de metadados
→ Geração de embeddings
→ Armazenamento no banco vetorial
→ Consulta pelo chatbot
→ Resposta ao usuário
```

---

# 9. Fluxo de ingestão de documentos

## 9.1. Detecção de arquivos

O workflow no n8n utiliza gatilhos do **Microsoft OneDrive** para detectar arquivos adicionados ou atualizados em uma pasta monitorada.

Essa etapa permite que a base da Stella seja atualizada conforme novos documentos internos são adicionados ou documentos existentes são revisados.

### Responsabilidades

- Detectar novos arquivos.
- Identificar arquivos alterados.
- Iniciar automaticamente o processo de indexação.
- Manter a base do chatbot alinhada com os documentos mais recentes.

---

## 9.2. Preparação do documento

Após detectar o arquivo, o workflow percorre os itens encontrados e define o identificador do documento.

Nos prints do workflow aparecem etapas como:

- `Loop Over Items`
- `Set File ID`
- `Delete Old Doc Records`
- `Delete Old Data Records`
- `Insert Document Metadata`
- `Download a file`

Essa etapa evita duplicidade e garante que, quando um documento for atualizado, os registros antigos sejam removidos antes da nova versão ser indexada.

---

## 9.3. Tratamento por tipo de arquivo

Depois do download, o fluxo usa uma etapa de decisão para direcionar o documento conforme seu formato.

### Tipos tratados

| Tipo de arquivo | Tratamento aplicado |
|---|---|
| **PDF** | Extração do conteúdo textual |
| **Word** | Conversão do documento com Word2Text |
| **Excel** | Leitura das abas e organização dos dados estruturados |
| **CSV** | Extração e estruturação dos dados tabulares |
| **Texto/JSON** | Conversão e padronização para armazenamento |

Essa separação permite que cada arquivo seja processado com a lógica mais adequada.

---

## 9.4. Extração de documentos textuais

Para arquivos como PDF e Word, o fluxo extrai o texto e prepara o conteúdo para indexação.

Etapas típicas:

```txt
PDF/Word
→ Extração textual
→ Limpeza e padronização
→ Divisão em blocos menores
→ Geração de embeddings
→ Armazenamento no banco vetorial
```

Esse processo permite que documentos grandes sejam consultados por partes, facilitando a localização de trechos relevantes durante as respostas.

---

## 9.5. Extração de dados tabulares

A Stella também trata arquivos em formato de tabela, como planilhas Excel e CSV.

No workflow, esse caminho inclui etapas como:

- `Get sheets`
- `Download a file`
- `Extract from Excel`
- `Extract from CSV`
- `Aggregate`
- `Summarize`
- `Set Schema`
- `Insert Table Rows`
- `Update Schema for Document Metadata`

Esse tratamento é importante porque documentos de RH podem conter tabelas, listas, controles, calendários ou dados organizados em linhas e colunas.

---

## 9.6. Metadados dos documentos

O fluxo cria e atualiza metadados para controlar os documentos indexados.

Os metadados ajudam a Stella a identificar os arquivos disponíveis, sua origem e como cada um deve ser consultado.

### Exemplos de metadados possíveis

- Nome do arquivo.
- Identificador do arquivo.
- Tipo do documento.
- Origem no OneDrive.
- Data de atualização.
- Estrutura da tabela, quando for planilha.
- Indicação de documento textual ou tabular.

---

## 9.7. Base vetorial

Depois que o conteúdo textual é extraído, o sistema gera embeddings e armazena os vetores no **PostgreSQL com pgvector**.

A base vetorial permite que a Stella encontre informações relevantes mesmo quando o usuário não utiliza exatamente as mesmas palavras do documento original.

Exemplo:

```txt
Pergunta do usuário: "Como peço férias?"
Documento: "Procedimento para solicitação de férias"
```

Mesmo sem igualdade exata entre os termos, a busca semântica permite aproximar a pergunta do conteúdo correto.

---

# 10. Fluxo de consulta pelo chatbot

## 10.1. Interface de conversa

A Stella é acessada por meio do **Open WebUI**, onde o usuário pode enviar perguntas em linguagem natural.

A interface permite uma experiência simples, semelhante a um chat comum, reduzindo a barreira de uso para colaboradores que não têm familiaridade técnica.

---

## 10.2. Recebimento da pergunta

No n8n, o fluxo de consulta começa com uma entrada via **Webhook**, que recebe a mensagem enviada pelo usuário.

Nos prints aparecem componentes como:

- `When chat message received`
- `Webhook`
- `RAG AI Agent`
- `Respond to Webhook`

Essa estrutura conecta a interface do chatbot ao workflow de consulta.

---

## 10.3. Agente RAG

O agente RAG é responsável por interpretar a pergunta, buscar informações relevantes e gerar uma resposta final.

No workflow aparecem ferramentas relacionadas a:

- `List Documents`
- `Get File Contents`
- `Query Document Rows`
- `Postgres Chat Memory`
- `OpenAI Chat Model`

Essas ferramentas permitem que a Stella consulte tanto documentos textuais quanto dados tabulares armazenados no banco.

---

## 10.4. Busca semântica

Quando a pergunta envolve informações textuais, o agente consulta o banco vetorial para encontrar trechos semelhantes à dúvida do usuário.

O caminho geral é:

```txt
Pergunta do usuário
→ Embedding da pergunta
→ Busca no pgvector
→ Recuperação dos trechos relevantes
→ Resposta com base no contexto encontrado
```

Essa abordagem evita que o chatbot dependa apenas de palavras-chave exatas.

---

## 10.5. Consulta a dados estruturados

Quando a pergunta envolve dados organizados em planilhas, o agente pode consultar linhas e tabelas associadas aos documentos.

Isso permite responder perguntas que dependem de dados estruturados, como listas, valores, categorias, datas ou registros internos.

---

# 11. Funcionamento resumido

```txt
1. Um documento interno é adicionado ou atualizado no OneDrive.
2. O n8n detecta o arquivo pelo gatilho configurado.
3. O fluxo baixa o documento.
4. O tipo do arquivo é identificado.
5. O conteúdo é extraído conforme o formato.
6. Dados antigos do mesmo documento são removidos, quando necessário.
7. Novos metadados são criados.
8. O texto é dividido em blocos e convertido em embeddings.
9. O conteúdo é armazenado no banco vetorial.
10. O usuário acessa a Stella pelo Open WebUI.
11. A pergunta é enviada ao agente RAG via Webhook.
12. O agente busca os documentos mais relevantes.
13. A resposta é gerada com base no conteúdo recuperado.
14. O usuário recebe uma resposta em linguagem natural.
```

---

# 12. Regras de negócio

## 12.1. A resposta deve se basear nos documentos indexados

A Stella deve priorizar respostas fundamentadas nos documentos disponíveis na base de conhecimento.

Quando a informação não estiver presente ou não puder ser identificada com segurança, a resposta deve indicar que não há base suficiente para afirmar.

---

## 12.2. Documentos atualizados substituem versões antigas

Quando um arquivo é alterado ou reenviado, o fluxo remove registros antigos relacionados ao documento antes de inserir a nova versão.

Isso reduz o risco de respostas baseadas em conteúdo desatualizado.

---

## 12.3. Documentos textuais e tabulares têm tratamentos diferentes

PDFs e arquivos Word são tratados como conteúdo textual. Planilhas e CSVs são tratados como dados tabulares.

Essa separação melhora a precisão da consulta, porque cada tipo de arquivo exige uma forma diferente de processamento.

---

## 12.4. A base deve respeitar o escopo de RH

A Stella foi criada para consultas internas relacionadas ao RH e informações corporativas desse contexto.

Perguntas fora desse escopo devem ser tratadas com cautela, principalmente quando não houver documentos indexados para sustentar a resposta.

---

# 13. Benefícios gerados

## Agilidade

Colaboradores e equipe de RH conseguem encontrar informações sem abrir manualmente documentos e pastas.

## Redução de retrabalho

Dúvidas recorrentes podem ser respondidas pelo chatbot, liberando o time de RH para atividades mais estratégicas.

## Padronização

As respostas passam a seguir a base documental indexada, reduzindo variações entre orientações dadas por diferentes pessoas.

## Centralização

Documentos internos ficam consultáveis em uma única interface conversacional.

## Facilidade de uso

O usuário não precisa conhecer a estrutura das pastas nem o nome exato dos arquivos para fazer uma consulta.

## Atualização contínua

A base pode ser atualizada automaticamente conforme novos documentos são adicionados ou revisados.

---

# 14. Pontos de atenção

## Qualidade dos documentos

A precisão da Stella depende da qualidade dos documentos indexados. Arquivos desatualizados, incompletos ou ambíguos podem gerar respostas menos confiáveis.

## Controle de acesso

Como o escopo envolve informações internas de RH, é importante garantir que apenas pessoas autorizadas tenham acesso à base e ao chatbot.

## Sensibilidade das informações

Documentos de RH podem conter informações sensíveis. O fluxo deve evitar exposição desnecessária de dados pessoais ou informações restritas.

## Atualização da base

Sempre que uma política, benefício ou procedimento mudar, o documento correspondente precisa ser atualizado na pasta monitorada para que a Stella reflita a versão mais recente.

## Validação das respostas

Para decisões formais ou situações específicas, a Stella deve ser usada como apoio à consulta, e não como substituta final da validação pelo RH quando houver dúvida.

---

# 15. Sugestões de melhoria futura

## 15.1. Separar bases por categoria

Criar categorias específicas de documentos, como:

- Benefícios.
- Admissão.
- Férias.
- Reembolso.
- Políticas internas.
- Integração de colaboradores.
- Treinamentos.

Isso pode melhorar a organização e facilitar filtros futuros.

---

## 15.2. Registrar fontes usadas na resposta

Incluir na resposta o nome do documento ou trecho utilizado como base, facilitando conferência e auditoria.

---

## 15.3. Criar rotina de revisão documental

Definir uma periodicidade para revisar os documentos indexados, garantindo que a base continue atualizada.

---

## 15.4. Criar níveis de acesso

Separar o que pode ser consultado por todos os colaboradores e o que deve ficar restrito ao time de RH ou lideranças.

---

## 15.5. Criar indicadores de uso

Medir perguntas mais frequentes, temas mais consultados e documentos mais usados pode ajudar o RH a identificar lacunas de comunicação interna.

---

# 16. Nome sugerido para o projeto

Nome principal:

```txt
Stella — Assistente Interna de RH
```

Outras opções:

- **Stella RH**
- **Assistente de RH SIMA**
- **Chatbot Interno de RH**
- **Base Inteligente de RH**
- **Consulta Inteligente de Documentos de RH**

---

# 17. Resumo executivo

A **Stella** é um chatbot interno criado para facilitar a consulta de documentos e informações relacionadas ao RH da SIMA.

A solução utiliza uma arquitetura RAG, com ingestão automática de documentos via n8n, extração de conteúdo de arquivos PDF, Word, Excel e CSV, criação de embeddings, armazenamento em banco vetorial e consulta por meio de uma interface conversacional no Open WebUI.

Com isso, colaboradores e equipe de RH conseguem consultar informações internas com mais rapidez, reduzindo buscas manuais, dúvidas repetitivas e inconsistências na comunicação. A Stella centraliza o conhecimento documental em uma base pesquisável e torna o acesso às informações mais simples, ágil e padronizado.
