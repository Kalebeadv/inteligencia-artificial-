# Resumo do conteúdo - Kalebe Silva

# Metodologias de Busca

_Pesquisa é o processo de percorrer vielas para ver se elas são sem saída. (Marston Bates)_

_Quando uma coisa é engraçada, busque-a cuidadosamente por uma verdade escondida. (George Bernard Shaw)_

_Se não acharmos qualquer coisa agradável, pelo menos acharemos algo novo. (Voltaire, Candide)_

_Aquele que pede, recebe; e, o que procura, encontra. (O Evangelho segundo São Mateus, Capítulo 7, Versículo 8)_

## Introdução

No Capítulo 3 foram apresentados árvores de busca e outros métodos e representações que são utilizados para solucionar problemas por meio de técnicas de Inteligência Artificial, tais como busca. No Capítulo 4 apresentamos diversos métodos que podem ser utilizados para busca e discutimos quão efetivos eles são em diferentes situações. Busca em profundidade e busca em largura são os métodos de busca mais conhecidos e amplamente utilizados e, neste capítulo, examinamos porque e como eles são implementados. Também veremos diversas propriedades dos métodos de busca, incluindo o ser ótimo e a completude, que podem ser utilizados para determinar quão útil um método de busca será ao solucionar um determinado problema.

Os métodos que são descritos neste capítulo e no Capítulo 5 têm impacto em quase todo aspecto da Inteligência Artificial. Por causa da natureza sequencial, na qual computadores tendem a operar, a busca é necessária para determinar soluções para uma enorme gama de problemas.

Este capítulo começa discutindo métodos de busca cega e prossegue examinando métodos de busca informados – esses métodos de busca utilizam heurísticas para examinar um espaço de busca mais eficientemente.

### Solução de problemas como busca

Solução de problemas é um importante aspecto da Inteligência Artificial. Um problema pode ser considerado como consistindo em um objetivo e um conjunto de ações que podem ser praticadas para alcançar esse objetivo. Em qualquer tempo, consideramos o estado do espaço de busca para representar aonde chegamos como um resultado das ações aplicadas até então.

Por exemplo, consideremos o problema de procurar as lentes de contato no campo de futebol. O estado inicial é como começamos, ou seja, sabemos que as lentes estão em algum lugar no campo de futebol, mas não sabemos aonde. Se utilizarmos a representação na qual examinamos o campo em unidades de um centímetro quadrado, então nossa primeira ação será examinar o quadrado no canto superior esquerdo do campo. Se não acharmos as lentes lá, podemos considerar o estado agora como sendo examinamos o quadrado do canto superior esquerdo e não achamos as lentes. Depois de várias ações, o estado deverá ser que examinamos 500 quadrados e agora encontramos as lentes no último quadrado examinado. Esse é um estado objetivo, pois ele satisfaz a meta que tínhamos de encontrar as lentes de contato.

Busca é um método que pode se utilizado por computadores para examinar um espaço de problema, como esse, de modo a encontrar um objetivo. Frequentemente, queremos encontrar o objetivo o mais rápido possível ou sem utilizar muitos recursos. Um espaço de problema também pode ser considerado como espaço de busca, pois, de modo a solucionar o problema, faremos uma busca no espaço por um estado objetivo. Continuaremos utilizando o termo espaço de busca para descrever esse conceito.

Neste capítulo, analisaremos vários métodos para examinar um espaço de busca. Esses métodos são chamados de métodos de busca.

### Busca guiada por dados ou busca guiada por objetivos

Existem duas abordagens principais para fazer busca em uma árvore de busca, que aproximadamente correspondem às abordagens de-cima-para-baixo e de-baixo-para-cima, discutidas na Seção 3.12.1. Busca guiada por dados parte de um estado inicial e usa ações que são permitidas para ir em frente até que um objetivo seja atingido. Essa abordagem também é conhecida como encadeamento para frente.

Alternativamente, a busca pode começar no objetivo e voltar para um estado inicial, vendo quais deslocamentos poderiam ter levado ao estado objetivo. Isso é busca guiada por objetivos, também conhecida como encadeamento para trás.

A maioria dos métodos de busca que examinaremos neste capítulo e no Capítulo 5 é formada por métodos de busca guiada por dados: eles partem de um estado inicial (a raiz na árvore de busca) e trabalham em direção ao nó objetivo.

Em muitas circunstâncias, busca guiada por objetivos é preferível à busca guiada por dados, mas na maior parte do livro, quando nos referirmos a “busca”, estaremos falando de busca guiada por dados.

Busca guiada por objetivos e busca guiada por dados terminam produzindo o mesmo resultado, porém, dependendo da natureza do problema a ser resolvido, um dos métodos pode resolvê-lo mais eficientemente que o outro – em particular, em algumas situações um deles pode envolver examinar mais estados que o outro.

Busca guiada por objetivos é particularmente útil em situações nas quais o objetivo pode ser claramente especificado (por exemplo, um teorema a ser provado ou encontrar uma saída em um labirinto). É também, sem dúvida, a melhor escolha em problemas como diagnósticos médicos onde o objetivo (a condição a ser diagnosticada) é conhecido, mas os outros dados (neste caso, as causas da condição) precisam ser encontrados.

Busca guiada por dados é mais útil quando os dados iniciais são fornecidos e não temos clareza sobre o objetivo. Por exemplo, um sistema que analise dados astronômicos e, assim, faça deduções sobre a natureza de estrelas e planetas, receberia um grande volume de dados, mas não necessariamente lhe seria dado um objetivo direto. Em vez disso, seria esperado que analisasse os dados e tirasse suas próprias conclusões. Esse tipo de sistema tem um imenso número de possíveis objetivos que poderia localizar. Neste caso, busca guiada por dados é mais apropriada.

É interessante considerar um labirinto que tenha sido criado para ser percorrido de um ponto inicial de modo a chegar a um ponto final específico. É quase sempre mais fácil começar do ponto final e voltar ao ponto inicial. Isso se deve aos diversos caminhos sem saída que foram estabelecidos desde o ponto inicial (dados) e somente um caminho que foi estabelecido para o ponto final (objetivo). Como resultado, vir do objetivo ao início tem somente um caminho possível.

### Gerar e testar

A mais simples abordagem de busca é chamada Gerar e Testar. Isto envolve simplesmente gerar cada nó no espaço de busca e testá-lo para verificar se este é um nó objetivo. Se for, a busca teve sucesso e não precisa ser levada adiante. Caso contrário, o procedimento segue para o próximo nó.

Essa é a forma mais simples de busca de força bruta (também chamada de busca exaustiva), assim chamada porque ela não pressupõe conhecimento adicional além de como percorrer a árvore de busca e como identificar nós folhas e nós objetivos, que terminará por examinar cada nó da árvore até encontrar um objetivo.

Para ter sucesso, Gerar e Testar precisa ter um Gerador adequado, que deve satisfazer a três propriedades:

1.Ele deve ser completo: em outras palavras, ele deve gerar todas as soluções possíveis; caso contrário, poderia descartar uma solução adequada.

2.Ele não deve ser redundante: isso significa que não deve gerar a mesma solução duas vezes.

3.Ele deve ser bem informado: isso significa que só deve propor soluções adequadas e não deve examinar possíveis soluções que não combinem com o espaço de busca.

O método Gerar e Testar pode ser aplicado com sucesso a diversos problemas e, na verdade, é o modo pelo qual as pessoas frequentemente solucionam problemas onde não há informação adicional sobre como alcançar uma solução. Por exemplo, se você sabe que um amigo mora em uma determinada rua, mas não sabe em qual casa, a abordagem Gerar e Testar poderia ser necessária; isto envolveria tocar a campainha de cada casa da rua até você achar seu amigo. De modo análogo, Gerar e Testar pode ser utilizado para achar soluções para problemas combinatórios como o problema das oito rainhas que é apresentado no Capítulo 5.

Gerar e Testar é também, algumas vezes, referido como uma técnica de busca cega, devido ao modo pelo qual a árvore de busca é percorrida, sem utilizar qualquer informação sobre o espaço de busca.

Exemplos mais sistemáticos de busca de força bruta são apresentados neste capítulo, em particular, busca em profundidade e busca em largura.

Técnicas de busca mais “inteligentes” (ou informadas) serão exploradas mais tarde neste capítulo.

### Busca em profundidade

Um algoritmo comumente utilizado é busca em profundidade. Busca em profundidade é assim chamada por seguir cada caminho até a sua maior profundidade antes de seguir para o próximo caminho. O princípio subjacente à busca em profundidade é ilustrado na Figura 4.1. Supondo que comecemos pelo lado esquerdo e sigamos para o lado direito, a busca em profundidade envolve descer pelo caminho mais à esquerda na árvore até achar uma folha. Se este for um estado objetivo, a busca foi concluída e será relatado sucesso.

Se a folha não representar um estado objetivo, a busca retrocederá ao primeiro nó anterior que tenha um caminho não explorado. Na Figura 4.1, após examinar o nó G e descobrir que este não é um objetivo, a busca retrocede ao nó D e explora seus outros filhos. Neste caso, só há um outro filho, que é H. Depois que este nó for examinado, a busca retrocederá ao próximo nó não expandido, que é A, pois B não tem filhos não explorados.

![](profunda.jpg)