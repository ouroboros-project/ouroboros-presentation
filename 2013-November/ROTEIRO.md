
[SLIDE 1] (ambos)
  Apresentação (talvez destacar a parte "automatizada")

[SLIDE 2] (wil)
  Índice: falar a estrutura da apresentação

----------------------------------
Introdução: Motivações e objetivos
----------------------------------

[SLIDE 1] (omar)
    Esse projeto nasceu a 2 anos atrás, no USPGameDev. Nós queríamos
    adicionar suporte a uma linguagem de script no jogo que estavámos desenvolvendo
    mas não conseguiamos nos decidir entre Lua e Python. Foi ai que tivemos a idéia
    de abusar de nossos conhecimentos de BCÇóide para usar as duas de forma
    generalizada.
    E assim surgiu o Sistema de Scripts da UGDK...

[SLIDE 2 - a, b, c, d] (omar)
    [a]
    A UGDK é a game engine que desenvolvemos no grupo de jogos, e usamos ela 
    em alguns jogos que fizemos.

    O sistema de scripts abstrai a interface das linguagens de script para
    que possamos usar elas a partir do C++ sem se preocupar com a interface dela,
    ou até com qual linguagem estamos usando. 

    Inicialmente, implementamos suporte a Lua e Python no sistema.

    [b]
    Esse é o Horus Eye, um jogo do USPGameDev. E isso é o script Lua que gera
    esse mapa, especificando onde ficam os elementos como paredes e transições
    pra outro mapa, assim também como gerar esses vasos aleatoriamente.

    Isso é algo que você pode fazer em C++? É, mas usando scripts nos permite
    desenvolver e testar esses aspectos do jogo bem mais rapidamente. Não só pelas
    facilidades da linguagem de script, mas também por não ter que recompilar o jogo.
    Podemos inclusive alterar um script enquanto o jogo está rodando.

    [c]
    E a interface única para diversas linguagens de script nos ajuda a usar tais
    linguagens sem nos preocuparmos com suas APIs, como veremos adiante.

    Porém essa interface única é uma de duas grandes partes do sistema. Ela permite
    ao C++ usar uma linguagem de script. O caminho inverso é a...

    [d]
    Exportação.
    Para os scripts usarem coisas de C++, como classes e funções do seu programa, 
    ele necessita conhecer tais coisas. Pra isso é necessário exportar a interface
    de C++ que você quiser para os scripts.
    E lá atrás em 2011 resolvemos isso usando o SWIG, o qual veremos mais adiante.

[SLIDE 3 - a, b, c, d, e] (omar)
    [a]
    Então há um ano atrás já tinhamos um sistema de scripts funcionando. Mas como descobrimos,
    e vamos explicar depois, o SWIG tinha vários problemas.
    Quando estavamos discutindo o que faríamos de TCC no final do ano passado, surgiu a idéia
    de melhorar o sistema de scripts. Já tinhamos feito o sistema antigo juntos, então resolvemos
    continuar juntos no TCC.
    
    E assim surgiu o...
    
    [b]
    Projeto Ouroboros.
    O ponto principal desse "novo" projeto, como vocês já devem imaginar, é...

    [c]
    Resolver os problemas do SWIG.
    E um ponto importante aqui é como decidimos resolver esse problema. Mas iremos explicar isso
    melhor depois.

    [d]
    Outra coisa importante que queríamos fazer era separar o sistema de scripts da UGDK, para
    lanca-lo como uma biblioteca separada. Afinal ele já não dependia dela pra nada e pode
    muito bem ser usado em qualquer aplicação, além de jogos.

    [e]
    E finalmente, outro ponto importante do projeto seria ele ser open-source. Tudo do grupo
    de jogos é software livre, e queremos manter isso, e publicar mais esse sistema separadamente.

-----------------------------------
Por trás de uma linguagem de script
-----------------------------------
[SLIDE 1] (wil)
  Linguagens de *script*: afinal o que são?
    - Compilação vs Interpretação
[SLIDE 2] (wil)
  Como o interpretador funciona: ele é um programa como qualquer outro.
[SLIDE 3] (wil)
  Confusão entre linguagens: nativa e virtual
[SLIDE 4] (wil)
  Máquinas virtuais: o interpretador é dividido em duas partes
[SLIDE 5] (wil)
  API nativa
[SLIDE 6] (wil)
  Onde queremos mexer para fazer o sistema

------------------
 O Sistema
------------------
[SLIDE 1] (omar)
  Agora vou falar mais do sistema em si...

### Visão Geral

[SLIDE 2] (omar)
  Começando por uma visão geral dele.

[SLIDE 3 - a, b, c] (omar)
    [a]
    Primeiramente, vamos definir melhor o "problema" por trás do projeto.

    [b]
    Integrar C++ com uma ou mais linguagens de script de forma automatizada.
    As linguagems possam ser usadas ao mesmo tempo, e que um script numa linguagem
    possa ser trocado por um script em outra linguagem, sem alterar o código C++, nem
    seu funcionamento, dado que os scripts definam a mesma interface e realizem as
    mesmas ações.

    [c]
    E modelamos, de forma simplificada, o problema assim.
    Tanto a aplicação quanto as máquinas virtuais tem "módulos". No caso da máquinas virtuais,
    os módulos são os scripts.
    Esses módulos especificam uma interface que os outros módulos (inclusive do outro lado)
    podem usar.
    Nosso sistema se enfia bem no meio disso, facilitando o uso das coisas de um lado pelo outro.
    

[SLIDE 4 - a, b, c, d] (omar)
    [a]
    Basicamente queremos integrar esses dois "ambientes", e...

    [b]
    essa integração tem dois sentidos. Então temos dois casos para resolvermos.

    [c]
    O primeiro é a incorporação, que é a máquina virtual sendo usada pela aplicação.
    Ou seja, o C++ carregando e usando scripts.

    [d]
    O segundo caso é a exportação, que é coisas da aplicação sendo usadas pela máquina virtual.
    Chamamos esse caso de exportação pois como mencionamos antes, para isso funcionar as
    classes e funções da aplicação precisam ser exportadas para as máquinas virtuais.

    Agora vamos explicar esses casos mais a fundo, e a solução do ouroboros para eles.
    

### Incorporação

[SLIDE 5] (wil)
  Yay.

[SLIDE 6] (wil)
  O que é? Explicar o exemplo.

[SLIDE 7] (ambos)
  Por trás dos bastidores. Explicar por cima.

[SLIDE 8] (wil)
  Interface unificada: OPA
    - Classes principais: VirtualObj, VirtualMachine, ScriptManager
    - Implementação: bom e velho polimorfismo!

[SLIDE 9] (wil)
  Problema da tipagem: explicar exemplo e ressaltar dificuldades

### Exportação

[SLIDE 10] (omar)
    Agora vamos explicar melhor a exportação...

[SLIDE 11 - a, b, c, d] (omar)
    [a]
    Exportação é o processo de possibilitar que uma interface da aplicação,
    como um conjunto de tipos, funções, etc, possa ser usada pelas máquinas
    virtuais. 
    Por exemplo, suponha que temos essa interface em C++...

    [b]
    Uma classe e uma função. Pra simplicidade por enquanto não importa o que 
    tem dentro da classe.
    Queremos que nossos scripts possam usar a função bar e usar a classe MyClass.
    Preferencialmente de uma forma intuitiva e fácil de usar. Se MyClass é uma classe
    em C++, deverá ser uma (ou algo parecido com uma) classe na dada linguagem de script.
    Bar também deverá ser uma função nos scripts.
    Então queremos que os scripts usem isso mais ou menos assim...

    [c]
    em Lua...

    [d]
    e em Python.

    Isso pode parece simples, mas para isso funcionar é necessário código C++
    para exportar as coisas (o que chamamos de wrappers), e isso não é exatamente
    simples, como veremos a seguir.

[SLIDE 12] (wil)
  Por trás das câmeras: Lua. Explicar brevemente.

[SLIDE 13 - a, b] (omar)
    [a]
    Para exportar a função bar em Python não é muito diferente de Lua...

    [b]
    Você deve criar uma função que encapsula a que deseja exportar, converte
    os argumentos recebidos da máquina virtual, executa a função, converte
    o valor de retorno e devolve ele.
    Também deve criar uma tabela com as funções do módulo, e criar uma função
    que inicializa o módulo - ou seja, cria ele para que os scripts possam
    usá-lo.
    Mas enquanto a idéia é bem parecida entre as APIs dessas linguagems, como
    elas funcionam é bem diferente. Enquanto a do Lua funciona se baseando na
    pilha interna e índices, a do python trabalha entorno de ponteiros de PyObject,
    que é um objeto que representa um objeto qualquer em python.

[SLIDE 14 - a, b, c] (omar)
    [a]
    Como vocês podem ver...

    [b]
    É bem chato. Só para exportar 1 função pra 2 linguagens
    você deve escrever um código estranho, e isso porque ainda simplificamos esses
    exemplos, deixando de lado detecção de erros. 
    Imagina agora fazer isso para exportar um número maior de funções mais complexas...
    Logo...

    [c]
    Precisamos gerar esse código automaticamente. Como vocês devem ter notado, o
    código pra exportar uma função é bem repetitivo então não é dificil gerá-lo
    para N funções dado que você tenha metadados representando essas funções -
    nome delas, tipo de retorno, parametros que recebe, etc.

[SLIDE 15 - a, b, c, d, e, f, g] (omar)
    [a]
    A primeira solução de geração automática dos wrappers que usamos foi o...

    [b]
    SWIG. Usamos ele no sistema antigo que desenvolvemos no grupo de jogos.
    Escolhemos ele pois depois de pesquisar o que poderíamos usar para gerar
    wrappers para diferentes linguagens de script - o SWIG foi a UNICA opção.
    
    [c]
    Ele consegue gerar wrappers para Lua, Python e algumas outras linguagens
    de script, mas como fomos descobrindo com o passar do tempo, não é sem
    seus problemas...

    [d]
    O primeiro sendo que não permite herança de classes exportadas. E como
    definir uma classe abstrata em C++ para ser implementada de diferentes
    jeitos em script é algo bem intuitivo que algum programador queira fazer,
    isso manchou bem nosso conceito sobre o SWIG.

    [e]
    Segundo problema muito importante é que ele não reconheçe algumas estruturas
    ou elementos de código C++ que são relativamente comuns de usar, como classes
    aninhadas. Isso deve decorrer do fato que o SWIG foi feito originalmente
    para C, mas ainda assim é bem desanimador para nós...

    [f]
    Terceiro, complicações para gerenciar a memória. As máquinas virtuais tem seus
    próprios sistemas para gerenciar sua memória, deletando objetos que não
    são mais usados. Mas sua aplicação em C++ também deve ter algo parecido,
    então temos que tomar cuidado para que não ocorra segfaults pois algum lado
    tentou usar alguma coisa que o outro lado deletou. E o jeito que o SWIG nos
    provê para fazer esse gerenciamento é dificil de usar.

    [g]
    Finalmente, a interface do SWIG não é trivial de usar. Para que ele exporte
    algum módulo, você deve criar um arquivo .i com as instruções para a
    exportação, o que é bem estranho pra usar, e chato pra ficar mantendo num
    projeto.
    
    
[SLIDE 16 - a, b, c, d, e, f] (omar)
    [a]
    Então chegamos na segunda solução, que nós mesmos desenvolvemos e foi o
    principal ponto de desenvolvimento neste TCC...

    [b]
    O OPWIG. Nossa própria ferramenta de gerar wrappers diretamente a partir
    do código C++ da interface a ser exportada.

    [c]
    Suas principais partes são:

    [d]
    O Parser, que analisa o código C++ a ser exportado, gerando uma árvore
    de...

    [e]
    Metadados, que definem os elementos a serem exportados, seus atributos
    e a estrutura deles no código, como classes, funções, variáveis, etc.

    [f]
    E finalmente o gerador de código, que gera o código de wrapper para
    cada linguagem de acordo com os metadados. Essa parte do OPWIG define
    uma classe abstrata que representa o gerador de código pra uma linguagem,
    e deve ser implementado para que o OPWIG possa gerar wrapper para tal
    linguagem. Obviamente implementamos para Lua e Python por enquanto.

[SLIDE 17 - a, b, c, d] (omar)
    [a]
    O parser do OPWIG é algo bem importante e que nos deu bastante trabalho -
    ficamos quase o primeiro semestre inteiro para deixá-lo num estado
    funcional, conseguindo reconhecer namespaces, classes, funções e métodos,
    variáveis e enumerações.
    Para facilitar a criação do parser - que reconheçe a gramática do C++,
    usamos algo como vemos em Lab Prog I...

    [b]
    o Flex...

    [c]
    e o bison.

    [d]
    mas usamos flexc++ e o bisonc++ criados por um cara chamado Frank Brokken,
    que como vocês podem ver tem um humor meio estranho. Essas imagens são as
    que ele colocou nos sites das respectivas ferramentas.
    Esse flex e bison do brokken são versões mais avançadas do flex e bison
    antigos, gerando código C++ diretamente usando ferramentas mais modernas.


--------------------
 Conclusões
--------------------

[SLIDE 1] (wil)
  HUZZAH

[SLIDE 2] (wil)
  Funciona?
    - Testes com jogos
    - Testes de unidade e funcionalidade
    - Milestones

[SLIDE 3] (wil)
  Dificuldades: seguir slides.

[SLIDE 4] (wil)
  + Próximos passos
    - Lançamento
    - Melhorias
    - Outras linguagens
    - Metaprogramação!

