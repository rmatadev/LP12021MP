**UNIVERSIDADE LUSÓFONA DE HUMANIDADES E TECNOLOGIAS**

*2020/2021*

# Mini Projecto - BombRoad

# 1. Introdução

## 1.1 Recomendações

Na resolução deste projecto deve ser utilizada a Linguagem de Programação C. Para além da correta implementação dos requisitos, tenha em conta os seguintes aspetos:
- O código apresentado deve estar *bem indentado*. 
- O código deve compilar sem erros ou *warnings* utilizando o *gcc* com as seguintes flags:
 `-Wall -Wextra -Wpedantic -ansi`
- Tenha em atenção os nomes dados das variáveis, para que sejam indicadores daquilo que as mesmas vão conter.
- Evite o uso de constantes mágicas. 
- Evite duplicação de código. 
- Considere a implementação de funções para melhorar a legibilidade, evitar a duplicação e criar soluções mais genéricas.
- É proíbida a utilização de variáveis globais - i.e. variáveis declaradas fora de qualquer função.
- Este trabalho será realizado individualmente.

Para a realização deste projecto, os alunos deverão adquirir as seguintes competências:
- Manipulação de ficheiros
- Vectores e matrizes
- Strings
- Ciclos
- Condições

## 1.2 Descrição

O objectivo desta primeira parte do projecto é desenvolver um programa capaz de ler um ficheiro de configuração contendo o mapa com a localização de uma série de minas. e deverá fornecer ao utilizador uma interface para fazer alterações ao mapa - adicionar novas minas ou explodir uma mina existente.

Cada posição no mapa representa-se por duas coordenadas (X, Y). Estas coordenadas são valores inteiros e, nesta primeira fase, podem assumir valores no intervalo [0, 24]. Cada uma das coordenadas do mapa poderá estar vazia (sem mina) ou ter uma mina. Cada mina poderá assumir apenas 2 estados, ou armed ou off (armada ou explodida). 

![mapa](map.png)

# 2 Implementação

## 2.1 Menu

O programa deverá começar por apresentar o seguinte menu:

```
+-----------------------------------------------------
read <filename>     - read input file
show                - show the mine map
trigger <x> <y>     - trigger mine at <x> <y>
plant <x> <y>       - place armed mine at <x> <y>
export <filename>   - save file with current map
quit                - exit program
sos                 - show menu
+-----------------------------------------------------
```

Sempre que o programa estiver à espera que o utilizador introduza um input, deverá imprimir, numa linha isolada, o caracter `>` - ver exemplo.

### 2.1.1 - Opção `read`

Quando o utilizador introduz o texto `read` seguido do nome do ficheiro `filename`, o programa deverá ler o ficheiro com o nome `filename` e construir o mapa correspondente. 

### 2.1.2 Opção `show`

Quando o utilizador introduz o texto `show`, o programa deverá apresentar o mapa no terminal. Caso nenhum ficheiro tiver sido lido, o mapa deverá ser constituído por espaços vazios.

No terminal o caracter `.`  representa uma mina em estado armed e o caracter `*` representa uma bomba off. As posições do mapa que estao vazias representam-se utilizando o caracter ` _` (underscore).

Exemplo:

```
>show
_.*...___________________
____.____________________
___.__*__._______________
____.____._______________
_________._______________
__*..____._______________
____*.___*_______________
__.___.__________________
___.__*__________________
```

### 2.1.3 Opção `trigger`

Quando o utilizador introduz o texto `trigger`, seguido das coordenadas X e Y, o programa deverá alterar o estado da mina nas coordenadas X e Y de armed para off.

Se, as coordenadas passadas pelo utilizador não forem válidas, o programa deverá imprimir no stdout a mensagem `Invalid coordinate`.

Se, nas coordenadas passadas pelo utilizador, não existir uma mina, o programa deverá imprimir no stdou a mensagem `No mine at specified coordinate`.

Se, nas coordenadas passadas pelo utilizador, existir uma mina no estado off,  o programa deverá apenas continuar sem nenhuma mensagem de erro.

### 2.1.4 Opção `plant`

Quando o utilizador introduz o texto `plant`, seguido das coordenadas X e Y, o programa deverá colocar, nas coordenadas X e Y, uma mina em estado armed.

Se, as coordenadas passadas pelo utilizador não forem válidas, o programa deverá imprimir no stdout a mensagem `Invalid coordinate`.

Se, nas coordenadas passadas pelo utilizador, existir uma mina em estado `off`, o programa deverá alterar o seu estado para `armed`.

Se, nas coordenadas passadas pelo utilizador, existir uma mina no estado `armed`,  o programa deverá apenas continuar sem nenhuma mensagem de erro.

### 2.1.5 Opção `export`

Quando o utilizador introduz o texto `export` seguido do nome do ficheiro `filename`, o programa deverá criar um ficheiro novo, com o nome `filename`, contendo a informação do mapa. O ficheiro deverá conter  a informação da localização de todas as minas, independente do seu estado. Ou seja, o ficheiro não fará distinção entre os estados das minas, assumindo que estão todas em estado armed. 

O formato do ficheiro de output deverá ser o mesmo do ficheiro de input. Ou seja, deverá conter uma série de pares de coordenadas X, Y. É indiferente a ordem pela qual cada par de coordenadas é escrita no ficheiro e também é indiferente o separador utilizado entre pares de coordenadas, podendo ser utilizado um ` `(espaço), `\t`(tab) ou `\n` (newline).

### 2.1.6 Opção `quit`

O programa deverá simplesmente terminar.

### 2.1.7 Opção `sos`
Apresenta de novo o menu com as opções.

### 2.2 Ficheiro de input

O programa deverá ler um ficheiro de input. Este ficheiro terá tipicamente a extensão `.ini`, contudo outras extensões poderão ser utilizadas.  

O ficheiro de input será um ficheiro codificado em texto que deverá conter um conjunto de pares de coordenadas que representam as localizações das minas em estado armed. As minas em estado off ou os espaços vazios não são representados no ficheiro. Cada coordenada representa-se por um par de valores inteiros (X Y) separados por um espaço em branco, sendo X o número da linha e Y o número da coluna. Não há nenhuma restrição relativa a mudanças de linha dentro do ficheiro ou em relação à ordem de cada par de coordenadas dentro do ficheiro.

Exemplo de ficheiro de input:

```
0 1
0 2
0 3
5 9 6 4 6 5
0 4
0 5

1 4 2 3 2 6 2 9
3 4
3 9 4 9
5 2
5 3

5 4
6 9 7 6
7 2
8 3
8 6
```

A leitura do ficheiro deverá ser realizada até detectar o fim do ficheiro. 

Caso não seja possível abrir o ficheiro, o programa deverá imprimir no stdout a mensagem `Error opening file`.

Para cada coordenada X representada no ficheiro deverá existir uma coordenada Y. Caso isso não se verifique, significa que o ficheiro está mal formatado. Nesse caso o programa deverá mostrar no stream `stdout`a mensagem `File is corrupted`.
A mesma mensagem deverá ser mostrada caso alguma coordenada presente no ficheiro não seja válida (fora dos limites).

Os alunos deverão criar os seus próprios ficheiros de input para testarem os seus programas. Ficheiros de input criados pelos alunos deverão ser entregues no moodle juntamente com o relatório.


## 3.  Exemplo de utilização

A ser preenchido.

## 4. Material a entregar

* Ficheiro `.c` com código devidamente comentado e indentado:
    - Deve implementar as funcionalidades pedidas.
    - O código deverá ser submetido na plataforma PANDORA [(2)](#ref2) até às **23:59 do dia 3 de Janeiro de 2021** no *contest* **IC2020MP**.
    - A plataforma corre automaticamente uma série de testes e no fim atribui uma classificação **indicativa**. Os alunos deverão analisar o relatório emitido pela plataforma e poderão alterar o código e voltar a submeter o trabalho. **Neste trabalho haverá limite de submissões!**
      A plataforma não permite a entrega de trabalhos após a data e hora limite.
    - Incorrecta indentação do código poderá originar penalizações na nota.
* Ficheiro .zip contendo o relatório em formato [md] e os ficheiros de input criados.
    - Este ficheiro deverá ser submetido no moodle [(3)](#ref3) até às  **23:59 do dia 4 de Janeiro de 2021**. 

## 5. Relatório e Peer Review

Cada grupo deverá produzir e entregar um relatório cujo objectivo é explicar em detalhe a solução encontrada. Note que um relatório não deve conter código. Se for absolutamente essencial, pode, pontualmente, mostrar trechos de código.

O relatório deve conter *no mínimo* as seguintes secções:
   * Título
   * Descrição da solução
       * Fluxograma das funções principais
   * Descrição do método utilizado para teste e debug
   * Conclusões e matéria aprendida
   * Referências
      * Incluindo trocas de ideias com colegas, código aberto reutilizado e bibliotecas utilizadas

Neste trabalho iremos aplicar uma técnica de *peer review* dos relatórios. Consequentemente os relatórios terão de ser anónimos - **os relatórios não podem conter os nomes, nem os números dos autores**. Apenas o nome do ficheiro terá de respeitar o formato: `relatorio_<nome_do_grupo>.md`. 

Cada aluno (individualmente) terá que rever e avaliar pelo menos 3 relatórios de colegas seus. Esta avaliação consiste no preenchimento de formulário com critérios estipulados. Este formulário será disponibilizado na plataforma Moodle.

Alunos que não submetam a sua avaliação dos relatórios do colegas, não terão nota no Mini Projecto.

A avaliação dos relatórios terá um peso na avaliação. A avaliação das avaliações será feita pelo professor e serão aplicadas penalizações sobre esta componente sempre que forem detectadas avaliações injustificadas e que não correspondem à realidade. Por exemplo classificar um relatório com nota de 20 quando claramente o relatório não satisfaz critérios para uma classificação superior a 15.

## 6. Peso na avaliação

O projecto vale 15% da nota final e será cotado de 0 a 20 valores, distribuídos seguinte forma:

* 17  valores (85%) - Código (funcionalidade, indentação, comentários) - A classificação do Pandora servirá apenas de referência.
* 3 valores (15%) - Relatório e Peer Review

## 7. Honestidade Académica

Nesta disciplina, espera-se que cada aluno siga os mais altos padrões de honestidade académica. Trabalhos que sejam identificados como cópias serão anulados e os alunos envolvidos terão nota zero - quer tenham copiado, quer tenham deixado copiar.
Para evitar situações deste género, recomendamos aos alunos que nunca partilhem ou mostrem o seu código.
A decisão sobre se um trabalho é uma cópia cabe exclusivamente aos docentes da unidade curricular.
Os alunos são encorajados a discutir os problemas com outros alunos mas não deverão, no entanto, copiar códigos, documentação e relatórios de outros alunos. Em nenhuma circunstância deverão partilhar os seus próprios códigos, documentação e relatórios. De facto, não devem sequer deixar códigos, documentação e relatórios em computadores de uso partilhado.

## Referências

<a name="ref1"></a>

* (1) Pereira, A. (2017). C e Algoritmos, 2ª edição. Sílabo.

<a name="ref2"></a>

* (2)  PANDORA - Yet Another Automated Assessment Tool, https://saturn.ulusofona.pt/.

<a name="ref3"></a>

* (3)  Lusófona Moodle, https://secure.grupolusofona.pt/ulht/moodle/

## Metadados

* Autor: [Pedro Serra]
* Curso:  [Licenciatura em Videojogos]
* Instituição: [Universidade Lusófona de Humanidades e Tecnologias][ULHT]
