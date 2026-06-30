# Documentação — Consolidação Comercial e Dashboard de Eficiência de HH

## 1. Visão geral

Este projeto consiste em uma automação desenvolvida no **n8n** para consolidar dados comerciais, financeiros e operacionais da empresa, gerando uma base estruturada para análise em dashboard.

Na empresa, o time Comercial monta propostas para venda dos serviços de engenharia. Após a contratação, os serviços passam a gerar controle de faturas, valores pagos, tarifas por cargo, horas executadas e documentos técnicos entregues ao cliente. Essas informações, quando analisadas em conjunto, permitem comparar o que foi planejado comercialmente com o que foi efetivamente consumido na execução.

A automação foi criada para extrair essas informações de diferentes fontes, tratar os dados, calcular indicadores de HH e exportar uma base consolidada para o dashboard. Com isso, a empresa passa a visualizar de forma mais clara a eficiência das propostas, os desvios de planejamento e a relação entre horas executadas e documentos produzidos.

---

## 2. Objetivo do projeto

Automatizar a consolidação entre propostas comerciais, faturas, tarifas por cargo e documentos produzidos, permitindo acompanhar a relação entre o que foi vendido e o que foi efetivamente gasto na execução dos serviços.

### Objetivos principais

- Comparar **HH estimado** na proposta com **HH executado** calculado a partir dos valores pagos.
- Identificar estouros de horas, horas não previstas e horas superestimadas.
- Relacionar dados comerciais com dados financeiros e documentos técnicos gerados.
- Consolidar informações por OS, cliente, cidade, disciplina, cargo/função e tipo de documento.
- Gerar bases tratadas para alimentar o dashboard.
- Apoiar decisões comerciais e operacionais com dados mais confiáveis.

---

## 3. Contexto do processo

O processo consolidado envolve três grupos principais de informação.

### 3.1. Dados comerciais

São os dados das propostas montadas pelo time Comercial. Neles estão as horas vendidas ou estimadas, normalmente organizadas por OS, disciplina e cargo/função.

Essas informações representam o planejamento comercial da execução.

### 3.2. Dados de faturas, valores pagos e tarifas

A fonte de faturamento contém os valores pagos relacionados à execução dos serviços. Para transformar esses valores em horas executadas, a automação cruza o valor pago com a tarifa por hora de cada cargo.

A regra central é:

```txt
HH Executado = Valor Pago / Valor Hora do Cargo
```

Com isso, o fluxo consegue converter valores financeiros em esforço operacional estimado em horas.

### 3.3. Documentos técnicos gerados

Os documentos técnicos representam o produto entregue pela empresa. A automação também consolida a lista desses documentos, permitindo medir volume de entregáveis por OS, disciplina e tipo de documento.

Essa informação permite criar análises como:

```txt
HH por Documento = HH Executado / Quantidade de Documentos
```

---

## 4. Problema resolvido

Antes da automação, a análise dependia de consultas manuais em várias fontes:

- Planilhas de propostas comerciais.
- Planilhas de faturas e valores pagos.
- Tabelas de tarifas por cargo.
- Pastas de OS.
- Listas de documentos produzidos.
- Informações complementares de cliente, cidade e descrição da OS.

Esse processo gerava dificuldade para cruzar dados entre áreas diferentes, além de exigir muito esforço manual para montar relatórios. Também aumentava o risco de divergência entre planilhas, fórmulas incorretas, dados desatualizados ou inconsistências na comparação entre o que foi vendido e o que foi executado.

A automação centraliza esse cruzamento e transforma os dados em uma base padronizada para análise.

---

## 5. Abrangência da automação

A automação realiza as seguintes atividades:

- Extrai dados da folha de faturas.
- Extrai dados da folha de tarifas por cargo.
- Calcula HH executado a partir de valores pagos e valor/hora do cargo.
- Extrai dados das propostas do Comercial.
- Busca pastas e arquivos relacionados às OSs.
- Obtém lista de documentos técnicos produzidos.
- Extrai detalhes de OS, como cliente, descrição e cidade.
- Cruza faturamento, tarifas, propostas e documentos.
- Calcula indicadores de horas e desvios.
- Exporta os dados tratados para tabelas utilizadas pelo dashboard.
- Permite análise por OS, disciplina, cargo/função e tipo de documento.

---

## 6. Tecnologias utilizadas

| Tecnologia | Uso no projeto |
|---|---|
| **n8n** | Orquestração dos fluxos de extração, tratamento e consolidação |
| **Microsoft OneDrive / SharePoint** | Origem das pastas, planilhas e documentos |
| **Microsoft Excel 365** | Leitura das planilhas e escrita das bases consolidadas |
| **JavaScript / Code Nodes** | Tratamento dos dados, cálculos, agrupamentos e cruzamentos |
| **Dashboard Web** | Visualização dos indicadores consolidados |
| **Tabelas de apoio** | Armazenamento das bases tratadas consumidas pelo dashboard |

---

## 7. Funcionamento geral

O workflow coleta dados de diferentes fontes, transforma essas informações em estruturas padronizadas e gera bases finais para o dashboard.

Fluxo macro:

```txt
Faturas e tarifas por cargo
+ Propostas comerciais
+ Pastas de OS
+ Documentos técnicos gerados
→ Extração dos dados
→ Tratamento e padronização
→ Cálculo de HH executado
→ Cruzamento Comercial x Executado
→ Cálculo dos indicadores
→ Exportação para tabelas finais
→ Dashboard de análise
```

---

## 8. Estrutura macro do workflow n8n

Pelos blocos do workflow, o projeto pode ser dividido em cinco grandes partes:

```txt
1. Obter dados de faturas, valores pagos e tarifas
2. Obter dados das propostas do Comercial
3. Obter lista de documentos produzidos
4. Obter detalhes das OSs
5. Salvar comparativo entre Comercial e Executado
```

Cada parte possui uma responsabilidade específica dentro da consolidação.

---

# 9. Etapas detalhadas da automação

## 9.1. Obter dados de faturas, valores pagos e tarifas

Este bloco busca a planilha utilizada como fonte de faturas e tarifas.

### Responsabilidade

- Baixar a planilha de controle financeiro.
- Extrair os dados da aba de faturas.
- Extrair os dados da aba de tarifas.
- Relacionar cada valor pago ao cargo/função correspondente.
- Dividir o valor pago pelo valor/hora do cargo.
- Gerar a base de HH executado.

### Regra principal

```txt
HH Executado = Valor Pago / Valor Hora do Cargo
```

### Resultado esperado

Uma base intermediária com OS, disciplina, cargo/função, valor pago, tarifa aplicada e HH executado calculado.

---

## 9.2. Obter dados da folha de propostas do Comercial

Este bloco coleta os dados planejados pelo time Comercial.

### Responsabilidade

- Acessar a pasta onde estão as OSs.
- Percorrer os itens encontrados.
- Baixar a planilha comercial correspondente.
- Extrair a folha de proposta.
- Tratar os dados comerciais em JavaScript.
- Exportar os dados tratados para uma tabela de apoio.

### Resultado esperado

Uma base com os dados comerciais previstos, incluindo OS, cliente, disciplina, cargo/função e HH Comercial.

---

## 9.3. Obter lista de documentos

Este bloco extrai a lista de documentos técnicos gerados, que representam o produto entregue pela empresa.

### Responsabilidade

- Percorrer pastas de OS ou documentos.
- Identificar documentos associados ao projeto.
- Extrair dados de classificação documental.
- Padronizar a estrutura dos documentos para análise posterior.

### Informações esperadas

- OS.
- Disciplina do documento.
- Tipo de documento.
- Quantidade de documentos.
- Relação com HH executado, quando aplicável.

### Resultado esperado

Uma base de documentos que pode ser usada para calcular volume documental, HH por documento e distribuição por tipo, disciplina ou OS.

---

## 9.4. Obter detalhes de uma OS

Este bloco busca informações complementares de cada OS.

### Responsabilidade

- Ler a fonte associada à OS.
- Extrair detalhes descritivos.
- Filtrar apenas os dados úteis.
- Salvar informações complementares em uma tabela.

### Informações extraídas

- Nome do cliente.
- Descrição da OS.
- Cidade.
- Metadados relevantes da proposta.

### Resultado esperado

Uma tabela auxiliar para enriquecer o dashboard e permitir que as OSs sejam exibidas com informações mais legíveis.

---

## 9.5. Salvar comparativo entre Comercial e Executado

Este é o bloco responsável por consolidar as informações extraídas das fontes de proposta, faturamento, tarifas e documentos.

### Responsabilidade

- Receber os dados de proposta comercial.
- Receber os dados de valor pago e tarifa por cargo.
- Calcular HH executado.
- Cruzar informações por OS, disciplina e cargo/função.
- Calcular HH Comercial.
- Calcular HH Executado.
- Calcular saldo/desvio de horas.
- Exportar a base final para uso no dashboard.

### Resultado esperado

Uma tabela consolidada contendo a comparação entre o que foi vendido e o que foi executado.

---

# 10. Fontes de dados

| Fonte | Finalidade |
|---|---|
| **Folha de faturas** | Obter valores pagos relacionados à execução |
| **Folha de tarifas** | Obter valor/hora por cargo ou função |
| **Folha de propostas do Comercial** | Obter horas estimadas e planejamento comercial |
| **Pastas de OS** | Localizar arquivos e documentos relacionados ao projeto |
| **Lista de documentos** | Medir volume de documentação técnica produzida |
| **Detalhes da OS** | Enriquecer a análise com cliente, descrição e cidade |

---

# 11. Dados consolidados

A base final gerada pela automação organiza os dados em linhas comparáveis, normalmente estruturadas por:

- Projeto / OS.
- Cliente.
- Cidade.
- Disciplina.
- Cargo ou função.
- Tipo de documento.
- HH Comercial.
- Valor pago.
- Valor/hora do cargo.
- HH Executado.
- Saldo ou desvio de horas.
- Quantidade de documentos.

Essa estrutura permite que o dashboard aplique filtros e gere comparações por diferentes dimensões.

---

# 12. Principais métricas calculadas

## 12.1. HH Comercial / HH Estimado

Representa as horas previstas ou vendidas na proposta comercial.

```txt
HH Comercial = horas estimadas na proposta
```

No dashboard, também aparece como **HH Estimado**.

---

## 12.2. HH Executado

Representa as horas consumidas na execução, calculadas a partir dos valores pagos e do valor/hora de cada cargo.

```txt
HH Executado = Valor Pago / Valor Hora do Cargo
```

---

## 12.3. Saldo de Horas

Mostra a diferença entre as horas vendidas e as horas executadas.

```txt
Saldo de Horas = HH Comercial - HH Executado
```

Interpretação:

| Resultado | Significado |
|---|---|
| Saldo positivo | Foram vendidas mais horas do que as executadas |
| Saldo negativo | Foram executadas mais horas do que as vendidas |
| Saldo zero | Execução alinhada à estimativa |

---

## 12.4. Horas Não Previstas

Representa horas executadas que não tinham HH Comercial correspondente.

```txt
Horas Não Previstas = HH Executado sem HH Comercial associado
```

Esse indicador ajuda a identificar atividades que consumiram horas, mas não estavam previstas na proposta.

---

## 12.5. Horas Superestimadas

Representa horas vendidas, mas não consumidas na execução.

```txt
Horas Superestimadas = soma dos desvios positivos
```

Esse indicador mostra folgas de planejamento ou estimativas acima do necessário.

---

## 12.6. HH por Documento

Relaciona o volume de horas executadas com a quantidade de documentos produzidos.

```txt
HH / Doc = HH Executado / Quantidade de Documentos
```

Essa métrica ajuda a avaliar produtividade documental por tipo, disciplina ou OS.

---

# 13. Dashboard gerado

O dashboard apresenta a base consolidada de forma visual e analítica. Ele permite acompanhar indicadores gerais, aplicar filtros e comparar OSs.

## 13.1. Dashboard Geral

A visão geral apresenta os principais KPIs do projeto:

- **HH Estimado**: total de horas vendidas na proposta.
- **HH Executado**: total de horas calculadas a partir dos valores pagos e tarifas.
- **Saldo de Horas**: diferença entre HH Estimado e HH Executado.
- **Horas Não Previstas**: horas executadas sem previsão comercial.
- **Horas Superestimadas**: horas previstas que não foram consumidas.
- **Total de Documentos**: quantidade de documentos considerados na análise.

---

## 13.2. Segmentação de dados

O dashboard permite filtrar os dados por:

- Projeto / OS.
- Disciplina.
- Tipo de documento.

Também permite selecionar múltiplas OSs para análise consolidada ou comparativa.

---

## 13.3. Distribuição por tipo

A seção de distribuição por tipo permite analisar o volume de documentos e horas sob diferentes perspectivas.

Métricas disponíveis:

- Quantidade de documentos.
- HH Executado.
- HH por documento.

Agrupamentos disponíveis:

- Tipo.
- Disciplina.
- OS.

---

## 13.4. Volume total de horas

Esta seção compara o volume de horas por categoria.

Visualizações possíveis:

- HH Executado.
- HH Comercial.
- Comparativo Executado x Comercial.

A análise pode ser feita por disciplina ou por cargo/função.

---

## 13.5. Desvios

A seção de desvios mostra a diferença entre horas orçadas e horas executadas.

```txt
Desvio = HH Comercial - HH Executado
```

Interpretação:

- Valores negativos indicam estouro de horas.
- Valores positivos indicam folga ou superestimativa.

---

## 13.6. Horas não previstas

Esta seção destaca atividades que tiveram HH Executado, mas não tinham HH Comercial associado.

Esse indicador é importante para identificar:

- Escopos não previstos.
- Atividades executadas fora da proposta.
- Falhas de planejamento comercial.
- Diferenças entre o vendido e o realizado.

---

## 13.7. Superestimativas de horas

Esta seção mostra casos em que a proposta previu mais horas do que foram efetivamente consumidas.

Esse indicador ajuda a identificar:

- Folgas de planejamento.
- Propostas conservadoras.
- Estimativas acima do esforço real.
- Possíveis oportunidades de ajuste em futuras propostas.

---

## 13.8. Dispersão: Estimado vs. Executado

A dispersão permite comparar visualmente as horas estimadas com as horas executadas.

Interpretação:

- Pontos próximos da diagonal indicam boa estimativa.
- Pontos acima da diagonal indicam estouro de horas.
- Pontos abaixo da diagonal indicam superestimativa.

---

## 13.9. Dados brutos

O dashboard também disponibiliza uma tabela com os registros individuais utilizados na análise.

Campos esperados:

- Projeto / OS.
- Disciplina.
- Cargo / função.
- HH Comercial.
- HH Executado.
- Desvio.

Essa tabela permite conferência dos dados consolidados.

---

## 13.10. Comparativo de OSs

Além da visão geral, o dashboard possui uma área comparativa para analisar duas ou mais OSs.

Seções comparativas:

- Visão comparativa de KPIs.
- Comparativo de horas por disciplina.
- Comparativo de saldo de horas por disciplina.
- Comparativo de documentação técnica.

Essa visão ajuda a identificar quais projetos tiveram melhor ou pior aderência entre planejamento e execução.

---

# 14. Regras de negócio

## 14.1. Cruzamento por OS, disciplina e cargo/função

O comparativo entre Comercial e execução depende do cruzamento correto entre:

- OS.
- Disciplina.
- Cargo ou função.

Esses campos precisam estar padronizados para evitar divergências.

---

## 14.2. Cálculo do HH executado

O HH executado não vem diretamente como uma hora preenchida manualmente. Ele é calculado a partir do valor pago e da tarifa do cargo.

```txt
HH Executado = Valor Pago / Valor Hora do Cargo
```

Exemplo conceitual:

```txt
Valor pago: R$ 10.000,00
Valor/hora do cargo: R$ 200,00
HH Executado: 50h
```

---

## 14.3. HH Comercial nulo

Quando uma combinação possui HH Executado, mas não possui HH Comercial, o sistema interpreta como hora não prevista.

```txt
Se HH Comercial está vazio e HH Executado existe → Hora não prevista
```

---

## 14.4. Desvio negativo

Quando HH Executado é maior que HH Comercial, ocorre estouro de horas.

```txt
HH Comercial - HH Executado < 0 → Subestimativa / prejuízo operacional
```

---

## 14.5. Desvio positivo

Quando HH Comercial é maior que HH Executado, ocorre superestimativa.

```txt
HH Comercial - HH Executado > 0 → Superestimativa / folga de horas
```

---

## 14.6. Documentos como produto entregue

A base de documentos permite analisar o volume de entregáveis e relacionar a produção documental com o esforço executado.

```txt
HH / Doc = HH Executado / Quantidade de Documentos
```

---

# 15. Saídas da automação

A automação gera tabelas consolidadas que são utilizadas pelo dashboard.

Saídas principais:

- Base de faturas e tarifas tratada.
- Base de propostas comerciais tratada.
- Base de documentos produzidos.
- Base de detalhes das OSs.
- Base comparativa Comercial x Executado.
- Dados prontos para visualização no dashboard.

---

# 16. Benefícios gerados

## 16.1. Visibilidade operacional

A empresa passa a enxergar com clareza onde houve aderência ou divergência entre proposta e execução.

---

## 16.2. Redução de trabalho manual

O fluxo reduz o esforço necessário para coletar, cruzar e consolidar informações de várias planilhas.

---

## 16.3. Menos inconsistência nos dados

A padronização do processo reduz erros de cópia, campos divergentes e cálculos manuais incorretos.

---

## 16.4. Melhor análise comercial

O Comercial pode usar os desvios históricos para ajustar futuras propostas com base em dados reais.

---

## 16.5. Melhor análise de produtividade

A relação entre HH Executado e documentos produzidos permite avaliar produtividade por OS, disciplina e tipo de documento.

---

## 16.6. Apoio à tomada de decisão

O dashboard facilita a identificação de:

- OSs com estouro de horas.
- Disciplinas subestimadas.
- Cargos/funções com maior desvio.
- Horas não previstas.
- Documentos que consumiram mais esforço.
- Propostas superestimadas.

---

# 17. Pontos de atenção

## 17.1. Padronização das nomenclaturas

A consolidação depende da consistência de nomes de OS, disciplinas, cargos/funções e tipos de documento.

Mudanças de nomenclatura podem impedir o cruzamento correto dos dados.

---

## 17.2. Estrutura das planilhas

Se as abas de fatura, tarifa ou proposta mudarem de nome ou estrutura, os nós de extração podem precisar de ajuste.

---

## 17.3. Dados incompletos

Registros sem OS, disciplina, cargo/função, valor pago, tarifa ou HH Comercial podem prejudicar os cálculos do dashboard.

---

## 17.4. Chave de comparação

A comparação entre Comercial e Executado precisa de uma chave confiável para localizar registros equivalentes entre as fontes.

Chaves recomendadas:

- OS.
- Disciplina.
- Cargo/função.
- Cliente.
- Tipo de documento, quando aplicável.

---

## 17.5. Atualização do dashboard

O dashboard depende das bases exportadas pela automação. Caso os dados não sejam atualizados corretamente, os indicadores podem ficar defasados.

---

# 18. Melhorias futuras

## 18.1. Criar log de execução

Registrar cada execução do workflow com:

- Data e hora.
- Arquivos processados.
- Quantidade de registros gerados.
- Status da execução.
- Mensagem de erro, se houver.

---

## 18.2. Criar validação de campos obrigatórios

Antes de exportar para a tabela final, validar campos como:

- OS.
- Disciplina.
- Cargo/função.
- Valor pago.
- Valor/hora.
- HH Comercial.
- HH Executado.
- Cliente.

---

## 18.3. Criar aba de inconsistências

Quando um registro não puder ser cruzado corretamente, ele pode ser enviado para uma aba de inconsistências.

Exemplos:

- OS encontrada no faturamento, mas não na proposta.
- Cargo/função sem tarifa associada.
- Disciplina com nomenclatura divergente.
- Documento sem OS identificada.

---

## 18.4. Criar versionamento dos dados exportados

Salvar versões históricas das bases consolidadas para comparar evoluções ao longo do tempo.

---

## 18.5. Criar indicadores por período

Permitir análise por mês, trimestre ou ano, caso a base contenha datas suficientes.

---

## 18.6. Automatizar a atualização do dashboard

Integrar diretamente a automação com a fonte consumida pelo painel para reduzir etapas manuais de atualização dos dados.

---

# 19. Nomes sugeridos para o projeto

Opções de nome:

- **Consolidação Comercial de HH**
- **Dashboard de Eficiência Comercial**
- **Comparativo HH Comercial x Executado**
- **Pipeline de Consolidação Comercial**
- **Análise de Desvios de Planejamento**

Nome mais claro para documentação interna:

```txt
Consolidação Comercial e Dashboard de Eficiência de HH
```

---

# 20. Resumo executivo

Este projeto automatiza a consolidação de dados entre propostas comerciais, faturas, valores pagos, tarifas por cargo e documentos técnicos produzidos.

A automação no n8n extrai dados de planilhas e pastas, trata as informações com JavaScript, calcula HH executado a partir de valores pagos e valor/hora de cada cargo, cruza os dados do Comercial com os dados de execução e gera bases consolidadas para um dashboard.

O dashboard permite visualizar indicadores como HH Estimado, HH Executado, Saldo de Horas, Horas Não Previstas, Horas Superestimadas, Total de Documentos, distribuição por tipo, desvios por disciplina e comparativos entre OSs.

Com isso, a empresa ganha uma visão mais clara sobre a eficiência das propostas, identifica estouros de horas, entende onde houve planejamento inadequado e passa a ter uma base mais confiável para melhorar futuras estimativas comerciais.
