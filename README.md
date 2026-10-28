[README_ACOES_PARCIAIS.md](https://github.com/user-attachments/files/34102960/README_ACOES_PARCIAIS.md)
# Gerador de Ações Parciais

Ferramenta, em um único arquivo HTML, que **constrói exemplos de ações parciais de grupos cíclicos Cₙ sobre conjuntos finitos X** e entrega o resultado como um **documento LaTeX completo e compilável**: construção, domínios, bijeções, verificação dos axiomas e dos critérios de não-trivialidade.

Basta informar a ordem n do grupo e a cardinalidade m = |X|. A página gera o `.tex`, que pode ser copiado ou baixado e compilado no Overleaf, TeXstudio, TeXmaker etc. Tudo roda no navegador, sem instalação e sem internet.

🔗 **Acesse online:** <https://lbemm3.github.io/gerador-de-acoes-parciais/>

---

## Sumário

- [Funcionalidades](#funcionalidades)
- [Como usar](#como-usar)
- [Parâmetros](#parâmetros)
- [Fundamentação matemática](#fundamentação-matemática)
- [Estratégias de construção](#estratégias-de-construção)
- [O documento LaTeX gerado](#o-documento-latex-gerado)
- [Exemplos](#exemplos)
- [Estrutura do código](#estrutura-do-código)
- [Limitações conhecidas](#limitações-conhecidas)
- [Executando localmente](#executando-localmente)
- [Publicação via GitHub Pages](#publicação-via-github-pages)
- [Autoria e licença](#autoria-e-licença)

---

## Funcionalidades

- **Geração automática** de uma ação parcial de Cₙ = ⟨g | gⁿ = e⟩ sobre um conjunto X com |X| = m.
- **Duas estratégias de construção**: A (Rotação Mínima) e B (Quase-Cosets), com **escolha automática** ou manual.
- **Documento LaTeX completo**, com preâmbulo, título e seções numeradas.
- **Cálculo dos domínios** D_h = X ∩ β_h(X) para todo h = gᵏ, com tabela de tamanhos.
- **Tabelas das bijeções** α_h entre os domínios.
- **Verificações automáticas** da quase-invariância de X e dos critérios de não-trivialidade NT1, NT2 e NT4.
- **Alvo opcional** para |D_g|, comparado com o valor obtido.
- **Copiar** o LaTeX para a área de transferência ou **baixar** como `acao_parcial.tex`.
- **Pré-visualização** do código gerado em uma caixa de texto na própria página.
- **Layout responsivo**, com painel de parâmetros fixo, que se reorganiza em telas pequenas.
- **Sem dependências**: HTML, CSS e JavaScript puro, que funciona offline.

## Como usar

1. Abra a página (ou o `index.html` no navegador).
2. Informe **n** e **m** (inteiros ≥ 2).
3. Se quiser, informe o alvo para |D_g| e escolha a estratégia (por padrão, *Automática*).
4. Clique em **▶ Gerar LaTeX**.
5. Use **⎘ Copiar LaTeX** ou **↓ Download .tex**.
6. Compile o arquivo com pdfLaTeX (por exemplo, no [Overleaf](https://www.overleaf.com)).

## Parâmetros

| Campo | Obrigatório | Significado | Restrição |
|---|---|---|---|
| **n** | Sim | Ordem do grupo cíclico Cₙ = ⟨g ∣ gⁿ = e⟩ | Inteiro ≥ 2 |
| **m** | Sim | Cardinalidade de X, o conjunto sobre o qual Cₙ age parcialmente | Inteiro ≥ 2 |
| **\|D_g\|** | Não | Alvo para o tamanho do domínio do gerador | 1 ≤ \|D_g\| ≤ m − 1 |
| **Estratégia** | — | *Automática*, *A — Rotação Mínima* ou *B — Quase-Cosets* | — |

O valor de |D_g| é apenas um **alvo de comparação**: o documento informa se foi atingido, mas a construção não é ajustada para atingi-lo (veja [Limitações](#limitações-conhecidas)).

## Fundamentação matemática

### Ação parcial

Uma ação parcial de um grupo G sobre um conjunto X é uma família de bijeções

```
α_h : D_{h⁻¹} → D_h        (h ∈ G),  com D_h ⊆ X,
```

tal que α_e = id_X (com D_e = X), α_{h⁻¹} = (α_h)⁻¹ e α_h ∘ α_k é restrição de α_{hk}. Os axiomas são rotulados **A1–A3** no documento gerado.

### Construção por restrição de uma ação global

A ferramenta usa o método clássico: parte de uma **ação global** β de Cₙ sobre um conjunto maior Y = {1, …, L} e escolhe um subconjunto X ⊆ Y que **não** é invariante. Então

```
D_h = X ∩ β_h(X)        e        α_h = β_h restrita a D_{h⁻¹}
```

definem uma ação parcial de Cₙ sobre X. O gerador g age por uma translação cíclica

```
β_g(i) = ((i − 1 + s) mod L) + 1
```

com passo s e módulo L escolhidos de modo que a ordem de β_g seja L / mdc(L, s) = n.

### Não-trivialidade

Uma ação parcial é considerada **não-trivial** quando não se reduz a uma ação global. O gerador verifica, por cálculo direto, os critérios:

| Critério | Condição verificada |
|---|---|
| **NT1** | 0 < \|D_g\| < m |
| **NT2** | Existem h ≠ k com D_h ≠ D_k |
| **NT4** | Existe h com D_h ⊊ X |
| **NT6** | L − n < m (L mínimo) |

Além disso, a seção de quase-invariância confere se X ∩ β_g(X) ≠ ∅, X ⊄ β_g(X) e β_g(X) ⊄ X.

## Estratégias de construção

### A — Rotação Mínima

```
q = ⌈m / n⌉        L = n · q        β_g(i) = i + q  (mod L)
```

- A ordem de β_g é L / mdc(L, q) = n.
- X é formado por duas partes: {1, …, a}, com a = ⌊m/2⌋, e um bloco de b = m − a elementos deslocado por d + 1 posições, onde d é o menor t em [1, q − 1] com mdc(t, q) = 1 (ou d = 1 se q = 1).
- O bloco é "enrolado" módulo L quando ultrapassa L.

### B — Quase-Cosets

Requer d = mdc(n, m) > 1.

```
k = n / d        ℓ = ⌈m / n⌉        L = n · ℓ        β_g(i) = i + ℓ  (mod L)
X = {1, …, m}
```

- O subgrupo de referência é H = ⟨g^(n/d)⟩, com d cosets.
- A ordem de β_g é L / mdc(L, ℓ) = n.

### Escolha automática

A estratégia **B** é usada quando d = mdc(n, m) > 1, m/d ≥ 2 e m ≥ 4. Em qualquer outro caso, usa-se a **A**. Se a estratégia B for pedida manualmente com mdc(n, m) = 1, o gerador usa a A.

## O documento LaTeX gerado

Classe `article` (12pt, A4), com os pacotes `inputenc`, `fontenc`, `babel` (brazil), `amsmath`, `amssymb`, `amsthm`, `geometry`, `array`, `booktabs` e `hyperref`. As seções são:

| Seção | Conteúdo |
|---|---|
| **1. Objetivo** | Enunciado do problema e estratégia usada. |
| **2. Plano** | Roteiro da construção em cinco passos. |
| **3. Dados de Entrada** | n, m, alvo de \|D_g\|, estratégia e parâmetros (q, d, L ou d, k, ℓ, L). |
| **4. Ação Global β** | Definição de β_g e verificação de que sua ordem é n. |
| **5. Conjunto X e Domínio D_g** | X, β_g(X), D_g = X ∩ β_g(X) e os testes de quase-invariância. |
| **6. Domínios D_h** | Tabela com D_{gᵏ} e \|D_{gᵏ}\| para k = 0, …, n − 1. |
| **7. Bijeções α_h** | Tabelas de α_{gᵏ} : D_{g^(n−k)} → D_{gᵏ} para k = 1, 2, 3. |
| **8. Axiomas A1–A3** | Verificação dos axiomas. |
| **9. Não-Trivialidade** | NT1, NT2, NT4 e NT6. |
| **10. Alvo \|D_g\|** | Aparece só se o alvo foi informado; compara alvo e valor obtido. |
| **Hipóteses** | Registro de que a construção seguiu a estratégia escolhida. |
| **Conclusão** | Resumo final. |

Conjuntos com mais de 20 elementos são abreviados no texto (os 15 primeiros, reticências e o último).

## Exemplos

Resultados obtidos executando a lógica da própria página (estratégia automática):

| n | m | Estratégia | L | X | D_g = X ∩ β_g(X) |
|---|---|---|---|---|---|
| **6** | **9** | B | 12 | {1, …, 9} | {3, 4, 5, 6, 7, 8, 9} (7 elementos) |
| **4** | **6** | B | 8 | {1, …, 6} | {3, 4, 5, 6} |
| **3** | **5** | A | 6 | {1, 2, 4, 5, 6} | {1, 2, 4, 6} |
| **5** | **7** | A | 10 | {1, 2, 3, 5, 6, 7, 8} | {3, 5, 7, 8} |
| **2** | **3** | A | 4 | {1, 3, 4} | {1, 3} |

**Exemplo detalhado: n = 6, m = 9.** Como mdc(6, 9) = 3, a estratégia é B, com ℓ = 2 e L = 12, e β_g(i) = i + 2 (mod 12). Os domínios resultantes são:

| h | D_h | \|D_h\| |
|---|---|---|
| g⁰ | {1, …, 9} | 9 |
| g¹ | {3, 4, 5, 6, 7, 8, 9} | 7 |
| g² | {1, 5, 6, 7, 8, 9} | 6 |
| g³ | {1, 2, 3, 7, 8, 9} | 6 |
| g⁴ | {1, 2, 3, 4, 5, 9} | 6 |
| g⁵ | {1, 2, 3, 4, 5, 6, 7} | 7 |

Os critérios NT1, NT2, NT4 e NT6 são satisfeitos. Informando o alvo |D_g| = 7, o documento registra "Atingido".

## Estrutura do código

Tudo está em `index.html`:

| Trecho | Conteúdo |
|---|---|
| **HTML + CSS** | Painel de parâmetros, caixa de saída do LaTeX, mensagens de erro e de sucesso. |
| `gcd(a, b)` | Máximo divisor comum. |
| `mkA(n, m)` | Constrói a estratégia A: q, L, d, X e a função β. |
| `mkB(n, m)` | Constrói a estratégia B (retorna `null` se mdc(n, m) = 1). |
| `dominios(n, X, fn)` | Calcula D_{gᵏ} = X ∩ β_{gᵏ}(X) para todo k. |
| `fs(arr)` | Formata um conjunto em LaTeX, abreviando os longos. |
| `tabela(...)` | Gera a tabela LaTeX de uma bijeção α_h. |
| `preambulo(n, m)` | Gera o preâmbulo do documento. |
| `montar(n, m, con, dg)` | Monta o documento LaTeX completo. |
| **Eventos** | Botões *Gerar*, *Copiar* e *Download*, com validação das entradas. |

## Limitações conhecidas

Estas observações vêm da execução da lógica sobre os pares 2 ≤ n ≤ 25 e 2 ≤ m ≤ 40:

- **Quando n divide m** (por exemplo, n = 3 e m = 9), a ferramenta **não produz uma ação parcial não-trivial**: X coincide com β_g(X) e o domínio D_g é todo X. O documento marca "Falha" em NT1, NT2 e NT4. Para m = n, o conjunto X gerado pela estratégia A ainda contém elementos repetidos.
- **Quando m = 2 e n ≥ 4**, D_g resulta vazio (NT1 falha).
- A **Conclusão** do documento é um texto fixo ("construída, verificada e não-trivial") e **não é ajustada** quando algum critério NT falha. Confira sempre a seção 9.
- Os axiomas **A1–A3** e o critério **NT6** são declarados "por construção" e não são recalculados.
- O título da seção 9 menciona NT1–NT6, mas a lista traz apenas NT1, NT2, NT4 e NT6.
- O valor de |D_g| informado é **apenas um alvo**: se não for atingido, a construção não é modificada.
- A seção 7 traz as bijeções α_{gᵏ} apenas para k = 1, 2, 3; as demais ficam indicadas como "por simetria".
- O botão **Copiar** usa `document.execCommand('copy')`, que pode não funcionar em alguns navegadores. Nesse caso, use o **Download**.
- Valores muito grandes de n e m geram documentos longos, e os cálculos fazem cerca de n·m operações.
- Ferramenta com **fins didáticos e de exploração**: o resultado deve ser conferido antes de ser usado em um texto formal.

## Executando localmente

Não há instalação nem build. Baixe o arquivo e abra no navegador (funciona offline):

```bash
git clone https://github.com/lbemm3/acoes-parciais.git
cd acoes-parciais
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Ou use um servidor local:

```bash
python3 -m http.server 8000
# acesse http://localhost:8000/
```

## Publicação via GitHub Pages

O arquivo deve se chamar `index.html` para a página abrir direto na raiz do endereço.

1. No repositório, vá em **Settings → Pages**.
2. Em *Build and deployment*, selecione **Deploy from a branch**.
3. Escolha a branch principal (`main`) e a pasta `/ (root)`, e clique em **Save**.
4. Após alguns minutos, a ferramenta estará em <https://lbemm3.github.io/gerador-de-acoes-parciais/>.

## Autoria e licença

Desenvolvido por **Laerte Bemm** (Departamento de Matemática, Universidade Estadual de Maringá – UEM).

Licença: defina a licença do repositório (por exemplo, MIT) e inclua o arquivo `LICENSE`.
