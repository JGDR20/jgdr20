---
layout: post
title:  "Roll20 Useful Tricks"
date:   2019-06-15 00:00:00 +0000
categories: [Games, D&D]
author: Joe
---
{% assign doubleOpen = '{{' %}
Most of these are for the D&D 5e by Roll20 (OGL) sheet and could probably be done in a better way.<!-- more -->

**Work in Progress**

# Spells
## Arcane Ward

Gives the caster a temp hp boost equal to twice the spell’s level, up to a max of twice the spellcasting level + intelligence (mod).
Shows the new temp HP value that in the chat log for reference.
Add the below to the bottom of Abjuration Spells Description text (for primary class spellcasters):

`Temp HP = [[{[[@{hp_temp}+2*1]],[[2*@{base_level}+intelligence_mod]]}kl1]]`

![][Arcane_Ward]
> Example output of Shield spell with additional Temp HP from Arcane Ward displayed

This can (should) probably be modified in a similar way to Disciple of Life healing spells as below for spells with a Higher Casting option.

## Disciple of Life

Adds additional healing to the target of the spell level plus 2.
This one is a bit of a bodge; adds the additional healing as a query in the healing roll. Using Cure Wounds as an example:

`[[?{Cast at what level?|1,0|2,1|3,2|4,3|5,4|6,5|7,6|8,7|9,8}+1]]d8 + [[?{Cast at what level?|1,0|2,1|3,2|4,3|5,4|6,5|7,6|8,7|9,8}+1]] + 2`

The *Cast at what level?* query has the same name as the normal prompt for higher-level casting so this special prompt-box gets re-used. Nice!  
However, it looks like the Cast at what level? query is zero-indexed or something because, without the `|1,0|2,1|…` referencing, the spell-tracking tries to take a spell off the next level (e.g. you choose Level 3 and it takes one off your Level 4 slots). The |pipe|separated| options give the user the 'real' spell level then substitutes the appropriate behind-the-scenes level to the API to do its magic.
The flipside of this is that you then need to +1 to the result to get the correct number of dice rolls.

As this takes over the calculation for the *Higher Casting* effect you can blank out that section.

![][Cure_Wounds_template]
> Cure Wounds settings, note the macro text in the Healing field and 0d6+0 in the Higher Level Cast Damage field

![][Cure_Wounds_roll]
> Cure Wounds cast at level 5 with expanded roll, showing the 5 dice rolls + 5 extra for spell level + 2 bonus (+5 for Int)

## Blessed Healer

Kicks back some bonus healing to the caster using the same calculation as Disciple of Life above, allowing them to focus more on healing others than themselves.
Again, bit of a bodge; uses the Range text to show the bonus healing.
Alternatively could be used in Damage2 whilst the Disciple of Life bonus could go in Damage[1].

Tack the below onto the end of whatever is in the Range field:

`- Cast using level [[?{Cast at what level?|1,0|2,1|3,2|4,3|5,4|6,5|7,6|8,7|9,8}+1]] slot. Blessed Healer: Caster is also healed for [[?{Cast at what level?|1,0|2,1|3,2|4,3|5,4|6,5|7,6|8,7|9,8}+1 + 2]] HP for a total of [[{@{hp|max},(?{Cast at what level?|1,0|2,1|3,2|4,3|5,4|6,5|7,6|8,7|9,8}+1 + 2 + @{hp})}dh1]] if they healed someone other than themself.`

![][Cure_Wounds_roll_range]
> Cure Wounds cast at level 2

As you can see in the screenshot, this seems to break the spell slot tracking – so… needs more work.

# Feat Mods

## Great Weapon Fighting (Fighter)

Gives you a (mandatory) re-roll on any _Damage_ roll of 1 or 2 where you must use the 2nd result (even if it is, itself, a 1 or 2).  
I haven't seen a way to do this in the actual rollable attacks in the Character Sheet but you can finagle it as an _Ability_ instead. Roll the attack from the sheet as normal then, in the chat box, press the \[up\] arrow once to get the command that corresponds to the attack. Copy this into a text editor for convenience (Notepad will do) then add `ro<2` directly after the damage roll in `{{ doubleOpen }}dmg1=...}`. As an example, below is the original command:

`@{Krodhal|wtype}&{template:atkdmg} {{ doubleOpen }}mod=+11}} {{ doubleOpen }}rname=Great-Arks}} {{ doubleOpen }}r1=[[@{Krodhal|d20}cs>20 + 5[STR] + 5[PROF] + 1[MAGIC]]]}} @{Krodhal|rtype}cs>20 + 5[STR] + 5[PROF] + 1[MAGIC]]]}} {{ doubleOpen }}attack=1}} {{ doubleOpen }}range=}} {{ doubleOpen }}damage=1}} {{ doubleOpen }}dmg1flag=1}} {{ doubleOpen }}dmg1=[[1d12 + 5[STR] + 1[MAGIC]]]}} {{ doubleOpen }}dmg1type=Slashing}} 0 {{ doubleOpen }}dmg2=[[0]]}} {{ doubleOpen }}dmg2type=Psychic}} {{ doubleOpen }}crit1=[[2d12[CRIT]]]}} {{ doubleOpen }}crit2=[[0[CRIT]]]}} 0 {{ doubleOpen }}desc=}}   {{ doubleOpen }}spelllevel=}} {{ doubleOpen }}innate=}} {{ doubleOpen }}globalattack=@{Krodhal|global_attack_mod}}} {{ doubleOpen }}globaldamage=[[0]]}} {{ doubleOpen }}globaldamagecrit=[[0]]}} {{ doubleOpen }}globaldamagetype=@{Krodhal|global_damage_mod_type}}} ammo= @{Krodhal|charname_output}`

And here is the modified version:

`@{Krodhal|wtype}&{template:atkdmg} {{ doubleOpen }}mod=+11}} {{ doubleOpen }}rname=Great-Arks}} {{ doubleOpen }}r1=[[@{Krodhal|d20}cs>20 + 5[STR] + 5[PROF] + 1[MAGIC]]]}} @{Krodhal|rtype}cs>20 + 5[STR] + 5[PROF] + 1[MAGIC]]]}} {{ doubleOpen }}attack=1}} {{ doubleOpen }}range=}} {{ doubleOpen }}damage=1}} {{ doubleOpen }}dmg1flag=1}} {{ doubleOpen }}dmg1=[[1d12`***`ro<2`***` + 5[STR] + 1[MAGIC]]]}} {{ doubleOpen }}dmg1type=Slashing }} 0 {{ doubleOpen }}dmg2=[[0]]}} {{ doubleOpen }}dmg2type=}} {{ doubleOpen }}crit1=[[2d12ro<2[CRIT]]]}} {{ doubleOpen }}crit2=[[0[CRIT]]]}} 0 {{ doubleOpen }}desc=}}   {{ doubleOpen }}spelllevel=}} {{ doubleOpen }}innate=}} {{ doubleOpen }}globalattack=@{Krodhal|global_attack_mod}}} {{ doubleOpen }}globaldamage=[[0]]}} {{ doubleOpen }}globaldamagecrit=[[0]]}} {{ doubleOpen }}globaldamagetype=@{Krodhal|global_damage_mod_type}}} ammo= @{Krodhal|charname_output}`

Save your version of the above as a new *Ability*, show it as a *Token Action* and you're good to go. Repeat for a roll with *Advantage* and you're golden.

You can call the Ability in another Ability (see [Nesting Abilities]) to chain attacks together, very handy for Fighters with *Extra Attacks* so you can do it all with a button push.  
Here's one for a Barbarian Half-Orc using *Action Surge*, *Fighting Spirit*, *Rapid Strike*, *Savage Attacker* and, of course, *Great Weapon Fighting*, all in one move:

```
/me goes f'ing berserk

@{Krodhal|wtype}&{template:traits} @{Krodhal|charname_output} {{ doubleOpen }}name=Action Surge}} {{ doubleOpen }}source=Class: Fighter}}

@{Krodhal|wtype}&{template:traits} @{Krodhal|charname_output} {{ doubleOpen }}name=Fighting Spirit}} {{ doubleOpen }}source=Class: Samurai}} {{ doubleOpen }}description=Xanathar: *15 temp HP}}

%{Krodhal|Great-Arks-Advantage}
%{Krodhal|Great-Arks-Advantage}
%{Krodhal|Great-Arks-Advantage}
%{Krodhal|Great-Arks-Advantage}
%{Krodhal|Great-Arks-Advantage}

@{Krodhal|wtype}&{template:traits} @{Krodhal|charname_output} {{ doubleOpen }}name=Rapid Strike}} {{ doubleOpen }}source=Class: Samurai}} {{ doubleOpen }}description=}}

%{Krodhal|Great-Arks}
%{Krodhal|Great-Arks}

@{Krodhal|wtype}&{template:traits} @{Krodhal|charname_output} {{ doubleOpen }}name=Savage Attacker}} {{ doubleOpen }}source=Feat: }} {{ doubleOpen }}description=Once per turn when you roll damage for a melee weapon attack, you can reroll the damage dice and use either total.

[[1d12]]}}

@{Krodhal|wtype}&{template:traits} @{Krodhal|charname_output} {{ doubleOpen }}name=Great Weapon Fighting}} {{ doubleOpen }}source=Class: Fighter}}
```

# Handy API scripts and Macros

You'll need a Pro subscription to Roll20 for some of these

## GroupInitiative

The GroupInitiative API script for bulk-rolling Initiative is available in the approved dropdown list. Once it is activated it can be prepared for D&D 5e rules using the below commands in the chat log:

```
!group-init --del-group 1
!group-init --add-group --Bare initiative\_bonus|current
!group-init --add-group --Bare npcd\_dex\_mod|current
```

This gets rid of the default calculation, adds the Player calculation as the primary group then the NPC calculation as the fall-back (assuming the NPC sheets don't have an `initiative_bonus` attribute set).  
  
This is only needed at the start of a campaign/game, see the [script notes] for more usage and stop by the author's [Patreon page][shdwjk Patreon Page] if you like it.

## Initiative Tracker

This one works well with *GroupInitiative* to add rounds to the turn order automatically. It can do a whole bunch more as well (including tracking effects like poisoning etc.) but I haven't really used those features.  
  
It's not available in the approved scripts dropdown but you can check out the [wiki article][InitTracker wiki article] that has a link to the code.

To use *Initiative Tracker* and *GroupInitiative* together make a new Macro (displayed in the *Macro Bar*) with the below text then highlight all tokens you want to include in the Turn Order and click the button:

```
!group-init
!tracker Round 1
!tracker Start
```

## Long Rest

This one comes with *5th Edition OGL by Roll20 Companion* which is available in the approved dropdown. It'll reset the Player characters' health and spell slots using the below command/Macro:

`!longrest <character name>`

I've prettied one up a little to make a Token Action:

`!longrest @{selected|character_name}
&{template:default} {{ doubleOpen }}name=Long Rest}} {{ doubleOpen }}@{selected|character_name} has completed a long rest}}`

Or you can make a list of Players to affect:

```
!longrest Adran
!longrest Gargax Norixius
!longrest Lanor Tilde
!longrest Lukian Nastacia
!longrest Volen
!longrest Hootie

&{template:default} {{ doubleOpen }}name=Long Rest}} {{ doubleOpen }}[[1t[Longrest-Description]]]}}
```

In the above a random entry from the custom *Longrest-Description* table is displayed as well for a bit of flavour.

[Arcane_Ward]: /assets/2019-06-15-Arcane_Ward.png "Example output of Shield spell with additional Temp HP from Arcane Ward displayed"
[Cure_Wounds_template]: /assets/2019-06-15-Cure_Wounds_template.png
[Cure_Wounds_roll]: /assets/2019-06-15-Cure_Wounds_roll.png
[Cure_Wounds_roll_range]: /assets/2019-06-15-Cure_Wounds_roll_range.png
[Nesting Abilities]: https://wiki.roll20.net/Macros#Nesting_Abilities
[script notes]: https://wiki.roll20.net/Script:Group_Initiative
[shdwjk Patreon page]: https://www.patreon.com/shdwjk
[InitTracker wiki article]: https://wiki.roll20.net/Script:Initiative_Tracker
