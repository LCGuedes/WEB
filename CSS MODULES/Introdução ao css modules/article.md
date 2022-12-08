- O CSS não possui nenhum tipo de solução nativa para dinamicidade da escrita e por sua permissividade em relação aos estilos globais. Nesse cenário, os efeitos colaterais podem se multiplicar e serem completamente imprevisíveis.

- Grande parte das soluções propostas nos últimos anos contornaram muito bem a questão da escrita, entre elas estão as metodologias de escrita e organização (e.g. OOCSS, SMACSS, BEM) e os pré-processadores (e.g. SASS, LESS, Stylus).

- A questão aqui é: como contornamos os problemas técnicos do CSS? Como ter manutenibilidade sem uma arquitetura desnecessariamente complexa? Como escopar e reaproveitar estilos como parte integrante da aplicação? E por último mas não menos importante: como parar de se preocupar com os side-effects?

- CSS Modules se propõe a resolver exatamente os problemas funcionais que viemos enfrentando, pelo menos em aplicações Javascript componentizadas (e.g. React, Angular, Ember).

- Basicamente esses problemas são resolvidos escopando os estilos por padrão.

- Utilizando webpack e babel, podemos escrever html e css de forma escopada. Basicamente geramos o código html atráves da manipulação do DOM usando javascript.

- O que antes seria:

  <button class="button">click</button>

- Com o CSS Modules será:

  import {button} from './styles.css'

  let generateButton = ´<button class="button">click</button>´

  document.write(generateButton);

- Desse modo, podemos criar componentes em arquivos separados e apenas importa-los para index (componentização).

- Cada componente vai levar seu CSS junto, o que quer dizer que não haverá mais uma complexidade desnecessária na arquitetura de estilos, ela vai apenas seguir a complexidade da própria aplicação. É por isso que CSS Modules funciona tão bem com React, por exemplo.

- É possível configurar o webpack para manipular os nomes das classes e transformá-los em únicos. Cada classe vai ter o nome do arquivo, o nome definido na declaração da classe e uma hash escrita em base 64.

Isso possibilita que tenhamos estilos completamente escopados e independentes. Usando o mesmo nome de classe ao escrever os componentes, isso não é propagado a outros componentes, o que elimina os side-effects. Além disso, não precisamos mais quebrar a cabeça ao escrever e organizar o CSS. Nada mais de classes do tipo “editable-todo-list-one-more-word-so-the-class-will-not-match-any-other“.

- É possível reaproveitar estilos entre classes. Isso é um assunto realmente delicado quando se fala em CSS Modules. Ter um workflow baseado em reaproveitamento de estilos globais num sistema que induz ao escopamento por padrão chega a ser contraditório. O ideal é que se aproveitem os componentes em si.

Exemplos:

    1) Reaproveitamento de classes em uma mesma folha:

.listBase {
border: 5px solid black;
font-size: 30px;
}

.myList {
composes: listBase;
color: red;
background-color: yellow;
}

2. Reaproveitamento de classes localizadas em arquivos diferentes:

base {
width: 50%;
line-height: 2em;
margin: 2rem auto;
}

.listBase {
border: 5px solid black;
font-size: 30px;
}

.myList {
composes: listBase;
composes: base from './base.css';
color: red;
background-color: yellow;
}
