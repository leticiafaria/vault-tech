# prompt_documentacao_sistema.md - ## Exemplo de Uso do Prompt

Para usar este prompt, envie a mensagem abaixo, substituindo os dados de exemplo pelos dados reais do seu sistema.

### ## Dados de Entrada

**Repositorio da Wiki (onde toda a documentacao será publicada):**

`https://github.com/minha-org/meu-sistema-documentacao.wiki.git`

**Repositorios de Aplicacao:**

| # | Nome | URL | Branch | Modulo | Tipo |
|---|:---|:---|:---|:---|:---|
| 1 | servico-calculo-api | https://github.com/minha-org/meu-sistema-servico-calculo-api | develop | Calculo | API REST (.NET 8.0) |
| 2 | servico-calculo-worker | https://github.com/minha-org/meu-sistema-servico-calculo-worker | develop | Calculo | Hosted Worker |
| 3 | servico-pagamento-bw | https://github.com/minha-org/meu-sistema-servico-pagamento-bw | develop | Pagamento | Batch (.NET 8.0) |
| 4 | servico-fechamento-bot | https://github.com/minha-org/meu-sistema-servico-fechamento-bot | develop | Fechamento | Batch (Java Spring Boot 3.x) |
| 5 | servico-notificacao-bot | https://github.com/minha-org/meu-sistema-servico-notificacao-bot | master | Notificacao | Subscriber (.NET 6.0) |
| 6 | servico-notificacao-api | https://github.com/minha-org/meu-sistema-servico-notificacao-api | master | Notificacao | API REST (.NET 6.0) |
| 7 | servico-parametros-api | https://github.com/minha-org/meu-sistema-servico-parametros-api | master | Parametros | API REST (.NET 6.0) |

**Repositorios de Banco de Dados:**

| # | Nome | URL | Branch | Catalog(s) |
|---|:---|:---|:---|:---|
| 1 | db-catalogo-principal | https://github.com/minha-org/meu-sistema-db-catalogo-principal | master | DOMUS1551T01 |
| 2 | db-catalogo-notificacao | https://github.com/minha-org/meu-sistema-db-catalogo-notificacao | master | DOMUS1551STR1 |
| 3 | db-catalogo-relatorio | https://github.com/minha-org/meu-sistema-db-catalogo-relatorio | master | DOMUS1551STR2 |

---

### ### Mensagem para o Agente

Preciso documentar o sistema **[NOME DO SISTEMA]**.

**Repositorio da Wiki:**
`https://github.com/minha-org/meu-sistema-documentacao.wiki.git`

**Repositorios de aplicacao:**
[cole a tabela acima ou anexe o arquivo .md]

**Repositorios de banco de dados:**
[cole a tabela acima ou anexe o arquivo .md]

---

# # Documentacao Completa do Sistema a Partir de Repositorios (v2)

### ## Objetivo
Analisar todos os repositórios de um sistema, compreender como se conectam e interagem, e gerar documentação completa e padronizada em três níveis:

1. **"Pagina wiki por repositorio"** - Documentacao individual detalhada (Regras de Negocio + Tecnica)
2. **"Mapa Geral do Sistema (Home)"** - Visão macro de arquitetura, fluxo, dependencias, glossario, segurança, feature flags e debitos tecnicos
3. **"Mapa de Mensageria"** - Detalhamento das filas/topicos, produtores, consumidores, politicas de retry e fluxo completo
4. **"Mapa de Bancos de Dados"** - Catalogos, tabelas com campos e tipos, stored procedures, acessos por repositorio e riscos de acesso compartilhado

Além disso, gerar paginas dedicadas para visoes transversais do sistema:
* **"Mapa de Mensageria"** - Todas as filas/topicos, produtores, consumidores, politicas de retry e fluxo completo
* **"Mapa de Bancos de Dados"** - Catalogos, tabelas com campos e tipos, stored procedures, acessos por repositorio e riscos de acesso compartilhado

Toda a documentação deve ser criada a partir da **"analise direta de código-fonte"**. Não utilize wikis existentes, READMEs antigos, ou qualquer conhecimento externo como fonte de verdade. Se algo for incerto, **"Não faça suposições - verifique o código"**.

---

### ## Entrada
Uma lista contendo os repositórios do sistema, com pelo menos:
* Nome do repositorio
* URL do repositorio no GitHub (ou outro host)
* Branch mais atualizado

Adicionalmente, serão fornecidos:
* **"Repositorio da wiki"** - URL do repositorio onde a wiki será publicada (ex: `https://github.com/org/repo-documentacao.wiki.git`). Toda a documentacao será centralizada neste único repositorio. **"Nao criar wikis nos repositorios individuais de aplicacao"**.
* **"Repositorios de banco de dados"** com scripts DDL, stored procedures e migrations (quando existirem)

---

### ## Princípios Fundamentais

#### ### 1. Analise Independente - Não Confie em Documentacao Existente
* Ignore completamente qualquer README.md, wiki, ou comentario existente nos repositorios. Esses artefatos podem estar desatualizados.
* Toda a verdade documentada deve ser extraída diretamente do **"código-fonte, configurações, schemas, testes e contratos"**.
* Se houver dúvida sobre o comportamento de algo, **"leia o código"** - não presuma.

#### ### 2. Compreensao Sistemica Primeiro
* Antes de documentar qualquer peça individual, **"mapeie o sistema inteiro"**: quais repositorios existem, como se conectam (mensageria, APIs, banco de dados), e qual é o fluxo principal de ponta a ponta.
* Cada repositorio deve ser documentado com **"consciencia do contexto completo"** do sistema.

#### ### 3. Acessibilidade
* A documentacao deve ser compreensivel para qualquer pessoa, **"mesmo sem conhecimento previo do sistema"**.
* Use uma linguagem clara e direta. Inclua glossario de termos de negocio.
* Cada secao deve fornecer contexto suficiente para que um novo membro da equipe entenda o proposito e o funcionamento.

#### ### 4. Verificacao Sob Demanda
* Se um campo, enum, fila, tabela ou comportamento não estiver claro no código, **"investigue mais profundamente"** antes de documentar.
* Prefira deixar uma nota de "Não identificado no código" do que inventar informacoes.

---

### ## Fase 1: Clonagem e Analise Exploratoria

#### ### 1.1 Clonar Todos os Repositorios
Para cada repositorio na lista:
* `git clone [URL]` em um diretório organizado (ex: `~/repos/sistema/`)
* `git checkout [branch]` especificado na lista:
  * `git checkout [branch]`
  * A branch especificada na lista, caso contrario tente: `master, main, develop, trunk`

#### ### 1.2 Clonar Repositorios de Banco de Dados
Para cada repositorio de banco de dados na lista:
* `git clone [URL]`
* `git checkout [branch]`
* Estes repositorios contem scripts DDL, stored procedures, migrations e/ou seeds.

#### ### 1.3 Levantamento Inicial de Cada Repositório
Para cada repositorio de aplicacao, identifique e registre:
* **"Stack Tecnológica"** | Linguagem, Framework, versao (ex: `.csproj` -> `net8.0`, `pom.xml` -> `Java/Spring Boot`, `package.json` -> `Node 20`)
* **"Tipo de aplicacao"** | API REST, Subscriber/Consumer, Worker/Batch, Hosted Worker, Middleware.
* **"Bancos de dados"** | Connection strings em appsettings.json / application.yml, provedores, nomes de catalogo, data source, tabelas acessadas.
* **"APIs Externas"** | Chamadas REST/gRPC a outros serviços ou sistemas externos.
* **"Mensageria"** | Filas ou tópicos consumidos e produzidos (RabbitMQ, Kafka, SQS, etc.), politicas de retry.
* **"Arquitetura Interna"** | Camadas (Hexagonal, Clean, MVC), padrão de pastas.

#### ### 1.4 Levantamento do Repositorio de Banco de Dados
Para cada repositorio de banco de dados, identifique e registre:
| Item | O que buscar |
|---|---|
| **Catalogo/Schemas** | Quais bancos/schemas sao gerenciados |
| **Tabelas (DDL)** | Scripts CREATE TABLE com todos os campos, tipos, constraints (PK, FK, NOT NULL, DEFAULT) |
| **Scripts/SP** | Scripts CREATE PROCEDURE com parametros de entrada/saida, logica principal, tabelas que acessa |
| **Views** | Scripts CREATE VIEW com tabelas base |
| **Indexes** | Scripts CREATE INDEX com campos, tipos (clustered, nonclustered, unique) |
| **Migrations/Versioning** | Se há migrations ordenadas (ex: V001_create_table_sql), documentar a sequencia |
| **Seeds/Dados Fixo** | Scripts INSERT de dados de dominio (tipos, status, parametros fixos) |
| **Relacionamentos** | Foreign keys entre tabelas, identificar entidades pai/filho |

---

### ## 1.5 Mapeamento de Conexões
Antes de documentar qualquer repositorio individualmente, construa um mapa de conexoes:
Para cada repositorio:
1. Quais filas/topicos CONSOME?
2. Quais filas/topicos PRODUZ/PUBLICA?
3. Quais APIs REST de outros repos consome?
4. Quais bancos/catalogos ACESSA?
5. Quais tabelas COMPARTILHA com outros repos?
6. Quais stored procedures EXECUTA?

**Registrar tambem:**
* **"Riscos de acesso compartilhado"**: se 2 repos acessam a mesma tabela, documentar o risco de impacto cruzado em mudancas de schema.
* **"transacoes distribuidas"**: se um fluxo acessa 2+ catalogos, documentar o risco.

---

### ## Fase 2: Documentacao por Repositorio (Pagina Wiki)
Para cada repositorio, criar uma pagina wiki `wiki/[modulo/nome-repo.md]` com **"400-600 linhas"**, dividida em duas partes:

#### ## Template - Parte 1: Regras de Negocio
**markdown**
# <Nome do Repositorio>
`[Modulo: <Nome do Modulo>] [Tipo: <API/Worker/Subscriber/Batch>] [Stack: <.NET 8.0 / Java Spring Boot / etc.>]`

[Voltar ao modulo](<link>) | [Home](<link>)

### ### 8.1 Visao Geral
O que este repositorio faz no contexto do sistema. Qual problema de negocio resolve.

### ### 8.2 Glossário Local
Termos de negocio especificos deste componente (complementa o glossario geral da Home).

### ### 8.3 Workflow Principal
Descricao passo a passo do fluxo principal com diagrama ASCII:
`[entrada] -> [processamento] -> [saida]`

### ### 8.4 Regras de Validacao
Quais validacoes sao feitas nos dados de entrada.

### ### 8.5 Cenarios de Uso
* **Cenario Feliz (Happy Path)**
* **Cenarios de erro/excecao**

### ### 8.6 Integracao com Outros Repositorios
Como se conecta com outros repos: mensageria, APIs REST, banco compartilhado.

### ### 8.7 Pontos de Atencao de Negocio
Comportamentos nao obvios, edge cases, decisoes de negocio relevantes.

---

#### ## Template - Parte 2: Documentacao Tecnica

### ### 9.1 Estrutura de Projeto
Arvore de diretorios com descricao de cada camada/projeto.

### ### 9.2 Dependências e Bibliotecas Principais

### ### 9.3 APIs Expostas (se aplicável)
Para cada controller/endpoint:
| Metodo | Rota | Descricao | Autenticacao |
|---|---|---|---|
| Incluir: versao da API (ex: `ApiVersion("1.0")`), Swagger/OpenAPI (URL do acesso se habilitado). |
Exemplo curl de uso.

### ### 9.4 Mensageria
| Direcao | Fila/Topico | Exchange | Formato | Descricao |
|---|---|---|---|---|
Politica de retry (quantidade de retentativas, intervalos, comportamento na ultima retentativa).

### ### 9.5 Banco de Dados
| Catalogo | Tabela | Tabelas Acessadas |
|---|---|---|
Listar procedures executadas.

### ### 9.6 Configuracao
Principais chaves de `appsettings.json` / `application.yml` com descricao do que cada uma controla.
* **"Feature Flags"**: flags que alteram comportamento em runtime.

### ### 9.7 Modelo de Dominio
Classes/entidades principais com propriedades e tipos.

### ### 9.8 Modelo de Seguranca
Como a autenticacao/autorizacao é tratada:
* Se endpoints sao `[Authorize], [AllowAnonymous], etc.`
* Se a auth é feita no API Gateway (documentar como decisao arquitetural, nao como bug)
* Se ha endpoints que aceitam caminhos de arquivo ou input de usuario sem sanitizacao (riscos)

### ### 9.9 Testes
| Projeto de Teste | Quantidade | Framework | Descricao |
Comando para executar: `dotnet test` / `mvn test` / etc.

### ### 9.10 Execucao Local
Instrucoes para executar a aplicacao localmente (comando, variaveis de ambiente necessarias).

### ### 9.11 Débitos Técnicos
Lista de problemas identificados no código:
* Framework desatualizado (EOL)
* Hardcoded strings
* Transacoes com timeout infinito (`TransactionScope` sem valor)
* Configurações residuais nao utilizadas (ex: `JWT` configurado mas usando `[AllowAnonymous]` em todos os endpoints)
* SQL Injection potencial (uso de string crua em query)
* Caminhos hardcoded (ex: caminhos Windows em deploy Linux)
* Codigo comentado ou morto
* Falta de tratamento de erro
* Feature flags que desabilitam modulos inteiros

### ### 9.12 Diagrama de Fluxo Ponta a Ponta
Diagrama ASCII detalhado mostrando a jornada completa dos dados neste repositorio:
`[Entrada] -> [Camada A] -> [Camada B] -> [Banco/Fila] -> [Saida]`
Incluir bancos, filas e APIs externas em cada etapa.

---

### ## Fase 3: Mapa Geral do Sistema (Home)
Criar a pagina `home.md` como pagina principal da wiki, contendo **"todas"** as seguintes secoes:

### ### 3.1 Visao Geral
Descricao do sistema: o que faz, qual problema resolve, quem sao os usuarios.

### ### 3.2 Glossario Geral
| Termo | Significado |
|---|---|
Linguagens acessiveis. Inclua todos os termos que um novo membro da equipe precisaria conhecer.

### ### 3.3 Arquitetura Geral
Diagrama ASCII macro mostrando todos os modulos, como se conectam, fluxo principal.
`[Modulo A] --fila--> [Modulo B] --REST--> [Modulo C] --DB--> [Banco]`

### ### 3.4 Fluxo Principal (Happy Path)
Jornada completa de um item desde a entrada no sistema até a saida final:
1. **ENTRADA**: descricao
   * Repo: nome-repo
   * Acao: o que acontece
   * Destino: nome-fila
2. **PROCESSAMENTO**: descricao
   * **"Deve incluir em cada etapa"**: repo responsavel, banco acessado, fila consumida/produzida.

### ### 3.5 Tabela de Modulos e Repositorios
| Modulo | Repositorios | Descricao |
|---|---|---|
Com link para a pagina de cada modulo.

### ### 3.6 Navegacao por Tipo
Agrupar repositorios por tipo de aplicacao:
* APIs REST
* Subscribers/Consumers
* Workers/Batches
* Middlewares

### ### 3.7 Stack Tecnológica
Resumo consolidado: linguagens, frameworks, versoes, ORM, mensageria, bancos.

### ### 3.8 Modelo de Segurança
* **"Obrigatorio"**: Documentar como se autenticam/autorizam funciona no sistema.
* Se a auth é feita no API Gateway (ou por servicos use `[AllowAnonymous]`), documentar como **"decisao arquitetural"**
* Se a auth é delegada ao API Gateway (Documentar como decisao arquitetural, nao como bug)
* JWT/OAuth configurado mas nao utilizado - documentar como configuracao residual.

### ### 3.9 Feature Flags
Tabela consolidada de todas as feature flags encontradas no código:
| Flag | Repositorio | Valor Padrao | Efeito |
Feature flags controlam comportamentos criticos e devem estar visiveis em um unico lugar.

### ### 3.10 Debitos Tecnicos do Sistema
Lista consolidada de todos os debitos tecnicos encontrados:
* Frameworks desatualizados (versoes EOL) com tabelas | Repo | Framework | Status EOL |
* SQL Injection potencial
* Transacoes com timeout infinito
* Configuracoes residuais
* Caminhos hardcoded

### ### 3.11 Dependencias entre Repositorios
Qual repo depende de qual (via REST, mensageria, banco compartilhado).

---

### ## Fase 4: Mapa de Mensageria (Pagina Dedicada)
Criar a pagina `Mapa-de-Mensageria.md` com visao consolidada de **"toda"** a mensageria do sistema:

### ### 4.1 Visao Geral
Qual tecnologia de mensageria (RabbitMQ, Kafka, etc.), quantas filas/topicos, arquitetura geral.

### ### 4.2 Tabela de Filas/Topicos
| Fila/Topico | Exchange | Produtor(es) | Consumidor(es) | Modulo | Formato da Mensagem |
|---|---|---|---|---|---|

### ### 4.3 Politica de Retry
* Como funciona o retry no sistema?
* Quantas filas de retry por fila principal?
* Quantas filas de DLQ?
* Comportamento na ultima tentativa (dead-letter, descartar, fallback)
* Configuracoes relevantes (MaxConsecutiveErrors, WorkerTimeout, etc.)

### ### 4.4 Formato das Mensagens
Exemplos de payloads JSON/Avro/Protobuf de mensagens com campos e tipos (extrair das classes de contrato/DTO no codigo).

### ### 4.5 Diagrama de Fluxo de Mensagens
Diagrama ASCII mostrando a jornada das mensagens pelo sistema:
`[Produtor A] --publica--> [Fila 1] --consome--> [Consumidor B] --publica--> [Fila 2] --consome--> [Consumidor C]`

---

### ## Fase 5: Mapa de Bancos de Dados (Pagina Dedicada)
Criar a pagina `Mapa-de-Bancos-de-Dados.md` com visao consolidada de **"todos"** os bancos de dados do sistema:

### ### 5.1 Visao Geral dos Catalogos/Schemas
| Catalogo | Modulo Principal | Data Source | Repos que Acessam (quantidade) |
|---|---|---|---|

### ### 5.2 Detalhamento por Catalogo
Para **"cada catalogo/schema"**, documentar:

#### #### 5.2.1 Tabelas
Extrair dos repositorios de banco de dados (DDL) e cruzar com o codigo das aplicacoes:
`**Para cada tabela:**`
**markdown**
##### ##### <table>
Extrair dos repositorios de banco de dados (DDL) e cruzar com o codigo das aplicacoes:
* **"Descricao"**: O que esta tabela armazena.
* **"Acessada por"**: repositorios (SELECT, INSERT, UPDATE, DELETE).
| Coluna | Tipo | Nullable | Default | PK | FK | Descricao |
|---|---|---|---|---|---|---|
| CD_CAMPO | int | NOT NULL | (nextval) | PK | | Codigo Identificador |
| DS_NOME | varchar | NOT NULL | | | | Nome do Registro |
| CD_REFE | int | NULL | | | FK | Tabela_Referencia |

`*Fontes para extrair campos e tipos:*`
1. **"Repositorio de banco de dados (fonte primaria)"**: Scripts SQL (CREATE TABLE), migrations, ALTER TABLE.
2. **"Codigo das aplicacoes (fonte complementar)"**: Classes de dominio/DTO, mapeamentos SQL (Dapper, SqlBulkCopy column mappings), entidades EF Core (Fluent API), queries SQL inline.

`**Regras de cruzamento:**`
* Se o campo existe apenas no repo de banco: usar tipo e constraints do DDL (fonte autoritativa).
* Se o campo existe apenas no codigo da aplicacao: documentar com nota de "Identificado no codigo, nao encontrado no DDL".
* Se ha divergencia entre DDL e codigo: documentar ambos as versoes com alerta.

#### #### 5.2.2 Stored Procedures
Extrair dos repositorios de banco de dados:
`**Para cada stored procedure:**`
**markdown**
##### ##### PR_Nome_Procedure
* **"Catalogo"**: CATALOGO
* **"Descricao"**: O que faz
* **"Timeout"**: Valor (default 1200s)
* **"Parametros"**:
| Nome | Tipo | Direcao | Descricao |
|---|---|---|---|
| @param1 | int | IN | Codigo de retorno |
* **"Tabelas que acessa"**: TR_A (SELECT, UPDATE), TR_B (INSERT)
* **"Logica Principal"**:
Descrever em alto nivel o que a procedure faz (nao copiar o codigo inteiro, mas descrever os passos principais).
* **"Pontos de atencao"**:
Cursores, loops, transacoes longas, temp tables, etc.

*Se o codigo da stored procedure nao estiver disponivel nos repositorios de banco de dados:*
Criar a estrutura acima com o que puder ser inferido do codigo da aplicacao (nome, parametros, timeout).
Marcar explicitamente: "Codigo-fonte da procedure nao disponivel no repositorio. Estrutura inferida a partir do codigo da aplicacao que a invoca."

#### #### 5.2.3 Views
Para cada view:
| Nome | Catalogo | Tabelas Base |
|---|---|---|

#### #### 5.2.4 Indexes
Documentar indexes nao-padrao (excluir PKs que ja estao na tabela):
| Tabela | Nome | Colunas | Tipo | Unique |
|---|---|---|---|---|

#### #### 5.2.5 Triggers
| Tabela | Trigger | Evento | Descricao |
|---|---|---|---|

#### #### 5.2.6 Seeds/Dados de Dominio
Documentar tabelas com dados fixos (tipos, status, parametros):
**markdown**
| Codigo | Descricao |
|---|---|
| 1 | Tipo A |
| 2 | Tipo B |
Estes dados sao essenciais para entender os enums e codigos usados no sistema.

### ### 5.3 Mapa de Acesso por Repositorio
Tabela consolidada mostrando qual repo acessa qual catalogo e quais tabelas:
| Repositorio | Catalogo | Tabelas | Tipo de Acesso (R/W/D) | Stored Procedures |
|---|---|---|---|---|

### ### 5.4 Relacionamentos entre Tabelas
Uso de lista ou diagrama de foreign keys inter-tabelas, especialmente entre tabelas de catalogos diferentes (quando repos fazem JOINs cross-database).

### ### 5.5 Riscos de Acesso Compartilhado
Identificar tabelas acessadas por 2+ repositorios:
* Listar tabelas acessadas por múltiplos repositorios (Risco de impacto cruzado em mudancas de schema)
* Repositorios que acessam 2+ catalogos (transacoes distribuidas)
* Stored procedures chamadas por multiplos repos
* Tabelas sem foreign keys explicitas que deveriam ter (integridade referencial ausente)

---

### ## Fase 6: Documentacao por Modulo (Paginas Wiki)
Para cada modulo, criar uma pagina wiki `wiki/[modulo/modulo.md]` com **"400-600 linhas"**:

#### ## Template de Pagina de Modulo
**markdown**
# Modulo: <Nome>
`[Home](<link>) | [Mapa de Mensageria](<link>) | [Mapa de Bancos de Dados](<link>)`

### ### 8.1 Visao Geral do Modulo
O que este modulo faz no contexto do sistema.

### ### 8.2 Repositorios
| # | Repositorio | Tipo | Stack | Descricao |
|---|---|---|---|---|
Com link para pagina individual de cada repo.

### ### 8.3 Arquitetura do Modulo
Diagrama ASCII mostrando como os repos do modulo se conectam:
`[repo-a] --fila--> [repo-b] --REST--> [repo-c]`
| | |
`[BANCO: CATALOGO]`

### ### 8.4 Fluxo Principal do Modulo
Passo a passo detalhado do fluxo principal, com banco e fila em cada etapa.

### ### 8.5 Bancos de Dados do Modulo
Quais catalogos, tabelas principais, stored procedures usados pelos repos deste modulo.

### ### 8.6 Mensageria do Modulo
Filas consumidas e produzidas pelos repos deste modulo.

### ### 8.7 Integracao com Outros Modulos
Como este modulo se conecta com os demais (quais filas/APIs/bancos cruzam fronteiras de modulo).

### ### 8.8 Pontos de Atencao
Debitos tecnicos, riscos, comportamentos nao obvios.

### ### 8.9 Decisoes Arquiteturais
Decisoes relevantes identificadas no codigo (ex: "este repo foi projetado para ser generico mas é usado apenas para X").

---

### ## Fase 7: Navegacao e Publicacao

#### ### 7.1 Sidebar
Criar `_sidebar.md` com navegacao hierarquica:
**markdown**
* [Home](Home)
* **Modulos**
  * [Nome Modulo 1](modulo-1)
  * [Nome Modulo 2](modulo-2)
* [Mapa de Mensageria](Mapa-de-Mensageria)
* [Mapa de Bancos de Dados](Mapa-de-Bancos-de-Dados)

#### ### 7.2 Footer
Criar `_footer.md` com metadados:
**markdown**
Documentacao gerada a partir de codigo-fonte. Ultima atualizacao: <data>.

#### ### 7.3 Links
* **"Todos os links devem ser URLs completas"** (ex: `https://github.com/org/repo-documentacao.wiki/Home`), nao links relativos ou em formato `wiki/pagina.md` (nao funcionam corretamente com subdiretorios na wiki do GitHub).
* Publicar diretamente no repositório de documentação informado na entrada (push direto no repo .wiki.git).
* Toda a documentacao deve ficar centralizada neste único repositorio wiki. Nao criar wikis, READMEs ou documentacoes individuais de repositorios.
* **"Priorizar sempre o push das alteracoes"** ao subir as mudancas antes de reportar ao usuario.

---

### ## Outputs Esperados
| Artefato | Local | Quantidade |
|---|---|---|
| Pagina wiki por repositorio | `wiki/[modulo/nome-repo.md]` | 1 por repositorio |
| Pagina wiki por modulo | `wiki/[modulo/modulo.md]` | 1 por modulo |
| Home da wiki | `wiki/Home.md` | 1 |
| Sidebar da wiki | `wiki/_sidebar.md` | 1 |
| Footer da wiki | `wiki/_footer.md` | 1 |
| Mapa de Mensageria | `wiki/Mapa-de-Mensageria.md` | 1 |
| Mapa de Bancos de Dados | `wiki/Mapa-de-Bancos-de-Dados.md` | 1 |

---

### ## Checklist de Qualidade por Pagina
Antes de considerar uma pagina completa, verificar:

#### #### Pagina de Repositorio
- [ ] Glossario local presente
- [ ] Diagrama ASCII do workflow principal
- [ ] APIs expostas (se aplicavel) com metodo, rota, autenticacao e exemplo curl
- [ ] Versao da API documentada (se aplicavel)
- [ ] Swagger/OpenAPI URL documentada (se disponivel)
- [ ] Todas as filas/topicos consumidos e produzidos listados com formato de mensagem
- [ ] Todos os bancos/catalogos e tabelas acessados listados
- [ ] Instrucoes de execucao local
- [ ] Modelo de seguranca documentado (auth, [AllowAnonymous], riscos)
- [ ] Se a auth é delegada ao API Gateway (Documentar como decisao arquitetural, nao como bug)
- [ ] Secao de testes com quantidade e comando de execucao
- [ ] Debitos tecnicos identificados e sinalizados
- [ ] Diagrama de fluxo ponta a ponta com bancos e filas
- [ ] Campos com todos os valores concretos
- [ ] 400-600 linhas

#### #### Home (Wiki)
- [ ] Glossario geral completo
- [ ] Diagrama de arquitetura geral (ASCII)
- [ ] Fluxo principal com bancos e filas em cada etapa
- [ ] Tabela de modulos com links
- [ ] Feature flags consolidadas
- [ ] Debitos tecnicos consolidados
- [ ] Stack tecnologica

#### #### Mapa de Mensageria
- [ ] Tabela completa de filas/topicos de sistemas listados
- [ ] Produtor e consumidor de cada fila
- [ ] Politica de retry e DLQ descrita
- [ ] Formato JSON/Avro/Protobuf de mensagens
- [ ] Diagrama de fluxo de mensagens (ASCII)

#### #### Mapa de Bancos de Dados
- [ ] Dicionario de dados completo de catalogos listados
- [ ] Tabelas com campos, tipos e constraints extraidos do DDL
- [ ] Stored procedures com parametros e logica principal
- [ ] Views, indexes e triggers documentados
- [ ] Seeds/dados de dominio listados
- [ ] Matriz de acesso por repositorio
- [ ] Mapa de acesso por repositorio
- [ ] Riscos de acesso compartilhado e transacoes distribuidas
- [ ] Cruzamento DDL x codigo-fonte (divergencias documentadas)

---

### ## Notas Importantes

1. **"O codigo é a fonte de verdade."** Documentacoes existentes, comentarios no codigo e READMEs antigos podem estar desatualizados. Use-os apenas como pista, nunca como fato.
2. **"Analisar TODOS os repositorios antes de documentar qualquer um."** O contexto do sistema inteiro é necessario para documentar cada peca corretamente.
3. **"Se nao conseguir determinar algo com certeza"**, documentar como "Nao foi possivel identificar no codigo" em vez de inventar.
4. **"As linguagens devem ser clara e em português brasileiro"** (no idioma solicitado pelo usuario). Termos tecnicos em ingles podem ser mantidos quando sao padrao da industria (ex: "retry", "subscriber", "endpoint").
5. **"Diagramas ASCII sao obrigatorios"** nos fluxos principais. Eles ajudam enormemente na compreensao visual do sistema.
6. **"Cenarios de uso devem ser concretos."** Use valores realistas (ex: "R$ 1.500,00 de comissao a vista" em vez de "valor X de comissao").
7. **"Pontos de atencao devem ser honestos."** Se encontrar codigo com problemas (variaveis em thread-safe, transacoes mal configuradas, falta de validacao), documente. A documentacao serve para alertar a equipe.
8. **"Repositorios de banco de dados sao a fonte autoritativa para DDL."** Quando houver divergencia entre o DDL do repositorio de banco e o codigo da aplicacao, priorizar o DDL e documentar a divergencia.
9. **"Links devem ser URLs completas."** Nao usar `[wiki-links]` ou links relativos - eles nao funcionam corretamente com subdiretorios na wiki do GitHub.
10. **"Publicar alteracoes antes de reportar."** Sempre subir as mudancas na wiki antes de informar ao usuario sobre o progresso.
11. **"Feature flags e modelo de seguranca sao obrigatorios."** Devem estar tanto na pagina individual do repo quanto na visao consolidada da Home. Omiti-los pode causar confusao em novos membros da equipe.
12. **"Enums devem ter todos os valores documentados com significado."** Enums controlam comportamentos criticos e devem ser facilmente consultaveis.
