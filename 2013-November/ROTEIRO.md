
[SLIDE 1] (ambos)
  Apresentação (talvez destacar a parte "automatizada")

[SLIDE 2] (wil)
  Índice: falar a estrutura da apresentação

Introdução: Motivações e objetivos
----------------------------------

[SLIDE 1] (omar)
  Vamos começar contando como surgiu a ideia desse trabalho. Como vocês verão,
  o projeto foi feito primeiramente como parte de nossas tramoias no USPGameDev,
  e depois aprimorado para se tornar o que é hoje.

[SLIDE 2] (omar)
  Sistema de scripts da UGDK (explicar o que é a UGDK)
    - Queríamos usar scripts, mas não decidíamos entre Lua e Python
    - Decidimos usar ambos
    - Explicar a imagem do horus
    - Para isso, fizemos o sistema de script, que automaticamente abstrai qual
      a linguagem usada.
    - Ressaltar que parte do sistema era o SWIG quem fazia

[SLIDE 3] (omar)
  Proposta de TCC: nome
    - SWIG sux
    - Emancipação do sistema de script para o glorioso PROJETO OUROBOROS

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

O Sistema
---------

[SLIDE 1] (omar)
  Oooooooi gente.

### Visão Geral

[SLIDE 2] (omar)
  Vamos dar uma visão geral do sistema.

[SLIDE 3] (omar)
  Como modelamos o problema, e qual a solução que propomos.

[SLIDE 4] (omar)
  Os causos: incorporação e exportação

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
  Sbrubles

[SLIDE 11] (omar)
  O que é? Explicar exemplo.

[SLIDE 12] (wil)
  Por trás das câmeras: Lua. Explicar brevemente.

[SLIDE 13] (omar)
  Por trás das câmeras: Python. Explicar brevemente.

[SLIDE 14] (omar)
  RAGEQUIT. Solução alternativa: gerar código!  

[SLIDE 15] (omar)
  Primeira solução: SWIG. IT SUX

[SLIDE 16] (omar)
  Segunda solução: OPWIG. Does not sux.
    - Parsear código declarativo em C++
    - Construir metadados
    - Gerar wrappers usando especificação apropriada

[SLIDE 17] (omar)
  Ferramentas usadas para parsear: flexc++ e bisonc++.
    - Falar do Brokken.
    - Falar de gramáticas.
    - Falar que estamos gerando código para fazer um gerador de códigos.

Conclusões
----------

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

