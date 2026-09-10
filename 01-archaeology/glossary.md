# Glossário do SIFAP Legado

> **Trilha:** [Kit do Time](../README.md) › [Estágio 1](README.md) › **Glossário**

**Artefato preenchido pelo time durante o Estágio 1.** Uma tabela com todos os termos, abreviações e siglas encontrados no código Natural/Adabas — a base da linguagem ubíqua para o Estágio 2.

| Campo                  | Valor                                                                   |
| ---------------------- | ----------------------------------------------------------------------- |
| **Público-alvo**       | Todas as duplas — cada dupla contribui com os termos dos seus programas |
| **Pré-requisitos**     | Abrir os arquivos `.NSN` e `.ddm` atribuídos                            |
| **Estágio**            | Estágio 1 — Arqueologia                                                 |
| **Resultado esperado** | 30 termos ou mais, com programa de origem e status CONFIRMADO/HIPÓTESE  |

> [!NOTE]
> Guia passo a passo: [`GUIDE.md`](GUIDE.md).

---

## Por que o glossário importa

Sistemas legados têm vocabulário próprio, raramente documentado em um lugar acessível — ele vive em nomes de variável, abreviações de campo e comentários de código. Se o time do Estágio 2 não souber o que significam `DSCT`, `BENF`, `PE` ou `CTC`, vai escrever uma especificação baseada em suposições sobre esses termos.

O glossário transforma abreviações de 3 a 6 caracteres em uma linguagem ubíqua compartilhada pelo time inteiro — e dá a base para os nomes de entidades e atributos do modelo de domínio no Estágio 3.

**Erro comum:** marcar um termo como CONFIRMADO sem evidência literal no código ou na documentação histórica. Se você inferiu o significado pelo contexto, marque como HIPÓTESE e identifique quem é responsável pela validação.

---

## Como preencher

| Coluna       | O que registrar                                                                                                          |
| ------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **Termo**    | A abreviação ou sigla exatamente como aparece no código.                                                                 |
| **Expansão** | O significado completo do termo.                                                                                         |
| **Programa** | O arquivo `.NSN` ou `.ddm` onde o termo foi encontrado.                                                                  |
| **Contexto** | Explicação breve de como e onde o termo é usado.                                                                         |
| **Status**   | `CONFIRMADO` — evidência literal no código ou na documentação. `HIPÓTESE` — inferido do contexto e aguardando validação. |

### Dica de extração com o modo Ask do GitHub Copilot

Antes de usar o prompt abaixo, cole no chat o conteúdo de 2 a 3 arquivos `.NSN`:

> "Liste todas as abreviações e siglas usadas neste código Natural. Para cada uma, sugira a expansão e marque como 'CONFIRMADO' ou 'HIPÓTESE'."

Compare a sugestão do Copilot com o que você observou diretamente no código. Se coincidirem, registre como CONFIRMADO; caso contrário, registre como HIPÓTESE.

---

## Termos encontrados

**Time:** `Franklyn` · **Data:** 2026-09-10 · **Base:** 15 programas Natural, 9 membros de apoio, 4 DDMs e 3 documentos históricos.

**74 termos registrados** — 69 `CONFIRMADO` e 5 `HIPÓTESE` —, mais **11 termos em conflito** e **6 citados na documentação e ausentes do código**.

> [!IMPORTANT]
> `CONFIRMADO` significa que existe evidência literal — um comentário no código, uma observação no DDM ou uma passagem da documentação histórica. Não significa que a regra esteja correta nem que seja a vigente. Vários termos confirmados têm significados divergentes entre fontes; esses casos estão na seção [Termos em conflito](#termos-em-conflito).

### Convenções de nomenclatura da biblioteca

| #   | Termo                 | Expansão           | Programa                                | Contexto                                                                                                                               | Status     |
| --- | --------------------- | ------------------ | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| 1   | `CAD`                 | Cadastro           | `CADBENEF.NSP:L9`                       | Prefixo de programas de manutenção de registro mestre. O cabeçalho declara "BENEFICIARY REGISTRATION - ADD/UPDATE"                     | CONFIRMADO |
| 2   | `CONS`                | Consulta           | `CONSBENF.NSP:L12`                      | Prefixo de programa de leitura. Cabeçalho: "QUERY BENEFICIARY DATA - ONLINE 3270 SCREEN"                                               | CONFIRMADO |
| 3   | `CALC`                | Cálculo            | `CALCBENF.NSN:L12`                      | Prefixo dos motores de cálculo. Cabeçalho: "CALCULATE MONTHLY BENEFIT AMOUNT"                                                          | CONFIRMADO |
| 4   | `VAL`                 | Validação          | `VALELEG.NSN:L9`                        | Prefixo das rotinas de verificação. Cabeçalho: "VALIDATE BENEFICIARY ELIGIBILITY FOR A PROGRAM"                                        | CONFIRMADO |
| 5   | `REL`                 | Relatório          | `BATCHREL.NSP:L9`                       | Prefixo dos geradores de listagem. Cabeçalho: "GENERATE MONTHLY CONSOLIDATED REPORTS"                                                  | CONFIRMADO |
| 6   | `BENF` / `BENEF`      | Beneficiário       | `BENEFIC.ddm:L23`; `CADBENEF.NSP:L297`  | Sufixo mais recorrente da biblioteca. O DDM descreve "SOCIAL PROGRAM BENEFICIARY REGISTRY"; `BENF` é o código de entidade da auditoria | CONFIRMADO |
| 7   | `DEPEN`               | Dependente         | `CADDEPEN.NSP:L9`                       | Cabeçalho: "BENEFICIARY DEPENDENT REGISTRATION"                                                                                        | CONFIRMADO |
| 8   | `PROG`                | Programa social    | `CADPROG.NSP:L9`; `SOCPROG.ddm:L12`     | Cabeçalho: "SOCIAL PROGRAM REGISTRATION". O DDM descreve "SOCIAL PROGRAM REGISTRY AND ELIGIBILITY RULES"                               | CONFIRMADO |
| 9   | `PGT` / `PGTO`        | Pagamento          | `BATCHPGT.NSP:L14`; `BATCHPGT.NSP:L540` | Cabeçalho: "MONTHLY BATCH PAYMENT GENERATION". `PGTO` é o código de entidade da auditoria                                              | CONFIRMADO |
| 10  | `DSCT`                | Desconto           | `CALCDSCT.NSP:L9`                       | Cabeçalho: "CALCULATE BENEFIT DISCOUNTS AND DEDUCTIONS"                                                                                | CONFIRMADO |
| 11  | `CORR`                | Correção monetária | `CALCCORR.NSP:L8-L9`                    | Cabeçalho: "CALCULATE RETROACTIVE PAYMENT CORRECTIONS - RECALCULATE BY IPCA INDEX VARIATION"                                           | CONFIRMADO |
| 12  | `CON` (em `BATCHCON`) | Conciliação        | `BATCHCON.NSP:L9`                       | Cabeçalho: "PAYMENT RECONCILIATION AGAINST BANK RETURN"                                                                                | CONFIRMADO |
| 13  | `ELEG`                | Elegibilidade      | `VALELEG.NSN:L9`; `SOCPROG.ddm:L63`     | Verificação de aptidão do beneficiário ao programa. O DDM traz o campo `CJ COD-ELIGIBILITY`                                            | CONFIRMADO |
| 14  | `CP` (em `SUBVALCP`)  | CPF                | `SUBVALCP.NSN`; `CADBENEF.NSP:L161`     | Subprograma de validação de CPF chamado por quatro programas                                                                           | CONFIRMADO |
| 15  | `NI` (em `SUBVALNI`)  | NIS                | `SUBVALNI.NSN`; `VALDOCS.NSP:L109`      | Subprograma de validação de NIS/PIS/PASEP                                                                                              | CONFIRMADO |

### Estruturas Natural e Adabas

| #   | Termo               | Expansão                              | Programa                              | Contexto                                                                                                                       | Status     |
| --- | ------------------- | ------------------------------------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ---------- |
| 16  | `LDA`               | Local Data Area                       | `LDASIFAP.NSL`; `BATCHPGT.NSP:L30`    | Área de dados local compartilhada, invocada por `LOCAL USING`. Usada por 13 dos 24 membros — o nó mais conectado da biblioteca | CONFIRMADO |
| 17  | `PDA`               | Parameter Data Area                   | `PDACALC.NSA`; `CALCBENF.NSN:L17`     | Contrato de parâmetros entre chamador e subprograma, invocado por `PARAMETER USING`                                            | CONFIRMADO |
| 18  | `CC`                | Copycode                              | `CCAUDIT.NSC`; `CADBENEF.NSP:L418`    | Fragmento de fonte inserido em tempo de compilação por `INCLUDE`                                                               | CONFIRMADO |
| 19  | `FNR`               | File Number                           | `BENEFIC.ddm:L31`; `SOCPROG.ddm:L21`  | Identificador numérico do arquivo Adabas. `BENEFIC`=150, `SOCPROG`=151, `AUDIT`=153                                            | CONFIRMADO |
| 20  | `DDM`               | Data Definition Module                | `BENEFIC.ddm:L30`                     | Visão Natural de um arquivo Adabas, com nomes e tipos de campo                                                                 | CONFIRMADO |
| 21  | `PE`                | Grupo periódico                       | `BENEFIC.ddm:L88`; `SOCPROG.ddm:L69`  | Grupo de campos repetido. `DA GRP-DEPEND (1:10)` em `BENEFIC`; `DA GRP-CALC-BAND (1:5)` em `SOCPROG`                           | CONFIRMADO |
| 22  | `MU`                | Campo multivalorado                   | `BENEFIC.ddm:L102`; `SOCPROG.ddm:L78` | Campo com várias ocorrências. `ED NUM-PHONE (1:5)`; `EA TYPE-DISC-APPLIC (1:8)`                                                | CONFIRMADO |
| 23  | Superdescritor `S1` | Chave composta CPF + competência      | `BATCHPGT.NSP:L292-L294`              | Comentário: "USES SUPERDESCRIPTOR S1 - CPF + YEAR-MONTH-REF". Garante um pagamento por CPF e competência                       | CONFIRMADO |
| 24  | Competência         | Mês de referência no formato `AAAAMM` | `PAYMENT` view em `CALCBENF.NSN:L36`  | Campo `YEAR-MONTH-REF (N6)`, comentado como `/* YYYYMM */`. Chave temporal de todo o ciclo                                     | CONFIRMADO |

### Cadastro de beneficiário

| #   | Termo                    | Expansão                                  | Programa                             | Contexto                                                                                                           | Status     |
| --- | ------------------------ | ----------------------------------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------ | ---------- |
| 25  | `CPF`                    | Cadastro de Pessoa Física                 | `BENEFIC.ddm:L41`                    | Chave de negócio do beneficiário. Gravado como `A11` no arquivo e tratado como `N11` na maioria dos programas      | CONFIRMADO |
| 26  | `NIS` / `PIS` / `PASEP`  | Número de Identificação Social            | `BENEFIC.ddm:L54`                    | Observação do DDM: "NIS/PIS-PASEP (ADDED 2001)". Chave alternativa de busca em `CONSBENF.NSP:L157`                 | CONFIRMADO |
| 27  | `RG`                     | Registro Geral                            | `VALDOCS.NSP:L86-L94`                | Documento de identidade. Validado apenas por comprimento mínimo de cinco caracteres                                | CONFIRMADO |
| 28  | `CTPS`                   | Carteira de Trabalho e Previdência Social | `VALDOCS.NSP:L31`                    | Coletado na tela de validação de documentos e nunca verificado                                                     | CONFIRMADO |
| 29  | Situação cadastral       | Estado do beneficiário no cadastro        | `BENEFIC.ddm:L77-L78`                | Domínio literal do DDM: `A`=ativo, `S`=suspenso, `C`=cancelado, `I`=inativo, `D`=desligado                         | CONFIRMADO |
| 30  | `UF`                     | Unidade Federativa                        | `VALBENEF.NSN:L72-L100`              | Tabela de 27 siglas usada na validação cadastral                                                                   | CONFIRMADO |
| 31  | `CEP`                    | Código de Endereçamento Postal            | `BENEFIC.ddm:L67`                    | Observação do DDM: "POSTAL CODE WITHOUT HYPHEN". Recebido por `VALBENEF` e nunca validado                          | CONFIRMADO |
| 32  | Renda familiar declarada | Renda informada pelo beneficiário         | `BENEFIC.ddm:L80`                    | Campo `CH AMT-FAMILY-INCOME`, observação "DECLARED INCOME". Determina a faixa de cálculo                           | CONFIRMADO |
| 33  | Renda per capita         | Renda familiar dividida pelos membros     | `BENEFIC.ddm:L82`; `SOCPROG.ddm:L56` | Campo `CJ IND-PERCAP-INCOME`, "CALCULATED PER-CAPITA INCOME", comparado ao teto `CA MAX-PERCAP-INCOME` do programa | CONFIRMADO |

### Programa social e cálculo do benefício

| #   | Termo           | Expansão                                     | Programa                                                | Contexto                                                                                                                                 | Status     |
| --- | --------------- | -------------------------------------------- | ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| 34  | Valor base      | Valor mensal individual do programa          | `SOCPROG.ddm:L40`                                       | Campo `BA AMT-BASE-INDIVIDUAL`, "MONTHLY BASE AMOUNT PER PERSON". Ponto de partida de todo cálculo                                       | CONFIRMADO |
| 35  | Fator regional  | Multiplicador por código de região           | `CALCBENF.NSN:L97-L125`                                 | Tabela interna de 27 posições, com comentários que associam cada índice a uma sigla de UF                                                | CONFIRMADO |
| 36  | Fator familiar  | Multiplicador por quantidade de dependentes  | `CALCBENF.NSN:L206-L220`                                | Três faixas com incrementos de 0,0500, 0,0300 e 0,0200 por dependente                                                                    | CONFIRMADO |
| 37  | Faixa de renda  | Intervalo de renda com multiplicador próprio | `CALCBENF.NSN:L127-L136`                                | Cinco faixas: 300, 600, 1000, 1500 e 9999,99, com fatores de 1,0000 a 0,4000                                                             | CONFIRMADO |
| 38  | Fator de idade  | Multiplicador por faixa etária               | `CALCBENF.NSN:L237-L252`                                | 1,1500 a partir de 65 anos; 1,1000 a partir de 60; 1,0500 abaixo de 18; 1,0000 nos demais casos                                          | CONFIRMADO |
| 39  | Fator de ajuste | Multiplicador por programa social            | `SOCPROG.ddm:L50`; `CADPROG.NSP:L124`                   | Campo `BH FACTOR-ADJUST`, "FACTOR ON BASE AMOUNT (2002)"                                                                                 | CONFIRMADO |
| 40  | `FACTOR-K`      | Fator especial de correção                   | `SOCPROG.ddm:L45-L49`                                   | O próprio DDM o marca como ">>> UNDOCUMENTED <<<", inserido em ago/2008 "fulfills SENARC request". Ver `SIFAP-M-04`                      | HIPÓTESE   |
| 41  | 13º benefício   | Parcela adicional de dezembro                | `CALCBENF.NSN:L270-L281`                                | Bloco "CALCULATE 13TH SALARY - DECEMBER". Fórmula própria, distinta da mensal                                                            | CONFIRMADO |
| 42  | Abono           | Acréscimo de 15% em dezembro                 | `CALCBENF.NSN:L283-L289`                                | Comentário: "HOLIDAY BONUS - 15% ADDITION FOR TYPE 'A' PROGRAMS"                                                                         | CONFIRMADO |
| 43  | Truncamento     | Corte dos centavos sem arredondar            | `CALCBENF.NSN:L264-L266`; `BUSINESS-RULES-2012.md:L127` | Implementado multiplicando por 100 e dividindo por 100 em campo inteiro. A RN-014 confirma: "truncamento, não arredondamento matemático" | CONFIRMADO |

### Descontos e deduções

| #   | Termo               | Expansão                                  | Programa                      | Contexto                                                                                                      | Status     |
| --- | ------------------- | ----------------------------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------- | ---------- |
| 44  | Contribuição social | Dedução obrigatória sobre o bruto         | `CALCBENF.NSN:L360`           | Comentário: "BASIC DEDUCTION - 3% SOCIAL CONTRIBUTION". Ver `SIFAP-M-12` para a divergência de alíquota       | CONFIRMADO |
| 45  | Teto de desconto    | Limite de 30% do valor bruto              | `CALCDSCT.NSP:L106-L107`      | Comentário: "CALCULATE MAXIMUM DISCOUNT CAP - 30% OF GROSS". Confirma a RN-021                                | CONFIRMADO |
| 46  | Desconto judicial   | Retenção determinada por decisão judicial | `CALCDSCT.NSP:L128-L137`      | Tipo `J`, "COURT-ORDERED DISCOUNT". O comentário L136 registra "COURT-ORDERED DISCOUNT HAS NO CAP"            | CONFIRMADO |
| 47  | Pensão alimentícia  | Retenção de alimentos                     | `CALCDSCT.NSP:L138-L146`      | Tipo `P`, "ALIMONY"                                                                                           | CONFIRMADO |
| 48  | Consignação         | Empréstimo consignado em folha            | `BUSINESS-RULES-2012.md:L166` | RN-022, código 01: "Consignação voluntária — empréstimo consignado autorizado". Sem correspondência no código | HIPÓTESE   |

### Batch, pagamento e conciliação

| #   | Termo                         | Expansão                                                   | Programa                                         | Contexto                                                                                                   | Status     |
| --- | ----------------------------- | ---------------------------------------------------------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- | ---------- |
| 49  | Folha mensal                  | Ciclo de geração de pagamentos                             | `SIFAPJ01.jcl:L44`; `BATCHPGT.NSP:L15`           | "PAYROLL GENERATION (BATCHPGT)"; roda no primeiro dia útil                                                 | CONFIRMADO |
| 50  | Situação do pagamento         | Estado do pagamento no ciclo                               | `BATCHREL.NSP:L97-L101`; `RELPGT.NSP:L182-L196`  | `G`=gerado, `P`=pago, `C`=cancelado, `D`=devolvido, `E`=estornado                                          | CONFIRMADO |
| 51  | Lote                          | Agrupamento mensal de pagamentos                           | `BATCHPGT.NSP:L142-L144`                         | Comentário "BATCH CONTROL - TICKET 3315/1999". Campos `NUM-BATCH` e `SEQ-BATCH`                            | CONFIRMADO |
| 52  | Remessa bancária              | Arquivo de crédito enviado ao banco                        | `BATCHPGT.NSP:L491-L503`; `SIFAPJ01.jcl:L53-L54` | Extrato gravado em `CMWKF01`, registro tipo `3`, LRECL 240                                                 | CONFIRMADO |
| 53  | `CNAB 240`                    | Layout bancário de retorno                                 | `BATCHCON.NSP:L10`; `BATCHCON.NSP:L52`           | "BANCO DO BRASIL CNAB 240 PROCESS". Campos lidos por posição fixa                                          | CONFIRMADO |
| 54  | `IPCA`                        | Índice Nacional de Preços ao Consumidor Amplo              | `CALCCORR.NSP:L9`; `CALCCORR.NSP:L82`            | Índice usado na correção retroativa. Tabela interna com apenas 2010 a 2012 carregados                      | CONFIRMADO |
| 55  | `NATBATCH`                    | Executor batch do Natural no z/OS                          | `SIFAPJ01.jcl:L46`; `SIFAPJ02.jcl:L49`           | `EXEC PGM=NATBATCH`. Recebe comandos por `CMSYNIN`                                                         | CONFIRMADO |
| 56  | `CMSYNIN` / `CMWKF` / `CMPRT` | Nomes lógicos de entrada, arquivo de trabalho e impressora | `SIFAPJ01.jcl:L69`; `SIFAPJ02.jcl:L57`           | `CMSYNIN` traz comando e parâmetros; `CMWKFnn` são arquivos sequenciais; `CMPRTnn` são impressoras lógicas | CONFIRMADO |

### Auditoria e conformidade

| #   | Termo                 | Expansão                             | Programa                                  | Contexto                                                                                                                        | Status     |
| --- | --------------------- | ------------------------------------ | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| 57  | Trilha de auditoria   | Registro de eventos no arquivo 153   | `AUDIT.ddm:L24`; `CADBENEF.NSP:L38`       | Comentário recorrente: "AUDIT TRAIL - FILE 153 - IN-TCU 63/2010"                                                                | CONFIRMADO |
| 58  | `IN-TCU 63/2010`      | Instrução Normativa do TCU           | `CADBENEF.NSP:L38` e mais cinco programas | Norma citada como fundamento da trilha de auditoria                                                                             | CONFIRMADO |
| 59  | `IN` (código de ação) | Inclusão                             | `CADBENEF.NSP:L296-L300`                  | Gravado junto da descrição "REGISTRATION CREATED"                                                                               | CONFIRMADO |
| 60  | `AL` (código de ação) | Alteração                            | `CADBENEF.NSP:L320-L324`                  | Gravado junto da descrição "REGISTRATION UPDATED"                                                                               | CONFIRMADO |
| 61  | `DV` (código de ação) | Divergência                          | `RELAUDIT.NSP:L176-L178`                  | O relatório rotula o código como "DISCREPANCY"                                                                                  | CONFIRMADO |
| 62  | `EX` (código de ação) | Exclusão                             | `RELAUDIT.NSP:L128-L133`                  | Único uso conhecido é o filtro que **oculta** esses eventos do relatório. Nenhum programa lido grava o código. Ver `SIFAP-M-17` | HIPÓTESE   |
| 63  | `Lei 8159, art. 14`   | Norma de retenção de documentos      | `BATCHREL.NSP:L20`                        | Cabeçalho: "10-YEAR RETENTION - LAW 8159 ART 14"                                                                                | CONFIRMADO |
| 64  | `Portaria 847/2003`   | Norma sobre alteração do arquivo 150 | `BENEFIC.ddm:L25-L26`                     | "DO NOT CHANGE FIELD ORDER WITHOUT AUTHORIZATION FROM THE CGTI/MDAS TECHNICAL COMMITTEE"                                        | CONFIRMADO |

### Organizações e sistemas externos

| #   | Termo                   | Expansão                                      | Programa                                               | Contexto                                                                                                                                                    | Status     |
| --- | ----------------------- | --------------------------------------------- | ------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| 65  | `SENARC`                | Secretaria Nacional de Renda de Cidadania     | `BUSINESS-RULES-2012.md:L32`; `SOCPROG.ddm:L14-L15`    | Área que autoriza alterações no `FACTOR-K` e recebe cópia dos relatórios (`SIFAPJ02.jcl:L89`)                                                               | CONFIRMADO |
| 66  | `CGPB`                  | Coordenação-Geral de Benefícios               | `BUSINESS-RULES-2012.md:L81`                           | Autoriza exceções ao limite de dependentes pelo formulário FR-SIFAP-012                                                                                     | CONFIRMADO |
| 67  | `CGTI` / `MDAS` / `MDS` | Coordenação de TI e ministério responsável    | `BENEFIC.ddm:L25`; `SOCPROG.ddm:L30`                   | Comitê técnico que autoriza mudanças de esquema; `AE RESPONSIBLE-AGENCY` guarda o código do órgão                                                           | CONFIRMADO |
| 68  | `SIAFI`                 | Sistema Integrado de Administração Financeira | `natural-programs/README.md` (descrição de `BATCHCON`) | Citado como destino da conciliação. **Nenhuma referência a SIAFI foi encontrada no código de `BATCHCON`**, que concilia com retorno CNAB do Banco do Brasil | HIPÓTESE   |
| 69  | `CadÚnico`              | Cadastro Único para Programas Sociais         | `BUSINESS-RULES-2012.md:L186`                          | RN-016 registra integração de 2006 cujo programa "não consta no inventário oficial do SIFAP"                                                                | HIPÓTESE   |
| 70  | `SIFAPPRD`              | Biblioteca Natural de produção                | `SIFAPJ01.jcl:L70`; `BENEFIC.ddm:L29`                  | Comando `LOGON SIFAPPRD` nos dois jobs                                                                                                                      | CONFIRMADO |

### Herança técnica

| #   | Termo            | Expansão                                 | Programa                                           | Contexto                                                                                                                   | Status     |
| --- | ---------------- | ---------------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ---------- |
| 71  | `BUG2000`        | Projeto de remediação Y2K de 1998        | `CADBENEF.NSP:L226-L228`; `CALCBENF.NSN:L228-L231` | Citado em cinco programas, sempre com a equipe R.SOUZA e o ticket 1998/0442                                                | CONFIRMADO |
| 72  | Janela de século | Regra de expansão de ano de dois dígitos | `CADDEPEN.NSP:L131-L141`; `BATCHPGT.NSP:L332-L349` | Pivô 50: abaixo dele soma 2000, a partir dele soma 1900. Mantida ativa em `BATCHPGT` para registros com ano inferior a 100 | CONFIRMADO |
| 73  | Plano Verão      | Conversão monetária de 1989 a 1991       | `CALCCORR.NSP:L130-L143`                           | Bloco comentado com os fatores 2,7500 e 1,4289 e o indicador de correção `V`, referente à transição Cruzado–Cruzeiro       | CONFIRMADO |
| 74  | Modo aviso       | Validação que reporta e não bloqueia     | `CADBENEF.NSP:L190-L193`; `CADBENEF.NSP:L260-L262` | Expressão literal "WARNING MODE - DOES NOT BLOCK SAVING PENDING BENEFITS DEPARTMENT REVIEW". Ver `SIFAP-M-03`              | CONFIRMADO |

---

## Termos em conflito

Termos com **evidência literal em mais de uma fonte e significados incompatíveis**. Nenhum pode ser adotado na linguagem ubíqua do Estágio 2 sem validação humana.

| #   | Termo                                 | Fonte A                                                                           | Fonte B                                                                                                                | Mistério     |
| --- | ------------------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ------------ |
| 75  | `COD-REGION`                          | `CALCBENF.NSN:L98-L125` trata como código de UF, de 1 a 25                        | `BENEFIC.ddm:L70` define o domínio como "01-05 OR 99"                                                                  | `SIFAP-M-10` |
| 76  | Região `99`                           | `VALELEG.NSN:L121` comenta "INTERNATIONAL/DIPLOMATIC"                             | `BUSINESS-RULES-2012.md:L91` registra "é o bypass do Roberto"                                                          | `SIFAP-M-13` |
| 77  | Grau de parentesco                    | `CADDEPEN.NSP:L152` aceita `FI`, `CO`, `IR` e `OU`                                | `BENEFIC.ddm:L92-L93` documenta `FI`, `CJ`, `NT` e `TU`                                                                | `SIFAP-M-02` |
| 78  | Tipo de programa                      | `CALCBENF.NSN:L47` comenta `A`=assistência, `P`=pensão, `T`=trabalho              | `SOCPROG.ddm:L30-L31` documenta `A`=assistência, `T`=emprego, `P`=previdência                                          | —            |
| 79  | Tipo de desconto                      | `CALCDSCT.NSP:L18-L19` usa `C`, `I`, `J`, `S`, `P` e `A`                          | `SOCPROG.ddm:L78-L82` lista `IR`, `JD`, `CS`, `PA`, `EM`, `TX`, `OU` e `EX`; a RN-022 usa códigos numéricos de 01 a 05 | `SIFAP-M-12` |
| 80  | `CO` (código de ação)                 | `CONSBENF.NSP:L172-L176` grava para consulta de dado pessoal                      | `RELAUDIT.NSP:L170-L172` interpreta como conciliação                                                                   | `SIFAP-M-18` |
| 81  | `T` (tipo de pagamento)               | `CALCBENF.NSN:L41` comenta "T=THIRD"                                              | `RELPGT.NSP:L176` rotula como "THIRD". Nenhum programa grava o valor                                                   | —            |
| 82  | CPF válido                            | `VALBENEF.NSN:L229-L246` rejeita dígitos repetidos, exceto os iniciados por `000` | `VALDOCS.NSP:L137-L205` não rejeita, e `L226-L241` libera oito prefixos                                                | `SIFAP-M-15` |
| 83  | `REF` (posição 15 da tabela regional) | `CALCBENF.NSN:L113` rotula a posição como `REF`, entre `ES` e `PR`                | Nenhuma outra fonte menciona o termo. Não é sigla de unidade federativa                                                | `SIFAP-M-10` |
| 84  | Arquivo do programa social            | `CADPROG.NSP:L10` cita "FILE 155"; `CADPROG.NSP:L109` cita "FILE 151"             | `SOCPROG.ddm:L21` declara `FNR: 151`                                                                                   | —            |
| 85  | Arquivo de auditoria                  | `RELAUDIT.NSP:L13` cita "FILE 170"                                                | `AUDIT.ddm:L24` declara `FNR: 153`                                                                                     | —            |

---

## Termos citados na documentação e ausentes do código

| #   | Termo                         | Onde é citado                               | Situação                                                                                   |
| --- | ----------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------ | -------- |
| 86  | `CALCIDX`                     | `BUSINESS-RULES-2012.md:L146` (RN-019)      | Subprograma de índices de reajuste. Não existe na biblioteca                               |
| 87  | `LOGAUDIT`                    | `BUSINESS-RULES-2012.md:L101` (RN-010)      | Subprograma de auditoria. A função equivalente está em `CCAUDIT.NSC`                       |
| 88  | `VALCPF` / `VALNISN`          | `BUSINESS-RULES-2012.md:L72` (RN-001)       | Os membros reais são `SUBVALCP` e `SUBVALNI`                                               |
| 89  | `BN-QT-PROG`                  | `BUSINESS-RULES-2012.md:L193`               | Campo de contagem de programas simultâneos. Não existe no `BENEFIC.ddm`                    |
| 90  | `BN-NR-CPF-ANT`               | `BUSINESS-RULES-2012.md:L99` (RN-009)       | Campo de CPF anterior para auditoria. Não existe no `BENEFIC.ddm`                          |
| 91  | Prefixo `BN-` / `PS-` / `PG-` | `BUSINESS-RULES-2012.md:L74`, `L76`, `L107` | O documento usa prefixos de campo que **nenhum DDM adota**. Os DDMs usam nomes por extenso | HIPÓTESE |

> [!NOTE]
> Os termos 86 a 91 sugerem que o documento de 2012 descreve uma versão do sistema com convenção de nomes diferente da que está no código lido. Registrar como observação para o Estágio 2, sem presumir qual versão é anterior.

---

## Definição de pronto

- [x] 30 termos ou mais registrados — **74 termos**, mais 11 em conflito e 6 ausentes.
- [x] Todo termo tem um programa de origem com `arquivo:linha`.
- [x] Todo termo tem status CONFIRMADO ou HIPÓTESE.
- [ ] As hipóteses estão marcadas para validação com um facilitador.

---

### Continue lendo

| Anterior                                                                | Próximo                                                                                      |
| ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| [GUIDE do Estágio 1](GUIDE.md)<br/><sub>Cronograma passo a passo.</sub> | [Relatório de Descoberta](discovery-report.md)<br/><sub>Consolidação final do estágio.</sub> |

<sub>[Voltar ao índice do kit](../README.md)</sub>
