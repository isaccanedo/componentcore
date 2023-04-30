# componentcore
Protótipo de base de componentes de baixo nível com base na composição de comportamentos em padrões de interação

## Notas de implementação

## Comportamentos
Um comportamento é uma parte atômica de funcionalidade que pode ser aplicada a um componente, como
"pode ser desativado" ou "tem itens". Cada comportamento é definido como uma interface. Os comportamentos podem ser
cumulativo. Por exemplo, um componente que "tem um item ativo" deve inerentemente "ter itens".

Behaviors can be added to an existing class via mixin, which here is a function that takes
in a `class` and augments it with some additional functionality.

Comportamentos se compõem em padrões.

## Padrões
Um padrão é uma união de comportamentos que corresponde a um dos "padrões de interação" canônicos
comum na web. Na maioria das vezes, eles se alinham com as funções ARIA padrão.

Cada padrão define uma interface como uma união de comportamentos e um mixin que aplica tudo isso
comportamentos em uma classe existente.

### Estrutura agnóstica
O código aqui visa fornecer "primitivas comportamentais" que podem ser usadas em qualquer framework ou com
nenhum quadro em tudo.

### Comportamentos e padrões não conhecem o DOM
Diferentes frameworks têm seus próprios sistemas para executar a manipulação de DOM, portanto, adiamos
toda a renderização no nível do framework.

### Os elementos do contêiner sabem sobre seus filhos e não o contrário
* Todos os comportamentos residem no nível _container_, enquanto os itens filhos são contêineres de estado simples.
  * O contêiner *deve* saber sobre os filhos por causa de padrões como `aria-activedescendant`
  * Ter os itens filho _também_ sabendo sobre o pai introduziria um fluxo de dados circular que
     tornaria os padrões mais difíceis de raciocinar.
  * Isso implica que os ouvintes de eventos devem residir no nível do contêiner ou que o contêiner
     deve ter alguma maneira de saber quando o estado filho muda por meio de alguma implementação do
     Padrão de observador.
  * Isso também implica que o contêiner, que se preocupa com a ordem, deve ter alguma maneira de
     consulta para seus itens filhos ou que os itens filhos devem ser transmitidos quando são adicionados/removidos/movidos
     e que eles devem estar cientes de seu próprio índice.
  *Exemplo: Um `listbox` conhece suas opções, mas cada opção não conhece nada de seu listbox.
* Do ponto de vista da API, os desenvolvedores finais *querem* interagir com o contêiner para coisas que
   lidar com os itens filhos. Por exemplo, lendo o valor selecionado de um `listbox`, mesmo que
   o estado de seleção reside na `opção`.

### O estado da classe padrão é a fonte da verdade
* O DOM atua como a _saída_ do estado, e não como a fonte da verdade, que se alinha com
maioria dos quadros.
* Para elementos personalizados, a classe padrão _é_ o `Elemento` personalizado.


## Padrões de interação direcionados
* listbox
* option
* combobox
* dialog
* grid (row, gridcell, rowgroup, rowheader)
* tree
* slider
* menu
* menuitem, menuitemcheckbox, menuitemradio
* menubar
* tablist
* tab
* tabpanel
* radio-group

## Patterns that don't map exactly to an ARIA role
* Expansion panel
* Accordion
* Datepicker
