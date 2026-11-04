# Trabalho: Comparativo de Estruturas de Busca em C

**Disciplina:** Estrutura de Dados II  
**Conteúdo:** (pesquisa de dados e hashing) e (classificação e Árvore-B)  
**Grupo:** ### alunos  (MAX 4 TIMES)  
**Prazo:** 18/11/2026

## Integrantes

**Nome do time:** Elas e Ele

| Nome | Usuário do GitHub |
| --- | --- |
| Mateus Batista | Mateuz971 |
| Raika Maciel | raikamdias-lgtm |
| Raika Silva | K9s45vhk5p-hub |
| Victória Batalha | vivibatalha |
|  |  |
**Nome do time:**Os Yakuza
| Nome | Usuário do GitHub |
| --- | --- |
| Mateus Rocha| |
| Ruan Cesar | ruancesar.cln |
| Luiz Fernando | |
| Pedro rennek |  |
| Reiner|  |
| Luiz moçanbite| |
| Murilo | |
| Lucas de paula| |


## O problema

A secretaria acadêmica tem um cadastro com milhares de alunos e precisa localizar um aluno pela matrícula o mais rápido possível. Hoje o sistema procura um por um no vetor. A equipe de TI quer saber se vale a pena trocar por outra estrutura e qual.

Vocês vão implementar quatro formas de busca sobre **o mesmo cadastro** e medir quantas comparações cada uma faz:

| Estrutura | Situação |
| --- | --- |
| Busca sequencial no vetor fora de ordem | **já pronta**, serve de modelo |
| Ordenação (Inserção e Quicksort) + busca binária | vocês implementam |
| Tabela hash com encadeamento | vocês implementam |
| Índice em Árvore-B | vocês implementam |

O programa principal (`main.c`) já gera os dados, roda todas as buscas, mede o tempo, conta as comparações e **confere se as respostas de cada estrutura batem com a busca sequencial**.

## Arquivos

```
src/
  main.c             PRONTO  roda o comparativo (não alterar)
  aluno.c/.h         PRONTO  gera o cadastro e as consultas
  medicao.c/.h       PRONTO  contador de comparações, tempo, primo_ge()
  sequencial.c/.h    PRONTO  busca sequencial (modelo de contagem)
  arvoreb_pronto.c   PRONTO  criação e liberação de nós da Árvore-B
  ordenacao.c        TAREFA 1
  binaria.c          TAREFA 2
  hash.c             TAREFA 3
  arvoreb.c          TAREFA 4 (e o desafio)
RELATORIO.md         preencher com os resultados e as respostas
Makefile
```

Os dados são sempre os mesmos em qualquer computador: o programa usa um gerador de números aleatórios próprio, com semente fixa. As matrículas têm o formato ano de ingresso + 5 dígitos (ex.: `202304817`). Metade das 10.000 consultas procura matrículas que existem e metade procura matrículas que não existem.

## Como compilar e rodar

Com `make` (Linux, Mac, ou Windows com MinGW/MSYS2):

```bash
make
./comparativo                 # N = 100000 alunos, t = 3, alfa = 1.0
./comparativo 10000           # N = 10000
./comparativo 100000 50       # Árvore-B com t = 50
./comparativo 100000 3 2.0    # hash com fator de carga 2
```

Sem `make`:

```bash
gcc -Wall -Wextra -O2 -std=c99 -o comparativo src/*.c
```

No Windows, rode `comparativo.exe` no terminal (PowerShell ou CMD). Se usar Code::Blocks ou Dev-C++, adicione **todos** os arquivos `.c` da pasta `src` ao projeto.

Logo no início, com as tarefas ainda vazias, o programa já compila e roda: ele mostra a busca sequencial e avisa o que ainda não foi implementado.

## Tarefas

**Regra de ouro:** cada estrutura soma 1 na variável global `comparacoes` a cada comparação de chave, exatamente como indicado abaixo. É isso que permite comparar as estruturas de forma justa e conferir os números.

### Tarefa 1: Ordenação (`ordenacao.c`)

1. `ordena_insercao`: Insertion Sort. Some 1 a cada comparação entre duas matrículas.
2. `ordena_quick`: Quicksort, com pivô no elemento do meio. Some 1 a cada comparação de uma matrícula com o pivô.

Ordene o `Aluno` inteiro (struct), não só a matrícula. O programa confere se o vetor ficou ordenado. A Inserção só é testada com N até 20.000, porque com 100.000 ela demora demais.

### Tarefa 2: Busca binária (`binaria.c`)

Busca no vetor ordenado pela Tarefa 1. Some 1 a cada elemento do meio examinado. Use `meio = ini + (fim - ini) / 2`.

### Tarefa 3: Tabela hash (`hash.c`)

Tabela com tratamento de colisão por encadeamento (listas ligadas):

- `hash_funcao`: método da divisão, `chave % m`. O tamanho `m` já vem como número primo.
- `hash_insere`: insere o nó no **início** da lista. Cada nó guarda a matrícula e a posição do aluno no vetor.
- `hash_busca`: some 1 a cada nó visitado na lista. Devolva a posição do aluno ou -1.
- `hash_maior_lista` e `hash_libera`.

### Tarefa 4: Árvore-B (`arvoreb.c`)

O mesmo algoritmo do laboratório (inserção com divisão preventiva), agora com o grau mínimo `t` guardado em `a->t`:

- `arvb_busca`: some 1 a cada **nó** visitado (cada nó seria uma leitura de disco).
- `divide_filho`, `insere_nao_cheio`, `arvb_insere` (sem chaves repetidas) e `arvb_altura`.

Junto de cada chave, guarde a posição do aluno no vetor (`pos`). Use `arvb_novo_no(a, folha)` para criar nós.

### Desafio (bônus): consulta por faixa (`arvoreb.c`)

Implemente `arvb_conta_faixa(a, ini, fim)`, que conta quantas matrículas estão no intervalo, visitando **só** as subárvores que podem ter chaves do intervalo. O programa usa a função para contar os ingressantes de 2024 e confere o resultado.

## Confira seus resultados

Rodando `./comparativo 10000`, as colunas **Comparacoes/busca** e **Resultados** devem dar exatamente:

| Estrutura | Comparações por busca | Resultados |
| --- | --- | --- |
| Sequencial | 7465.45 | 10000/10000 OK |
| Binária | 12.87 | 10000/10000 OK |
| Hash (alfa = 1.00) | 1.24 | 10000/10000 OK |
| Árvore-B (t = 3) | 6.85 | 10000/10000 OK |

Se o resultado for OK mas a contagem for diferente, revise onde vocês somam em `comparacoes`. Os tempos variam de um computador para outro; isso é normal. As comparações do Quicksort também podem variar um pouco, conforme o jeito de particionar.

## Experimentos e relatório

Com tudo funcionando, rodem os experimentos e preencham o arquivo `RELATORIO.md`:

| Experimento | Comandos |
| --- | --- |
| E1. Crescimento de N | `./comparativo 10000`, `50000`, `100000` e `200000` |
| E2. Ordenação | `./comparativo 10000` (Inserção × Quick) e `./comparativo 100000` (Quick) |
| E3. Grau da Árvore-B | `./comparativo 100000 2`, `100000 3`, `100000 10`, `100000 50` |
| E4. Fator de carga da hash | `./comparativo 100000 3 0.5`, `... 3 1`, `... 3 2`, `... 3 5` |

## Entrega no GitHub

A entrega é feita por **Pull Request** neste repositório: cada grupo trabalha em uma cópia (fork) e, no final, envia o trabalho para cá. O professor corrige pelo Pull Request, sem juntar o código de um grupo com o de outro.

1. **Um** integrante, logado no GitHub, clica em **Fork** (canto superior direito desta página) e depois em **Create fork**. O fork é a cópia do grupo.
2. No fork, vá em **Settings → Collaborators → Add people** e adicione os colegas de grupo.
3. Cada integrante aceita o convite (chega por e-mail) e clona o fork: `git clone <link-do-fork>`.
4. Dividam as tarefas. **Cada integrante deve ter pelo menos 3 commits** com mensagens claras, por exemplo `Tarefa 3: insercao e busca na hash`.
5. Preencham a tabela de integrantes no topo deste README e o `RELATORIO.md`.
6. Não enviem o executável (`comparativo` ou `comparativo.exe`): o `.gitignore` já cuida disso.
7. **Para entregar:** no fork, clique em **Contribute → Open pull request**. No título, coloque o nome do grupo e dos integrantes, preencha o modelo que aparece e clique em **Create pull request**.
8. Abram só **um** Pull Request por grupo. Até o prazo, cada novo `git push` no fork atualiza o mesmo Pull Request automaticamente. Vale o que estiver no Pull Request na data de entrega.

Comandos do dia a dia:

```bash
git pull                                   # pega o que os colegas enviaram
git add src/hash.c
git commit -m "Tarefa 3: insercao e busca na hash"
git push                                   # envia para o GitHub
```


O código precisa compilar com `make` sem erros. Trabalho que não compila recebe nota apenas pelo relatório.
