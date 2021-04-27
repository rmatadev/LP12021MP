**UNIVERSIDADE LUSÓFONA DE HUMANIDADES E TECNOLOGIAS**

*2020/2021*

# Mini Projecto - BombRoad

# 1. Introdução

## 1.1 Recomendações

Na resolução deste projecto deve ser utilizada a Linguagem de Programação C. Para além da correta implementação dos requisitos, tenha em conta os seguintes aspetos:
- O código apresentado deve estar *bem indentado*. 
- O código deve compilar sem erros ou *warnings* utilizando o *gcc* com as seguintes flags:
 `-Wall -Wextra -Wpedantic -ansi -Wimplicit-fallthrough=0`
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

O objectivo desta primeira parte do projecto é desenvolver um programa capaz de ler um ficheiro de configuração contendo o mapa com a localização de uma série de minas (bombas). e deverá fornecer ao utilizador uma interface para fazer alterações ao mapa - adicionar novas minas ou explodir uma mina existente.

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

Quando o utilizador introduz o texto `show`, o programa deverá apresentar o mapa no terminal. Caso nenhum ficheiro tiver sido lido, o mapa deverá ser constituído por espaços vazios (sem minas).

No terminal o caracter `.`  representa uma mina em estado armed e o caracter `*` representa uma bomba em estado off. As posições do mapa que estao vazias representam-se utilizando o caracter `_` (underscore).

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

Se, nas coordenadas passadas pelo utilizador, não existir uma mina, o programa deverá imprimir no stdou a mensagem `There is no bomb at the specified coordinate`.

Se, nas coordenadas passadas pelo utilizador, existir uma mina no estado off, o programa deverá apenas continuar sem nenhuma mensagem de erro.

### 2.1.4 Opção `plant`

Quando o utilizador introduz o texto `plant`, seguido das coordenadas X e Y, o programa deverá colocar, nas coordenadas X e Y, uma mina em estado armed.

Se, as coordenadas passadas pelo utilizador não forem válidas, o programa deverá imprimir no stdout a mensagem `Invalid coordinate`.

Se, nas coordenadas passadas pelo utilizador, existir uma mina em estado `off`, o programa deverá alterar o seu estado para `armed`.

Se, nas coordenadas passadas pelo utilizador, existir uma mina no estado `armed`,  o programa deverá apenas continuar sem nenhuma mensagem de erro.

### 2.1.5 Opção `export`

Quando o utilizador introduz o texto `export` seguido do nome do ficheiro `filename`, o programa deverá criar um ficheiro novo, com o nome `filename`, contendo a informação do mapa. O ficheiro deverá conter a informação da localização de todas as minas e o seu estado.

O formato do ficheiro de output deverá ser o mesmo do ficheiro de input. É indiferente a ordem pela qual cada par de coordenadas é escrita no ficheiro, desde que o ficheiro respeite o formato de ficheiro input espeficicado.

### 2.1.6 Opção `quit`

O programa deverá simplesmente terminar.

### 2.1.7 Opção `sos`
Apresenta de novo o menu com as opções.

### 2.2 Ficheiro de input

O programa deverá ler um ficheiro de input. Este ficheiro terá tipicamente a extensão `.ini`, contudo outras extensões poderão ser utilizadas.

Todas as linhas do ficheiro começadas pelo caracter `#` deverão ser ignoradas. *Linhas em branco também deverão ser ignoradas.*
A primeira linha útil (que não começa `#`) do ficheiro deverá conter o tamanho do mapa representado por dois interos:
```
25 25
```
*Neste projecto, o tamanho do mapa é fixo (25 linhas e 25 colunas) e portanto todos os ficheiros de input têm a primeira linha igual.*

As linhas seguintes contém o estado, representado pelo caracter `.` ou `*`, seguido da localização da bomba. Cada linha pode apenas conter informação de uma bomba. Cada localização (coordenada) representa-se por um par de valores inteiros (X Y) separados por um espaço em branco (espaço ou tab), sendo X o número da linha e Y o número da coluna. Assim uma bomba em estado `armed` na coordenata 10 5 representa-se da seguinte forma:
```
. 10 5
```
Exemplo de ficheiro de input:
```
# Ficheiro exemplo
25 25
. 0 1
. 0 2
* 0 3
* 5 9
* 6 4
* 6 5
* 0 4
# comentario a ser ignorado
. 0 5
. 1 4
. 2 3
. 2 6
. 2 9
. 3 4
. 3 9
. 4 9
. 5 2
. 5 3
. 5 4
. 6 9
. 7 6 texto ignorado
. 7 2
. 8 3
. 8 6
```

A leitura do ficheiro deverá ser realizada até detectar o fim do ficheiro. 

Caso não seja possível abrir o ficheiro, o programa deverá imprimir a mensagem `Error opening file` e deverá voltar a esperar novo comando do utilizador.

Para cada bomba, deverá existir o estado, uma coordenada X e uma Y. Caso isso não se verifique, significa que o ficheiro está mal formatado. Nesse caso o programa deverá mostrar a mensagem `File is corrupted` e deverá voltar a esperar novo comando do utilizador. O caracter que separa o estado, a coordenada X e a coordenada Y poderá ser um espaço ou um tab, e todo o texto que apareça após a coodenada Y da bomba deverá ser simplesmente ignorado.

A mesma mensagem deverá ser mostrada caso alguma coordenada presente no ficheiro não seja válida (fora dos limites). Se isso acontecer, a leitura do ficheiro deverá terminar e o programa deverá voltar a esperar novo comando do utilizador.

Caso seja detectado algum erro no ficheiro, o mapa deverá ser limpo, eliminando todas as bombas que tiverem sido lidas até ao momento. 
** Não é necessário guardar uma cópia do mapa existente. **Se ocorrer um erro na leitura do ficheiro, o mapa deverá ficar limpo (sem bombas)**, independentemente de existirem bombas no mapa antes da leitura do ficheiro.

Os alunos deverão criar os seus próprios ficheiros de input para testarem os seus programas. Ficheiros de input criados pelos alunos deverão ser entregues no moodle e serão sujeitos a avaliação. Os alunos deverão criar ficheiros de input válidos e inválidos para poderem testar correctamente os seus programas.


## 4. Material a entregar

* Ficheiro `.c` com código devidamente comentado e indentado:
    - Deve implementar as funcionalidades pedidas.
    - O código deverá ser submetido na plataforma PANDORA [(2)](#ref2) até às **23:59 do dia 16 de Maio de 2021** no *contest* **LP12021MP**.
    - A plataforma corre automaticamente uma série de testes e no fim atribui uma classificação **indicativa**. Os alunos deverão analisar o relatório emitido pela plataforma e poderão alterar o código e voltar a submeter o trabalho. **Neste trabalho haverá limite de submissões!**
      A plataforma não permite a entrega de trabalhos após a data e hora limite.
* Ficheiro .zip a ser submetido no moodle [(3)](#ref3) até às  **23:59 do dia 17 de Maio de 2021**, dentro do qual deverá estar:
    - Fluxograma em formato pdf
    - Ficheiros de input criados pelo aluno
    - ficheiro .c com o código do aluno

## 5. Fluxograma e Peer Review

Cada aluno deverá produzir e entregar um fluxograma em formato pdf que descreve o funcionamento da função que faz a leitura do ficheiro. O fluxograma não pode estar identificado, ou seja, não deverá conter qualquer referência ao seu autor.


Neste trabalho iremos aplicar uma técnica de *peer review*. Consequentemente os fluxogramas, assim como o código submetido no moodle terá de ser anónimo.

Cada aluno (individualmente) terá que rever e avaliar o trabalho de 4 colegas seus. Esta avaliação consiste no preenchimento de um formulário com critérios estipulados. Este formulário será disponibilizado na plataforma Moodle.

**Alunos que não submetam a sua avaliação, não terão nota no Mini Projecto.**

A avaliação dos colegas terá um peso na avaliação. A avaliação das avaliações será feita pelo professor e serão aplicadas penalizações sobre esta componente sempre que forem detectadas avaliações injustificadas e que não correspondem à realidade. Por exemplo classificar um trabalho com nota de 20 quando claramente o não satisfaz critérios para uma classificação superior a 15.


### 5.1 Critérios de avaliação

1. Qualidade de código - variáveis - peso 3 - As variáveis utilizadas têm nomes apropriados que facilitam a leitura do código?
2. Qualidade de código - funções - peso 4 - o código foi correctamente dividido em funções facilitando a sua interpretação e evitando repetição de código?
3. Qualidade de código - indentação - peso 5 - o código está correctamente indentado?
4. Qualidade de código - cometários - peso 5 - o código está correctamente comentado? Deverá conter um comentário que descreve o objectivo de cada função e deverá conter comentários sempre que isso for benéfico para ajudar a interpretação do código.
5. Qualidade de código - variáveis globais - peso 3 - o código utiliza variáveis globais que não são constantes?
6. Qualidade de código - simplicidade - peso 3 - o código é simples e fácil de interpretar?
7. Qualidade de código - eficiencia - peso 5 - a solução implementada é eficiente? I.e. atinge os objectivos consumindo o menos recursos (memória e tempo) possível?
8. Fluxograma - peso 3 - o fluxograma está fiel ao código implementado?
9. Fluxograma - peso 3 - o fluxograma descreve correctamente o funcionamento pedido no enunciado?
10. Ficheiros de input - peso 2 - o aluno entregou pelo menos 1 ficheiro de input válido?
11. Ficheiros de input - peso 2 - o aluno entregou pelo menos 1 ficheiro de input inválido?

## 6. Peso na avaliação

O projecto vale 15% da nota final da componente prática e será cotado de 0 a 20 valores, distribuídos seguinte forma:

* 17  valores (85%) - Funcionamento
* 3 valores (15%) - Peer Review (Fluxograma e código)

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
