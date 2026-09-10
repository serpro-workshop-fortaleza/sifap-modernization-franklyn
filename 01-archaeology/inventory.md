# Inventário do Legado — Time `Franklyn`

> **Trilha:** [Kit do Time](../README.md) › [Estágio 1](README.md) › **Inventário**

**Primeiro artefato do Estágio 1.** Varra a estrutura e conte os arquivos sem abrir nenhum programa — use apenas os nomes de arquivo e a estrutura de pastas.

| Campo                  | Valor                                                                                     |
| ---------------------- | ----------------------------------------------------------------------------------------- |
| **Público-alvo**       | Dupla responsável pela varredura inicial                                                  |
| **Pré-requisitos**     | Acesso ao diretório `legacy-sifap/`                                                       |
| **Estágio**            | Estágio 1 — Arqueologia, Passo 1                                                          |
| **Resultado esperado** | Contagens corretas, padrões de nomenclatura identificados e 3 itens estranhos sinalizados |

> [!NOTE]
> Monte este inventário sem abrir nenhum programa. Trabalhe apenas com nomes de arquivo e estrutura de pastas. Ele será revisado à medida que o time extrai regras, mapeia dependências e registra mistérios.

**Data:** 2026-09-10
**Dupla responsável:** <!-- preencher -->
**Caminho varrido:** `01-archaeology/legacy-sifap/`

> [!IMPORTANT]
> Esta é a **primeira passada**, feita somente com nomes de arquivo e estrutura de pastas. Nenhum programa foi aberto. Toda hipótese aqui é provisória e deve ser revisada durante `/extract-business-rules` e `/map-dependencies`.

---

## Estrutura de pastas

**3 subdiretórios, 40 arquivos, profundidade máxima de 1 nível abaixo da raiz legada.**

```text
01-archaeology/legacy-sifap/
├── README.md
├── HOW-TO-READ-NATURAL.md
├── adabas-ddms/            (6 arquivos)
│   ├── README.md
│   ├── AUDIT.ddm
│   ├── BENEFIC.ddm
│   ├── PAYMENT.ddm
│   ├── SOCPROG.ddm
│   └── FDT-150-BENEFICIARY.txt
├── legacy-docs/            (7 arquivos)
│   ├── README.md
│   ├── ORIGINAL-ARCHITECTURE-1997.md    + .docx
│   ├── TECHNICAL-MANUAL-SIFAP-2008.md   + .docx
│   └── BUSINESS-RULES-2012.md           + .docx
└── natural-programs/       (25 arquivos)
    ├── README.md
    ├── BATCHCON.NSP   BATCHPGT.NSP   BATCHREL.NSP
    ├── CADBENEF.NSP   CADDEPEN.NSP   CADPROG.NSP
    ├── CALCCORR.NSP   CALCDSCT.NSP   CONSBENF.NSP
    ├── RELAUDIT.NSP   RELPGT.NSP     VALDOCS.NSP
    ├── CALCBENF.NSN   SUBVALCP.NSN   SUBVALNI.NSN
    ├── VALBENEF.NSN   VALELEG.NSN
    ├── CCAUDIT.NSC    CCVALCPF.NSC
    ├── PDACALC.NSA    PDAVALID.NSA
    ├── LDASIFAP.NSL
    └── SIFAPJ01.jcl   SIFAPJ02.jcl
```

Verificação independente:

```bash
find 01-archaeology/legacy-sifap -type d | wc -l   # 4 (raiz + 3)
find 01-archaeology/legacy-sifap -type f | wc -l   # 40
```

---

## Contagem de arquivos por tipo

| Extensão  | Contagem | Finalidade provável                                               |
| --------- | -------- | ----------------------------------------------------------------- |
| `.NSP`    | 12       | Programa Natural (unidade executável, ponto de entrada)           |
| `.NSN`    | 5        | Subprograma Natural (invocado por `CALLNAT`)                      |
| `.ddm`    | 4        | Data Definition Module — visão Natural de um arquivo Adabas       |
| `.NSC`    | 2        | Copycode — fragmento de fonte incluído por `INCLUDE`              |
| `.NSA`    | 2        | Parameter Data Area (PDA) — contrato de parâmetros de subprograma |
| `.jcl`    | 2        | Job Control Language — agendamento/execução batch no mainframe    |
| `.NSL`    | 1        | Local Data Area (LDA) — estrutura de dados local compartilhada    |
| `.txt`    | 1        | Listagem FDT (Field Definition Table) do Adabas                   |
| `.md`     | 8        | Documentação do kit + transcrições de documentos históricos       |
| `.docx`   | 3        | Documentos históricos originais (binários)                        |
| **Total** | **40**   |                                                                   |

> [!NOTE]
> As finalidades acima vêm de conhecimento geral de Natural/Adabas sobre extensões, **não** do conteúdo destes arquivos.

Fontes legadas propriamente ditas: **22 membros Natural + 2 JCL = 24 arquivos** em `natural-programs/`, mais **5 artefatos de dados** em `adabas-ddms/`.

---

## Padrões da convenção de nomes

Agrupamento por prefixo, apenas sobre os 24 arquivos de `natural-programs/` (exclui `README.md`).

| Prefixo  | Contagem | Arquivos                                       | Hipótese de domínio                                                                                                         |
| -------- | -------- | ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `BATCH`  | 3        | `BATCHCON.NSP`, `BATCHPGT.NSP`, `BATCHREL.NSP` | Pontos de entrada de processamento batch; os sufixos parecem qualificar o tipo de rodada (CON/PGT/REL) — **não confirmado** |
| `CAD`    | 3        | `CADBENEF.NSP`, `CADDEPEN.NSP`, `CADPROG.NSP`  | "Cadastro" — telas/programas de manutenção de registros mestres                                                             |
| `CALC`   | 3        | `CALCBENF.NSN`, `CALCCORR.NSP`, `CALCDSCT.NSP` | "Cálculo" — motores de cálculo; mistura programa e subprograma                                                              |
| `VAL`    | 3        | `VALBENEF.NSN`, `VALDOCS.NSP`, `VALELEG.NSN`   | "Validação" — regras de aceitação/consistência                                                                              |
| `SUBVAL` | 2        | `SUBVALCP.NSN`, `SUBVALNI.NSN`                 | Subvalidações reutilizáveis; prefixo `SUB` sugere chamada por outros validadores — **não confirmado**                       |
| `REL`    | 2        | `RELAUDIT.NSP`, `RELPGT.NSP`                   | "Relatório" — geração de listagens                                                                                          |
| `CC`     | 2        | `CCAUDIT.NSC`, `CCVALCPF.NSC`                  | Convenção de copycode: prefixo `CC` = _copycode_, alinhado à extensão `.NSC`                                                |
| `PDA`    | 2        | `PDACALC.NSA`, `PDAVALID.NSA`                  | Convenção de PDA: prefixo `PDA` = _Parameter Data Area_, alinhado à extensão `.NSA`                                         |
| `SIFAPJ` | 2        | `SIFAPJ01.jcl`, `SIFAPJ02.jcl`                 | Jobs numerados do sistema SIFAP; a numeração sugere ordem de execução — **não confirmado**                                  |
| `LDA`    | 1        | `LDASIFAP.NSL`                                 | Convenção de LDA: prefixo `LDA` = _Local Data Area_                                                                         |
| `CONS`   | 1        | `CONSBENF.NSP`                                 | "Consulta" — programa de consulta somente leitura                                                                           |

Observações estruturais (sem abrir arquivos):

- O sufixo `BENEF`/`BENF` reaparece em quatro famílias diferentes (`CADBENEF`, `VALBENEF`, `CALCBENF`, `CONSBENF`), o que sugere um **agregado central** do domínio.
- Os nomes seguem o padrão mainframe de **8 caracteres**, o que força abreviações (`BENF` vs `BENEF`, `DSCT`, `CORR`, `PGT`).
- Prefixo indica **verbo/função** (CAD, VAL, CALC, CONS, REL); sufixo indica **entidade/assunto** (BENEF, DEPEN, PROG, DOCS, AUDIT, PGT).

---

## Itens estranhos (top 3)

> [!NOTE]
> A varredura foi feita por nome e estrutura. O ranking por **tamanho de arquivo** ainda não foi verificado — rode `ls -lS` na pasta para completar esta seção.

| #   | Caminho do arquivo                                 | O que o torna estranho                                                                                                                                                                                                                                      | Investigação sugerida                                                                                                                                                          |
| --- | -------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | `legacy-sifap/adabas-ddms/FDT-150-BENEFICIARY.txt` | Único `.txt` do repositório legado e único artefato de dados que **não** é `.ddm`. Traz um número no nome (`150`, provável FNR do arquivo Adabas) e nome por extenso, quebrando o padrão de 8 caracteres dos DDMs. Existe FDT para apenas 1 dos 4 arquivos. | Comparar campo a campo o FDT com `BENEFIC.ddm`: o DDM pode expor menos campos que o FDT (campos ocultos). Confirmar se `150` é o FNR e procurar os FNRs dos outros 3 arquivos. |
| 2   | `legacy-sifap/natural-programs/LDASIFAP.NSL`       | Único `.NSL` e único membro cujo nome carrega o **nome do sistema inteiro**. Sendo a única Local Data Area, é candidata a estrutura compartilhada por vários programas — um ponto único de acoplamento.                                                     | Rastrear quais programas fazem `USING LDASIFAP`. Se for a maioria, é o "contrato implícito" do sistema e precisa ser mapeada antes de qualquer refatoração.                    |
| 3   | `legacy-sifap/legacy-docs/*.docx` (3 arquivos)     | Cada documento histórico existe em **duas cópias**: `.docx` (original binário) e `.md` (transcrição). Documentação duplicada em formatos diferentes tende a divergir; as datas (1997, 2008, 2012) mostram 15 anos sem atualização até hoje.                 | Verificar se `.md` e `.docx` batem. Tratar os documentos como **hipóteses datadas**, nunca como verdade: o código de 2026 é a fonte autoritativa.                              |

Menções honrosas:

- `CCVALCPF.NSC` — um copycode com nome de validador. Lógica de validação dentro de um `INCLUDE` é copiada em cada chamador, o que dificulta alterações centralizadas.
- `SUBVALNI.NSN` / `SUBVALCP.NSN` — os sufixos `NI` e `CP` não são decodificáveis a partir do nome. **Desconhecido — investigar na próxima etapa.**

---

## Ordem de leitura proposta

> Hipótese de sequência. Ela vai mudar quando o time rastrear as dependências reais em `/map-dependencies`.

1. **Estruturas de dados primeiro** — `adabas-ddms/*.ddm` (4) e depois `FDT-150-BENEFICIARY.txt`. Entender o modelo de dados antes da lógica evita interpretar código sem saber o que os campos significam.
2. **Contratos compartilhados** — `LDASIFAP.NSL`, `PDACALC.NSA`, `PDAVALID.NSA`. As áreas de dados definem as interfaces entre programas; lê-las cedo torna as chamadas `CALLNAT` legíveis.
3. **Pontos de entrada batch** — `SIFAPJ01.jcl`, `SIFAPJ02.jcl` e então `BATCHCON.NSP`, `BATCHPGT.NSP`, `BATCHREL.NSP`. O JCL revela a ordem de execução real; os programas `BATCH*` são a raiz das cadeias de chamada.
4. **Núcleo de cálculo** — `CALCBENF.NSN`, `CALCCORR.NSP`, `CALCDSCT.NSP`. Provável concentração de regras de negócio financeiras.
5. **Cadeia de validação** — `VALELEG.NSN`, `VALBENEF.NSN`, `VALDOCS.NSP`, `SUBVALCP.NSN`, `SUBVALNI.NSN` + copycodes `CCVALCPF.NSC`, `CCAUDIT.NSC`.
6. **Cadastro e consulta** — `CADBENEF.NSP`, `CADDEPEN.NSP`, `CADPROG.NSP`, `CONSBENF.NSP`.
7. **Relatórios por último** — `RELAUDIT.NSP`, `RELPGT.NSP`. Costumam ser consumidores de dados, não fontes de regra.

Critério de priorização: (a) dados antes de lógica; (b) `.jcl` e `BATCH*` como pontos de entrada prováveis; (c) sufixo `BENEF`/`BENF`, o mais recorrente do repositório, como provável nó mais conectado do grafo de chamadas.

---

## Perguntas em aberto geradas nesta varredura

| #   | Pergunta                                                                                    | Evidência (nome/estrutura)                                               |
| --- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| 1   | O que significam os sufixos `NI` e `CP` em `SUBVALNI` / `SUBVALCP`?                         | `natural-programs/SUBVALNI.NSN`, `natural-programs/SUBVALCP.NSN`         |
| 2   | Por que existe FDT para apenas 1 dos 4 arquivos Adabas?                                     | `adabas-ddms/FDT-150-BENEFICIARY.txt` vs 4 `.ddm`                        |
| 3   | `SIFAPJ01` e `SIFAPJ02` executam em sequência ou são jobs independentes?                    | `natural-programs/SIFAPJ01.jcl`, `SIFAPJ02.jcl`                          |
| 4   | `PGT` (em `BATCHPGT`, `RELPGT`) é a mesma abreviação de "pagamento" ligada a `PAYMENT.ddm`? | `natural-programs/BATCHPGT.NSP`, `RELPGT.NSP`, `adabas-ddms/PAYMENT.ddm` |

> Migre para [`mysteries-found.md`](mysteries-found.md) apenas as que sobreviverem à leitura dos arquivos, com evidência `path:line`.

---

## Definição de pronto

- [x] O inventário existe com contagens corretas (40 arquivos, 3 subdiretórios; verificável com `find`).
- [x] 3 padrões de nomenclatura ou mais identificados (11 prefixos mapeados).
- [x] 3 itens estranhos sinalizados com caminho e motivo.
- [x] Ordem de leitura proposta e justificada.
- [ ] Preencher time e dupla responsável no cabeçalho.
- [ ] Confirmar o ranking por tamanho de arquivo com `ls -lS`.

---

### Continue lendo

| Anterior                                                                | Próximo                                                                                      |
| ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| [GUIDE do Estágio 1](GUIDE.md)<br/><sub>Cronograma passo a passo.</sub> | [Catálogo de Regras](business-rules-catalog.md)<br/><sub>Passo 2 — extração de regras.</sub> |

<sub>[Voltar ao índice do kit](../README.md)</sub>
