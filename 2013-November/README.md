
Gradutation Paper Presentation
==============================

This presentation is in portuguese, and thus so will be this text.

Introdução: Motivações e objetivos
----------------------------------
  Vamos começar contando como surgiu a ideia desse trabalho. Como vocês verão,
  o projeto foi feito primeiramente como parte de nossas tramoias no USPGameDev,
  e depois aprimorado para se tornar o que é hoje.
  + Sistema de scripts da UGDK
    - Imagem do horus
    - Ressaltar que parte do sistema era o SWIG quem fazia
  + Proposta de TCC
    - SWIG sux
    - Emancipação do sistema de script para o glorioso PROJETO OUROBOROS

Alguns conceitos
----------------
  + Linguagens de *script*: afinal o que são?
  + Máquinas virtuais
    - APIs
    - Compilador/interpretador

O sistema
---------
  + Visão geral

### Incorporação
  + O que é?
    - Exemplo com código de uso
  + \comofas
    - APIs das máquinas viruais
    - Exemplos de funções que pegam valores primitivos
    - Ressaltar problema da diferença de interfaces entre linguagens
  + Interface unificada: OPA
    - Classes principais: VirtualObj, VirtualMachine, ScriptManager
  + Implementação
    - Bom e velho polimorfismo!
  + Problema da tipagem
    - Ilustrar exemplo
    - Falar que o OPWIG resolve, e será visto adiante

### Exportação
  + O que é?
    - Exemplo com código de uso
  + \comofas
    - Protocolos de exportação de módulos
    - Processo exaustivo =/
  + Gerador de wrappers: OPWIG
    - Exemplo de análise/geração simples
  + Implementação:
    - Primeiramente com SWIG
    - Agora com OPWIG
  + O que ele faz:
    - Parsear código declarativo em C++
    - Gerar wrappers usando especificação apropriada

Conclusões
----------
  + Funciona?
    - Milestones
  + O que falta?
    - TODO
  + Próximos passos
    - Software livre
    - GitHub
    - Outras linguagens
    - Metaprogramação!

