---
description: >-
  Este documento pretende desmistificar o utilitário de linha de comando Bourn
  Again Shell conhecido como bash. Bash, não é somente um interpretador de
  comandos, mas é também uma Linguagem de script.
cover: >-
  https://images.unsplash.com/photo-1629654297299-c8506221ca97?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxiYXNofGVufDB8fHx8MTY5MDU3MDY4MXww&ixlib=rb-4.0.3&q=85
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 😍 Descomplicando o Bash Script



## Tabela de Conteúdo (Table of Contents)

* O shell unix, tem ambos os papeis, ele é tanto um interpretador de comandos quanto uma linguagem de programação. E essas são a formas `interativa` e  não `interativa`.
  * Na modo `interativo`, o shell aceita comandos do usuário
  * No modo `não interativo`, o shell executa comandos que estão declarados dentro de arquivos.
* O shell permite a execução de comandos GNU de duas formas.
  * De forma `síncrona`: na forma síncrona o shell espera por um comando síncrono ser completado antes que de aceitar mais entradas do usuário.
  * De forma `assíncrona`: um comando executado de forma assíncrona, continua sendo executado em paralelo com o shell, ou seja, o comando executado fica em background enquadro o shell fica disponível ao usuário. Isso é possível graças aos comandos avançados de redirecionamento que permite o controle sobre conteúdo dos ambientes dos comandos.
*   O shell bash, também prove um pequeno conjunto de instruções `built-in` comandos built-in (integrados), são comandos internos de uma linguagem/ferramentas são palavras reservadas que implementam funcionalidades. No caso do shell bash, esses comandos implementam funcionalidades que seriam inconvenientes ou impossíveis de se obter via comandos de ferramentas separadas. Alguns exemplos de comandos `cd`, `break`, `continue` e `exec` esses comandos não podem ser implementados for do shell pois são comandos que manipulam diretamente o próprio shell, quando dizemos comandos externos, estamos falando de comandos providos por exemplo, por binários compilados no diretório `/bin`.

    Outros comandos built-in como `history`, `getopts` `kill` ou `pwd`, poderiam sim serem implementados fora do shell, em ferramentas separadas. Porem é mais conveniente esses comandos serem parte do conjunto integrado.
* A execução de comandos no modo `interactively` é essencial. Porém o shell bash, também prove recursos de linguagem de auto nível como `variables`, `flow control constructs`, `quoting` e `functions`.
* O shell bash oferece features voltados especificamente para o modo interativo, dentro dessas features inclui-se `job control`, `command line editing`, `command history` and `aliases.`

### 2 Definições (Definitions)

Essas definições são usadas por todo o manual.

* **POSIX**: É a família que específica padrões abertos dos sistemas baseados em Unix. O Bash, se preocupa principalmente em servir como Shell e prover utilitários especificados no padrão `POSIX 1003.1 standard`.
* **blank**: Um caractere de espaço ou tabulação.
* **builtin**: Um comando implementado internamente para o próprio shell. Ao invés de um programa executável localizado em algum lugar no sistema de arquivo.&#x20;
*   **control operator**: Operador de controle, é um `token` que executa operações de controle, é uma nova linha ou um dos desses operadores:

    `||`

    `&&`

    `&`

    `;`

    `;;`

    `;&`

    `;;&`

    `|`

    `|&`

    `(`

    `)`
* **exit status**: status de saída, é um valor retornado pelo comando para quem chamou, esse valor é restrito em 8 bits, e seu valor máximo é 255.
* **field**: Campo, é uma unidade de texto resultante das `expansions` do shell, Após a expansão, ao executar um comando, os campos resultantes são usados como o nome e os argumentos do comando.
* **filename**: Uma string de caracteres usada para identificar um arquivo.
* **job**: Um conjunto de processos que compreende o pipeline e qualquer processo descendente dele, sendo todos do mesmo grupo de processo.
* **job control**: Um mecanismo para que os usuários possam  `stop(suspend)` e `restart(resume)` seletivamente a execução dos processos.
*   **metacaractere (metacharacter)**: São caracteres que quando não estando entre caracteres (Quoting) separa as palavras, são caracteres (Unquoted**)**. Um metacaractere é um espaço (space), tabulação (tab), nova linha (newline) ou um dos seguintes caracteres:

    `|`

    `&`

    `;`

    `(`

    `)`

    `<`

    `>`
* **name**: Uma palavra constituída unicamente de letras, números e underscores, e começa com uma letra ou underscore. **Names** são usados para nomear variáveis e funções. Também pode ser referenciado com um **indentifier**.
* **operator**: Um operador de controle **(control operator)** ou um operador de redireção**`(redirection operator)`**. _Operadores contém pelo menos um metacharacter sem aspas._
* **process group**: Uma coleção de processos relacionados, cada processo tendo o mesmo group ID.
* **process group ID**: ID do grupo de processo, é um identificador atribuído a um processo, que identificar seu grupo durante seu tempo de vida.
* **reserved word**: Palavra reservada, é uma palavra que tem significado especial para o shell. A maioria das palavras reservadas, introduz construções de controle do fluxo shell, como o `for` e o `while`.
* **return status**: é sinônimo de status de saída (**exit status**).
* **signal**: Um mecanismo pelo qual um processo pode ser notificado pelo kernel de um evento que ocorre no sistema.
* **special builtin**: Um comando integrado do shell que foi classificado como um comando especial pelo padrão POSIX.
* **token**: Uma sequencia de caracteres considerados uma única unidade pelo shell. É uma palavra(**word**) ou um operador(**operator**).
* **word**: Uma sequência de caracteres tratadas de modo único pelo shell. Palavras não pode incluir **metacharacters** sem aspas (**unquoted**).

### Utilidades básicas do Shell

* Bash é um acrônimo de Bourne-Again SHell. O Bourne shell (sh) é o shell tradicional em Shell Unix, originalmente escrito por Stephen Bourne.&#x20;
* Todos os comandos builtin do Bourne Shell (sh), estão disponíveis no Bash. As regras de **`evaluation`** e aspas (**quoting**), são retiradas da especificação POSIX que descreve o shell "padrão" do unix.
* Algumas features do Shell Bash são:
  * Shell Syntax
  * Shell Commands
  * Shell Functions
  * Shell Parameters
  * Shell Expansions
  * Redirections
  * Executing Commands
  * Shell Scripts

## 3 Recursos Básicos do Shell (Basic Shell Features)

### 3.1 Sintax Shell (Shell Syntax)

Quando o shell lê uma entrada, ele segue uma sequência de operações. Se na entrada encontra `#`  entende que se trata de um comentário e ignora o conteúdo após isso.

A grosso modo, o shell lê a entrada e divíde em palavras e operadores. Empregando a regra das aspas para selecionar quais significados atribuir a várias palavras e caracteres.

O shell então, analisa (faz o parse) desse tokens em comandos e outras construções. Remove o significado especial de certas palavras ou caracteres, expande outros. Redireciona a entra e saída se necessário, executa o comando especificado, espera pelo status de saída do comando. E torna esse status de saída para futura inspeção ou processamento.

#### 3.1.1 Operação do Shell (Shell Operation)

A seguir, uma breve descrição de como o Shell opera quando ele lê e executa comandos. Basicamente, o shell realiza os seguintes passos:

1. Le sua entrada de um arquivo (veja em [Shell Scripts](./#3.8-shell-scripts)), de uma string provida como argumento usando a opção `-c` (veja [Executando o Bash](./#6.1-invoking-bash-chamando-o-bash)) ou do terminal do usuário.
2. Quebra a entrada em palavras e operadores, obedecendo as regras de aspas descritas em Regras de Aspas (veja [Regra das Aspas](./#3.1.2-regra-das-aspas-quoting)). Esses tokens são separados por **metacharacters**. A expansão de alias é realizada nesta etapa (veja [Aliases](./#6.6-aliases)).
3. Analisa os tokens em comandos simples e compostos (veja [Comandos Shell](./#3.2-comandos-shell-shell-commands)).
4. Executa as várias [expansões de shell](#user-content-fn-1)[^1] (veja [Shell Expansions](./#3.5-expansao-de-shell-shell-expansions)), quebra os tokens expandidos em listas de nome de arquivos (veja [Expansão de Nome de Arquivo](./#3.5.8-filename-expansion)) e comando e argumentos.
5. Executa qualquer redirecionamento necessário (veja [Redirecionamentos](./#3.6-redirecionamentos-redirections)) e remove os operadores de redirecionamento e seus operandos da lista de argumentos.
6. Executa o comando (veja [Executando Comandos](./#3.7-executando-comandos-executing-commands)).
7. Opcionalmente, espera que a execução do comando se complete e coleta o seu status de saída (veja [Status de Saída](./#3.7.5-exit-status)).

***

Próximo: [Comentários](./#3.1.3-comentarios-comments), Anterior: [Operaç](./#3.1.1-operacao-do-shell-shell-operation)[ão do Shell](./#3.1.1-operacao-do-shell-shell-operation),  Acima: [Sintaxe Shell](./#3.1-sintax-shell-shell-syntax) \[[Tabela de Conteúdo](./#tabela-de-conteudo-table-of-contents)]\[Índice]

#### 3.1.2 Caracteres de Quoting (Quoting)

Caractere Quoting é usado pra remover o significado de determinados caracteres para o shell, serve para definir textos literais (strings). Quoting pode ser usado para desativar o tratamento especial que o shell dá para determinados caracteres especiais, para prevenir que palavras reservadas sejam reconhecidas como tal, e para prevenir a [expansão de parâmetro](#user-content-fn-2)[^2].

Cada uma dos metacaracteres do shell (veja [Definições](./#2-definicoes-definitions)) tem um&#x20;

#### 3.1.3 Comentários (Comments)

### 3.2 Comandos Shell (Shell Commands)

### 3.5 Expansão de Shell (Shell Expansions)

#### 3.5.8 Filename Expansion

### 3.6 Redirecionamentos (Redirections)

### 3.7 Executando Comandos (Executing Commands)

#### 3.7.5 Status de Saída (Exit Status)

### 3.8 Shell Scripts

### 6.1  Executando o Bash (Invoking Bash)

### 6.6 Aliases

## Apêndice D Índices

[^1]: Esse termo está estranho, deve haver alguma tradução melhor.

[^2]: Estranho isso aqui
