---
title: Ontwerpmethodologie voor FPGAs
:digital-electronics:ugent:fpga:
---

# Ontwerpmothodologie voor FPGAs

## Statecharts

## Combinational datapath

Om dingen te pipelinen is het nodig om overal flip-flips toe te voegen. Deze vormen dan een buffer
waardoor de waarden gebuffered kunnen worden.
Alle uitgangen en niet-constante operatoren moeten een flipflop krijgen.

Om een volledig pipeline te krijgen is het nodig om de operanden die slecht in de laatste
berekekingstrap gebruikt worden, te vertragen, door een flip flop toe te voegen voor elke trap in
de pipeline.

Optimalisatie methoden:

* Strength reduction of operators: Vermenivuldigers met constanten als 2de operand, kunnen
  vervangen worden door een shit operatie.
* Height reduction of the expression tree: Het is mogelijk om meerdere optelingen in parallel
  uit te voeren.

## Memory optimisation

Gewoon goed kijken naar de parameters die gedefinieerd worden in de opgave. Denk logisch na over hoe
latency, area en thoughput bepaald zouden worden op basis van de oplossing.

PARETO curves kunnen lezen en opstellen

* latency vs area
* latency vs throughput
* throughput vs area

-> denk na over welke curves je kan gebruiken. Als er een oplossing op 2 aspected, de beste
oplossing biedt, heeft het geen nut om die curve op te stellen => je kan op het zicht zien welke
oplossing het beste is.

## VHDL

## SystemC

## Schedueling

## Busses

Eerst de throughput van alle onderdelen bepalen

object/clock ticks * clock speed -> ANS * bytes/object -> ANS = bitrate


