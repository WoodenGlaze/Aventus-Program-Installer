# PowerShell Installatie Script

## Wat het doet?
Het powershell script gaat na of de programma's die benodigd zijn zich in de huidige map bevinden, zo ja, dan installeert het script deze, zo niet, dan download het script het via de officiele URLs.

## Wat het niet doet?
Het script ruimt nog niet op na zichzelf, ook controleert het script niet of de programma's mogelijk al geinstalleerd zijn.

## Gebruik
Afhankelijk van de script execution policy op de machine van de gebruiker, zijn er mogelijk extra stappen nodig, omdat windows de functie om PowerShell scripts uit te voeren zonder verificatie tegenhoud(Met goede redenen!)

Om hier omheen te komen moet de gebruiker dit commando gebruiken in een powershell window met administrator rechten:

`Set-ExecutionPolicy -ExecutionPolicy Unrestricted`

### Omdat de code hierboven de controle uitschakeld, is het ten strengste aangeraden om later het volgende commando uit te voeren:

`Set-ExecutionPolicy -ExecutionPolicy Restricted`

## Auteur & Bronnen
Auteur: 
Dion "Glaze" Sorel - 25605OLVMA1(2022-2023)

Bronnen:
Microsoft - Powershell Documentatie
StackOverflow - Voorbeelden van bepaalde functies
LazyAdmin - Een manier om te downloaden zonder Internet Explorer te gebruiken
VMWare - Hun documentatie m.b.t. de installer.