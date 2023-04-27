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
* All behaviors live at the _container_ level, while child items are simple state containers.
  * The container *must* know about the children because of patterns like `aria-activedescendant`
  * Having the child items _also_ know about the parent would introduce a circular data flow that
    would make the patterns more difficult to reason about.
  * This implies that either event listeners must live at the container level, or that the container
    must have some way of knowing when the child state changes via some implementation of the
    Observer pattern.
  * This also implies that the container, which cares about order, must either have some way to
    query for its child items or that child items must broadcast when they are added/remove/moved
    and that they must be aware of their own index.
  * Example: A `listbox` knows about its options, but the each option knows nothing of its listbox.
* From an API perspective, end-developers *want* to interact with the container for things that
  deal with the child items. For example, reading the selected value from a `listbox`, even though
  the selection state lives on the `option`.

### The pattern class state is the source of truth
* The DOM acts as the _output_ of the state rather than as the source of truth, which aligns with
most frameworks.
* For custom elements, the pattern class _is_ the custom `Element`.


## Interaction patterns targeted
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
