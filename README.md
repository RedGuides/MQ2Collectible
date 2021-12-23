# MQ2Collectible

- Command /collectible searches Achievements and provides readable output on the status of Collections and Collectibles
- TLO ${Collectible["collectible name"]} provides Collectible status and other properties

## Usage

```
/collectible collected|need|all|help log|bazaar|console optional: expansion|collection|collectible "name"

${Collectible["collectible name"].Collected; .Expansion; .Collection; .Both}
```

- /collectible parameters must be ordered as shown.
- The "name" must be enclosed by quotes.
- The Collection and its components are returned when searching by Collectible.
- Logfiles are appended to.

### Parameter Abbreviations

```
-cd|collected
 -n|need
 -a|all
 -h|h|help|(no parameters)
 -l|log
-bz|bazaar
-cs|console
 -e|expansion
-cn|collection
-cl|collectible
```

## Command Examples

When using the bazaar logfile option, please note:

- The bazaar logfile is provided for you to copy/paste to your Bazaar.ini. Results are appended, not replaced like an INI. Any additional parameters in the [collectible name] blocks are informational and have no bearing on Bazaar.mac.
- **collected** assumes you want to _sell_ collectibles and provides default sell (2000000pp) parameters
- **need** assumes you want to _buy_ collectibles and provides default buy (1pp) parameters
- **all** provides default _buy_ (1pp) and _sell_ (2000000pp) parameters
- Default buy and sell amounts are hardcoded.

**/collectible need console collection "Flame-Licked Clothing"**

**/collectible need log collection "Flame-Licked Clothing"**

Outputs to console, or to logfile:

Logs\Collectible\server_charname_need.ini
```
MQ2Collectible: Flame-Licked Clothing, Claws of Veeshan
-------------------------------------------------------------------------------
Flame-Licked Belt [NEED]
```

**/collectible -a -l -cn "Flame-Licked Clothing"**

Logfile: Logs\Collectible\server_charname_all.ini
```
[MQ2Collectible] Flame-Licked Clothing, Claws of Veeshan
-----------------------------------------------------------------------------------------
[COLLECTED] Flame-Licked Trousers
[COLLECTED] Flame-Licked Vest
[COLLECTED] Flame-Licked Cuffs
[NEED]      Flame-Licked Boots
[COLLECTED] Flame-Licked Undershirt
[COLLECTED] Flame-Licked Cap
[NEED]      Flame-Licked Pouch
[NEED]      Flame-Licked Belt
```

**/collectible -a -bz -e "Terror of Luclin"**

Outputs Bazaar.mac compatible logfile with _all_ the collectibles from the Terror of Luclin expansion, a very long list:

Logfile: Logs\Collectible\server_charname_all_baz.ini
```
[Strangely Worded Note about Hearth]
Collected=0
Collection=Found Paper, Terror of Luclin
BuyPriceMin=1
BuyPriceMax=1
MinBuyCount=1
SellPriceMin=2000000
SellPriceMax=2000000
[Broken Wrist Shackles]
Collected=1
Collection=Breaker of Chains, Terror of Luclin
BuyPriceMin=1
BuyPriceMax=1
MinBuyCount=1
SellPriceMin=2000000
SellPriceMax=2000000

etc.
```

## TLO Examples

 Returns -1 if the collectible is not found, otherwise 0|1 depending on whether it has been collected.
```
${Collectible["collectible name"].Status}
```
Returns -1 if not found, otherwise the name of the Expansion, Collection, or both.
```
${Collectible["Broken Wrist Shackles"].Status}     -> 0|1
${Collectible["Broken Wrist Shackles"].Expansion}  -> "Terror of Luclin"
${Collectible["Broken Wrist Shackles"].Collection} -> "Breaker of Chains"
${Collectible["Broken Wrist Shackles"].Both}       -> "Breaker of Chains, Terror of Luclin"
${Collectible["Brasse's Brassiere"].Status}        -> -1
${Collectible["Brasse's Brassiere"].Collection}    -> -1
```

**Config\MQ2Hud.ini**

Enables you to view the status of collectibles when mousing over them.

You may want to use something like boxhud instead... or perhaps that functionality will be added here.

```
[Elements]
CollectionItemMOI = 7,30,0,255,255,255 ,${If[${EverQuest.LastMouseOver.MouseOver},${If[${FindItem[=${EverQuest.LastMouseOver.Tooltip}].Collectible},${If[${Collectible[${FindItem[=${EverQuest.LastMouseOver.Tooltip}]}]},Collected,Need]},""]},""]}
CollectionItemMOB = 7,30,0,255,255,255 ,${If[${EverQuest.LastMouseOver.MouseOver},${If[${FindItem[=${EverQuest.LastMouseOver.Tooltip}].Collectible},${If[${Collectible[${FindItemBank[=${EverQuest.LastMouseOver.Tooltip}]}]},Collected,Need]},""]},""]}
```
