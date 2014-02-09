
Master Thesis Proposal Presentation
==============================

This presentation is in portuguese, and thus so will be this text.

Introdução
----------
  + Moticação: Sistema de scripts da UGDK
  + Proposta do Projeto

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
  + Classes principais: VirtualObj, VirtualMachine, ScriptManager

### Exportação
  + O que é?
    - Exemplo com código de uso
  + \comofas
    - Protocolos de exportação de módulos
    - Processo exaustivo =/
  + Gerador de wrappers: OPWIG
  + O que ele faz:
    - Parsear código declarativo em C++
    - Gerar wrappers usando especificação apropriada

Organização do Projeto
----------
  + Ferramentas
  + Testes
  + Distribuição

