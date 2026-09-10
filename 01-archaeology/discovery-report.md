# Relatório de Descoberta — Estágio 1: Arqueologia Digital

> **Trilha:** [Kit do Time](../README.md) › [Estágio 1](README.md) › **Relatório de Descoberta**

**Artefato preenchido pelo time ao fim do Estágio 1.** Consolida os achados da arqueologia e é a entrada principal do Estágio 2.

| Campo                  | Valor                                                                                |
| ---------------------- | ------------------------------------------------------------------------------------ |
| **Público-alvo**       | Todas as duplas — consolidação ao fim do Estágio 1                                   |
| **Pré-requisitos**     | Catálogo de regras, mapa de dependências e glossário preenchidos                     |
| **Estágio**            | Estágio 1 — Arqueologia                                                              |
| **Resultado esperado** | Documento de até 3 páginas com resumo, hipóteses de fatiamento e artefatos de origem |

> [!IMPORTANT]
> Este documento consolida todos os achados do Estágio 1. Preencha cada seção com as conclusões do time. Sem ele, a especificação do Estágio 2 não tem base de evidência.

> [!NOTE]
> Guia passo a passo: [`GUIDE.md`](GUIDE.md).

**Time**: `Franklyn`
**Data**: 2026-09-10
**Edição**: Imersão de modernização de legado — SIFAP
**Participantes**: cobertura individual dos cinco domínios (cadastro, batch, cálculo, validação, consultas e relatórios)

---

## 1. Resumo executivo

O SIFAP, Sistema de Fiscalização e Administração de Pagamentos, tem 29 anos e reúne 24 membros Natural e 4 arquivos Adabas que concedem benefícios sociais, geram a folha mensal de pagamentos e conciliam o retorno bancário. A leitura integral dos 15 programas atribuídos extraiu **176 regras candidatas**, das quais apenas **24 têm confirmação documental** e **90 são questões em aberto**. O acoplamento não está nas chamadas entre programas — são apenas 9 `CALLNAT` — e sim na área de dados `LDASIFAP`, usada por 13 dos 24 membros, e no arquivo `BENEFIC`, acessado por 9 programas. O maior risco para o Estágio 2 é que **o cálculo do benefício, o critério de desconto e a definição de CPF válido têm implementações concorrentes e incompatíveis dentro do próprio sistema**, sem que nenhuma fonte indique qual é a vigente. A confiança da equipe na modernização é **média**: a estrutura é pequena e legível, mas nenhuma regra financeira pode ser reimplementada com segurança antes da validação humana das questões em aberto.

---

## 2. O que sabemos (confirmado)

### 2.1 Regras de negócio

24 das 176 regras têm evidência literal em documentação histórica ou em comentário do próprio código. Amostra representativa; a lista completa está em [`business-rules-catalog.md`](business-rules-catalog.md).

| Regra                                                          | Candidato EARS                                                                                            | Origem                   | Confirmação                                                                |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------ | -------------------------------------------------------------------------- |
| CPF obrigatório e validado por dígito verificador              | `SE o CPF for inválido, ENTÃO o sistema DEVE rejeitar o cadastro`                                         | `CADBENEF.NSP:L145-L168` | RN-001 ([catálogo, `CADBENEF` nº 2 e 3](business-rules-catalog.md))        |
| Processamento restrito a beneficiários ativos, em ordem de CPF | `O sistema DEVE processar os beneficiários ativos em ordem crescente de CPF`                              | `BATCHPGT.NSP:L246-L266` | §5.1 ([catálogo, `BATCHPGT` nº 2](business-rules-catalog.md))              |
| Elegibilidade exige situação cadastral ativa                   | `SE o beneficiário não estiver ativo, ENTÃO o sistema DEVE declará-lo inelegível`                         | `VALELEG.NSN:L130-L153`  | §4.2 ([catálogo, `VALELEG` nº 3](business-rules-catalog.md))               |
| Renda familiar limitada ao teto do programa                    | `SE a renda familiar exceder o teto do programa, ENTÃO o sistema DEVE declarar o beneficiário inelegível` | `VALELEG.NSN:L172-L182`  | §4.2 ([catálogo, `VALELEG` nº 5](business-rules-catalog.md))               |
| Centavos truncados, nunca arredondados                         | `O sistema DEVE truncar os centavos em cada etapa do cálculo`                                             | `CALCBENF.NSN:L264-L266` | RN-014 ([catálogo, `CALCBENF` nº 10](business-rules-catalog.md))           |
| Teto de 30% do valor bruto para descontos                      | `SE o total de descontos exceder 30% do valor bruto, ENTÃO o sistema DEVE limitá-lo a esse teto`          | `CALCDSCT.NSP:L106-L111` | RN-021 ([catálogo, `CALCDSCT` nº 3](business-rules-catalog.md))            |
| Desconto judicial não observa o teto                           | `ONDE o desconto for de origem judicial, o sistema DEVE aplicá-lo sem observar o teto`                    | `CALCDSCT.NSP:L128-L137` | §3 ([catálogo, `CALCDSCT` nº 4](business-rules-catalog.md))                |
| Inclusão e alteração geram registro de auditoria               | `QUANDO um cadastro for incluído ou alterado, o sistema DEVE registrar o evento na trilha de auditoria`   | `CADBENEF.NSP:L296-L327` | RN-010 ([catálogo, `CADBENEF` nº 23](business-rules-catalog.md))           |
| Consulta a dado pessoal é auditada                             | `QUANDO uma consulta for realizada, o sistema DEVE registrar o acesso na trilha de auditoria`             | `CONSBENF.NSP:L169-L180` | IN-TCU 63/2010 ([catálogo, `CONSBENF` nº 5](business-rules-catalog.md))    |
| Relatório consolidado com retenção de dez anos                 | `O sistema DEVE gerar cópia de arquivamento do relatório`                                                 | `BATCHREL.NSP:L20`       | Lei 8159, art. 14 ([catálogo, `BATCHREL` nº 7](business-rules-catalog.md)) |

### 2.2 Dependências

Todas as arestas têm evidência `arquivo:linha` em [`dependency-map.md`](dependency-map.md). **Nenhuma referência quebrada no código.**

| Tipo de aresta                   | Total |
| -------------------------------- | ----: |
| JCL → programa                   |     3 |
| `CALLNAT`                        |     9 |
| `INCLUDE`                        |     9 |
| `USING` (áreas de dados)         |    23 |
| Programa → DDM                   |    37 |
| Sub-rotinas internas (`PERFORM`) |    25 |

**Nós mais conectados:** `LDASIFAP` (13 `USING`), `CCAUDIT` (7 `INCLUDE`), `PDAVALID` (7 `USING`), `SUBVALCP` (4 `CALLNAT`) e o DDM `BENEFIC` (13 acessos, de 9 programas).

**Somente 3 dos 15 programas são agendados:** `BATCHPGT` por `SIFAPJ01`; `BATCHREL` e `RELPGT` por `SIFAPJ02`. `BATCHCON` é o único programa batch sem JCL.

### 2.3 Estruturas de dados

Quatro DDMs documentados, conforme [`inventory.md`](inventory.md) e as leituras cruzadas do catálogo.

| DDM       | FNR | Papel                                                                                                                         | Quem escreve                                                                    |
| --------- | --- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `BENEFIC` | 150 | Cadastro de beneficiários, cerca de 4,2 milhões de registros. Grupo periódico de até 10 dependentes                           | `CADBENEF`, `CADDEPEN`                                                          |
| `SOCPROG` | 151 | Tabela de parâmetros dos programas sociais, cerca de 45 ativos. Grupos periódicos de faixas de cálculo e parâmetros regionais | `CADPROG` (apenas `STORE`; **nenhum `UPDATE` na biblioteca**)                   |
| `PAYMENT` | —   | Pagamentos gerados, com grupo periódico de até 8 descontos                                                                    | `BATCHPGT`, `CALCBENF` (`STORE`); `BATCHCON`, `CALCCORR`, `CALCDSCT` (`UPDATE`) |
| `AUDIT`   | 153 | Trilha de auditoria, IN-TCU 63/2010                                                                                           | `CCAUDIT` (via `INCLUDE` em 7 programas) e `BATCHCON`                           |

Existe uma listagem FDT apenas para o arquivo 150 (`FDT-150-BENEFICIARY.txt`); os outros três não têm FDT no acervo.

---

## 3. O que é arriscado

### 3.1 Questões em aberto aguardando validação humana

**As 29 questões registradas estão abertas; nenhuma recebeu validação humana.** A tabela abaixo traz os 20 identificadores canônicos com pergunta, evidência inicial e responsável. **Impacto e hipótese não confirmada estão preservados integralmente em [`mysteries-found.md`](mysteries-found.md)** e foram omitidos aqui apenas para manter o limite de três páginas; os 9 achados bônus também constam lá.

| ID           | Questão em aberto                                                                                 | Evidência (`path:line`)                                                     | Pessoa/área responsável                | Status |
| ------------ | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------- | ------ |
| `SIFAP-M-01` | Qual é o número máximo de dependentes por beneficiário hoje em vigor, e quando esse limite mudou? | `CADDEPEN.NSP:L117`; `BENEFIC.ddm:L88`; `BUSINESS-RULES-2012.md:L81`        | Área de Benefícios (CGPB)              | Aberta |
| `SIFAP-M-02` | Quais são os códigos de grau de parentesco válidos, e o que existe hoje gravado no arquivo 150?   | `CADDEPEN.NSP:L152-L153`; `BENEFIC.ddm:L92-L93`                             | DBA Adabas (CGTI/MDAS)                 | Aberta |
| `SIFAP-M-03` | As três validações corporativas de 2011 devem bloquear a gravação ou permanecer em modo aviso?    | `CADBENEF.NSP:L154-L268`; `BUSINESS-RULES-2012.md:L72`                      | Área de Benefícios                     | Aberta |
| `SIFAP-M-04` | De onde vem a constante `0.347215`, e quem grava o campo `FACTOR-K` do arquivo 151?               | `CADPROG.NSP:L124`; `SOCPROG.ddm:L45-L49`                                   | Coordenação de Benefícios (SENARC)     | Aberta |
| `SIFAP-M-05` | O valor do benefício é calculado por soma ou por multiplicação de fatores?                        | `BATCHPGT.NSP:L430-L432`; `BUSINESS-RULES-2012.md:L115-L124`                | Coordenação de Benefícios (SENARC)     | Aberta |
| `SIFAP-M-06` | Quais descontos são efetivamente aplicados na folha mensal?                                       | `BATCHPGT.NSP:L16` e `L457-L462`; `CALCDSCT.NSP:L71-L76`                    | Área de Benefícios                     | Aberta |
| `SIFAP-M-07` | Qual é o critério oficial de arredondamento dos valores do benefício?                             | `BATCHPGT.NSP:L433-L436`; `BATCHREL.NSP:L166-L170`                          | SENARC e prestação de contas           | Aberta |
| `SIFAP-M-08` | Como são encerrados os pagamentos com divergência bancária e os retornos sem correspondência?     | `BATCHCON.NSP:L14-L15`, `L179-L200`, `L228-L231`                            | Área financeira / convênio CNAB        | Aberta |
| `SIFAP-M-09` | Quantos registros de pagamento são criados por beneficiário em cada ciclo mensal?                 | `CALCBENF.NSN:L308-L320`; `BATCHPGT.NSP:L473-L489`                          | SENARC e DBA Adabas                    | Aberta |
| `SIFAP-M-10` | O campo `COD-REGION` identifica região ou unidade federativa, e qual é o domínio válido?          | `CALCBENF.NSN:L96-L125`; `BENEFIC.ddm:L70`                                  | SENARC e DBA Adabas                    | Aberta |
| `SIFAP-M-11` | Como é calculada a correção monetária retroativa dos pagamentos?                                  | `CALCCORR.NSP:L82-L128`, `L229-L239`                                        | Coordenação de Benefícios (SENARC)     | Aberta |
| `SIFAP-M-12` | Qual é a alíquota devida da contribuição social sobre o benefício?                                | `CALCDSCT.NSP:L60-L69`; `CALCBENF.NSN:L356-L367`                            | SENARC e área tributária               | Aberta |
| `SIFAP-M-13` | O que é a região 99 e quem autoriza a dispensa integral da validação de elegibilidade?            | `VALELEG.NSN:L120-L128`; `BUSINESS-RULES-2012.md:L199-L205`                 | SENARC e auditoria interna             | Aberta |
| `SIFAP-M-14` | O que são os oito prefixos de CPF que dispensam a validação documental?                           | `VALDOCS.NSP:L56-L65`, `L226-L241`                                          | Área de Benefícios e auditoria interna | Aberta |
| `SIFAP-M-15` | Qual é a definição oficial de CPF válido no SIFAP?                                                | `CADBENEF.NSP:L344-L414`; `VALBENEF.NSN:L229-L246`; `VALDOCS.NSP:L137-L205` | Área de Benefícios                     | Aberta |
| `SIFAP-M-16` | Quem preenche o indicador de documentação em ordem exigido pela elegibilidade?                    | `VALELEG.NSN:L195-L200`; `BENEFIC.ddm:L84`                                  | Área de Benefícios e DBA Adabas        | Aberta |
| `SIFAP-M-17` | Por que o relatório de auditoria oculta os eventos de exclusão?                                   | `RELAUDIT.NSP:L128-L133`                                                    | Auditoria interna                      | Aberta |
| `SIFAP-M-18` | Qual é a tabela oficial de códigos de ação da trilha de auditoria?                                | `RELAUDIT.NSP:L163-L182`; `CONSBENF.NSP:L172-L177`                          | Auditoria interna e DBA Adabas         | Aberta |
| `SIFAP-M-19` | Qual é a regra de mascaramento de CPF exigida para tela e para relatório impresso?                | `CONSBENF.NSP:L292-L311`; `RELPGT.NSP:L163-L168`                            | Encarregado de dados e auditoria       | Aberta |
| `SIFAP-M-20` | Os subtotais por programa social do relatório analítico estão corretos?                           | `RELPGT.NSP:L123-L150`                                                      | Prestação de contas e SENARC           | Aberta |

### 3.2 Regras com evidência fraca

**62 regras estão classificadas como inferidas** — sustentadas apenas pela leitura do código, sem confirmação documental. Fundamentar requisitos nelas transfere para o sistema novo comportamentos que ninguém validou. Concentram-se em quatro grupos, todos detalhados em [`business-rules-catalog.md`](business-rules-catalog.md):

- **Constantes numéricas sem origem:** fatores familiares de 0,0500, 0,0300 e 0,0200; faixas de renda de 300 a 9999,99; fatores de idade de 60, 65 e 18 anos; limite de 75 anos para suspensão; abono de 15%.
- **Regras por ausência de código:** validações que a documentação exige e que nenhum programa implementa — idade mínima de 16 anos, domínio do código de região, limite de dependentes no cadastro, vínculo com programa social ativo no `CADBENEF`.
- **Comportamentos de fluxo:** o idioma `FIND` / `IF NO RECORDS FOUND` / `MOVE TRUE` sem `ESCAPE`, presente em três programas, cuja semântica exata depende do runtime Natural 4.2.
- **Campos declarados e nunca gravados:** `STAT-DEPEND`, `IND-DISABILITY`, `IND-DOCS-OK`, `AMT-PREV` e `AMT-NEW`.

---

## 4. Hipóteses de fatiamento recomendadas

> [!NOTE]
> **São hipóteses, não decisões.** Derivam de agrupamentos observados em [`dependency-map.md`](dependency-map.md): conjuntos com muitas arestas internas e poucas externas. Cabe ao arquiteto avaliá-las e decidir no Estágio 2.

### Hipótese 1: Cadastro de Beneficiário

- **Programas:** `CADBENEF`, `CADDEPEN`, `CONSBENF`, `VALBENEF`, `VALDOCS`, `SUBVALCP`, `SUBVALNI`, `CCVALCPF`
- **DDMs:** `BENEFIC` (150)
- **Justificativa:** são os únicos programas que escrevem em `BENEFIC` e concentram toda a validação de identidade; a fronteira coincide com a de escrita do agregado.

### Hipótese 2: Catálogo de Programas Sociais

- **Programas:** `CADPROG`
- **DDMs:** `SOCPROG` (151)
- **Justificativa:** único programa que escreve no arquivo 151, sem `CALLNAT` de entrada nem de saída; é o contexto mais isolado do sistema e o menor candidato a piloto de migração.

### Hipótese 3: Concessão e Cálculo do Benefício

- **Programas:** `BATCHPGT`, `CALCBENF`, `VALELEG`, `CALCDSCT`, `CALCCORR`
- **DDMs:** `PAYMENT` (escrita); `BENEFIC` e `SOCPROG` (leitura)
- **Justificativa:** concentra as 9 arestas `CALLNAT` da biblioteca e toda a lógica financeira; é também onde se acumulam 19 das 90 questões em aberto.

### Hipótese 4: Liquidação e Conciliação Bancária

- **Programas:** `BATCHCON`
- **DDMs:** `PAYMENT` (atualização de situação)
- **Justificativa:** único ponto de integração com sistema externo, por layout CNAB 240; a fronteira é imposta pelo contrato com o banco, não pelo código.

### Hipótese 5: Auditoria e Prestação de Contas

- **Programas:** `CCAUDIT`, `RELAUDIT`, `RELPGT`, `BATCHREL`
- **DDMs:** `AUDIT` (153); `PAYMENT` (leitura)
- **Justificativa:** `CCAUDIT` é o único gravador de auditoria e é incluído por 7 programas, o que sugere serviço transversal e não módulo de negócio.

---

## 5. Artefatos de origem

| Artefato           | Caminho                                                                           | Status                                                              |
| ------------------ | --------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| Inventário         | [inventory.md](inventory.md)                                                      | Completo — 40 arquivos, 3 subdiretórios, 11 padrões de nomenclatura |
| Regras de Negócio  | [business-rules-catalog.md](business-rules-catalog.md)                            | Completo — 176 regras, 15/15 programas lidos                        |
| Dependências       | [dependency-map.md](dependency-map.md) + [dependency-map.mmd](dependency-map.mmd) | Completo — 81 arestas, 0 referências quebradas                      |
| Questões em aberto | [mysteries-found.md](mysteries-found.md)                                          | Completo — 20 canônicos e 9 bônus, todos abertos                    |
| Glossário          | [glossary.md](glossary.md)                                                        | Completo — 74 termos, 11 em conflito, 6 ausentes do código          |

**Fora do escopo lido:** os nove membros de apoio (`LDASIFAP`, `PDACALC`, `PDAVALID`, `CCVALCPF`, `CCAUDIT`, `SUBVALCP`, `SUBVALNI`, `SIFAPJ01`, `SIFAPJ02`) foram mapeados como nós e arestas, mas não tiveram extração de regras. `SIFAP-M-15`, `SIFAP-M-08` e `SIFAP-M-11` dependem deles.

---

## 6. Aprovação do time

- Revisado por: <!-- preencher -->
- Data: <!-- preencher: AAAA-MM-DD -->
- Confiança: <!-- preencher: Alta / Média / Baixa — a leitura sugere Média -->

---

## Definição de pronto

- [x] Resumo com no máximo 5 frases.
- [x] De 3 a 5 hipóteses de fatiamento documentadas, rotuladas como hipóteses.
- [x] Todos os artefatos de origem têm status preenchido.
- [x] Questões sem validação humana listadas com evidência `path:line` e status.
- [ ] Aprovação do time preenchida.

---

### Continue lendo

| Anterior                                                                | Próximo                                                                                                      |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| [GUIDE do Estágio 1](GUIDE.md)<br/><sub>Cronograma passo a passo.</sub> | [Estágio 2 — Especificação moderna](../02-modern-spec/README.md)<br/><sub>Handoff H1 e início do EARS.</sub> |

<sub>[Voltar ao índice do kit](../README.md)</sub>
