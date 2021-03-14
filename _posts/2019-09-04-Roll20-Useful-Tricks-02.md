---
layout: post
title:  "Roll20 Useful Tricks #2"
date:   2019-09-04 00:00:00 +0000
categories: [Games, D&D]
author: Joe
---
{% assign doubleOpen = '{{' %}
# Advantage and Crit macros

As characters grow older and more experienced they start to gather Feats, Items or other oddities that change how their attacks work. At some point these changes may be too much for the standard attacks in the Roll20 character sheets to accommodate, which is where Macros and Abilities come in.

This post will detail separating attack and damage rolls whilst making the attack names clickable, just like the standard attacks from the character sheet, but with more flexibility.<!-- more -->

I've already written out some useful Macros [here][Roll20 Tricks 01] but I've come across another feature whilst doing some housekeeping on a Fighter. The Great and Mighty Krodhal is a half-orc fighter who has accumulated the following combat-relevant Feats and Traits:

- Savage Attacks (Xanathar's Guide to Everything) - an extra die on crits  
    This one is easy enough to handle, just modify the Crit field in the Weapon Attack to add the extra die
- Great Weapon Fighting - reroll 1s and 2s in Damage Rolls, keep the new value  
    This needs tweaks to the dice being rolled to happen automatically
- Action Surge - get another action  
    On it's own no big deal but when combined with Extra Attack at higher levels it leads to quite a few attack rolls
- Extra Attack - currently Krodhal gets 3 attacks per action, so 6 with Action Surge
- Fighting Spirit - Advantage on weapon attacks for the current turn (plus temp hit points)
- Orcish Fury - extra damage die (one-time use)
- Savage Attacker - reroll one damage roll, replace a previous roll if desired
- Rapid Strike - Convert one Advantaged attack to two normal attacks

It's a lot for the chat log to take, especially with keeping track of low rolls that can be replaced and switching between advantage and normal rolls. As far as I know, the Repeating Attack section doesn't allow the flexibility to modify the rolls as much as I'd like.

Enter [Nesting Abilities], and the **rname** (roll name) and **rnamec** (roll name crit) clickable parameters in the OGL template. I've recreated the attack roll as two macros (normal and advantage) and linked the attack name buttons to two damage macros (regular and crit), depending on the outcome of the attack so I can just click to roll damage. I can then create a parent macro that calls the appropriate combination of attacks and click for damage on each success, grouping the damage in the chat log to make totalling it up easier.

An example for Krodhal:

- Quick summary of feats being used
- 5 attacks with advantage
- 2 normal attacks
- Clickable attack names to roll damage later
- Reroll 1s and 2s in damage
- Handle crit attacks
- Extra damage roll to possibly replace a prior lousy roll

![][Krodhal Attack Rolls]
> Krodhal Attack Rolls

As you can see from the screenshot there is still a lot going into the chat log but this is all produced from a single button click. Now I can go through the list and click the name of each attack that succeeded to get the damage rolls underneath. Also, notice that the name of the attack has changed on the critical hit which can be useful if the effect of a crit is wildly different to normal.

Assuming Krodhal was attacking a creature with an AC of 32, the results look like the below, with 1 crit and 3 normal successes:

![][Krodhal Damage Rolls]
> Krodhal Damage Rolls

Looking through this I can see that I can replace the 15 with the spare 19 from Savage Attacker, giving \[17\] + 19 + 17 + 20 for a total of 73 - not bad! Also, I can see that the \[critical hit\] has some extra damage or takes the/a head of the creature - useful to see when a crit is rolled but useless otherwise.

The above has been achieved with 5 **Abilities**, added to the Attributes & Abilities tab in the 5e OGL Character Sheet; 2 attacks (advantage and normal), 2 damages (crit and normal) and a parent that ties them together and adds the extra info like the Buffs text.

This information is referenced from:

- [Roll20 Wiki - 5e OGL Roll Templates][5e OGL Roll Templates]  
    Information on the templates including blank templates of the templates
- [Roll20 Wiki - Dice Reference][Dice Reference]  
    Messing around with dice rolls
- [Roll20 Forum - Clickable Nested Character Ability][Clickable Nested Character Ability]  
    Where forum users detailed the dark art of making a clickable button link to an Attribute (thanks so much!).

# Attacks

## Advantage

```
%Vorpal-Advantage-Atk

@{Krodhal|wtype}&{template:atk}
{{ doubleOpen }}mod=+[[@{Krodhal|strength_mod} + @{Krodhal|pb} + 3]]}}
{{ doubleOpen }}rname=[Vorpal Greatsword](~Krodhal|Vorpal-Dmg)}}
{{ doubleOpen }}rnamec=[Headshot!](~Krodhal|Vorpal-Dmg-Crit)}}
{{ doubleOpen }}r1=[[@{Krodhal|d20}cs>20 + @{Krodhal|strength_mod}[STR] + @{Krodhal|pb}[PROF] + 3[MAGIC]]]}}
{{ doubleOpen }}advantage=1}}
{{ doubleOpen }}r2=[[@{Krodhal|d20}cs>20 + @{Krodhal|strength_mod}[STR] + @{Krodhal|pb}[PROF] + 3[MAGIC]]]}}
@{Krodhal|charname_output}
```

Here the key sections are:

- `{{ doubleOpen }}rname=[Vorpal Greatsword](~Krodhal|Vorpal-Dmg)}}`  
    This is the regular attack roll name, linked to the regular damage roll
- `{{ doubleOpen }}rnamec=[Headshot!](~Krodhal|**_Vorpal-Dmg-Crit_**)}}`  
    This is the crit attack roll name, linked to the crit damage roll
- `{{ doubleOpen }}advantage=1}}`  
    Forces the roll to be at advantage, can be replaced with `@{Krodhal|rtype}` to use the Advantage toggle value on the character sheet  
    Can be used with advantage/always/disadvantage/normal=1
- `{{ doubleOpen }}r2=[[@{Krodhal|d20}cs>20 + @{Krodhal|strength_mod}[STR] + @{Krodhal|pb}[PROF] + 3[MAGIC]]]}}`  
    Specifies the second (advantage) roll for box 2, can be removed if `{{ doubleOpen }}normal=1}}` is used

I don't think that specifying the character `Krodhal|` is required but it feels safer. Using the `~` tilde instead of the usual `%` definitely is though!

# Damage

## Crit

```
%Vorpal-Dmg-Crit

@{Krodhal|wtype}&{template:dmg}
{{ doubleOpen }}rname=}} {{ doubleOpen }}range=}}
{{ doubleOpen }}damage=1}}
{{ doubleOpen }}dmg1flag=1}}
{{ doubleOpen }}dmg1=[[2d6ro<2 + @{Krodhal|strength_mod}[STR] + 3[MAGIC]]]}}
{{ doubleOpen }}dmg1type=Slashing}}
0
{{ doubleOpen }}crit=1}}
{{ doubleOpen }}crit1=[[7d8ro<2[CRIT]]]}}
{{ doubleOpen }}crit2=}}
0
{{ doubleOpen }}desc=Crits either take off the head or deals [[6+1]]d8 extra}}
@{Krodhal|charname_output}
```

You can see the Crit Damage roll is named `Vorpal-Dmg-Crit`, which is what the `rnamec` link points to in the attack roll.

# Parent

```
%Hulk-Out-V

/me goes f'ing berserk

@{Krodhal|wtype}&{template:traits} @{Krodhal|charname_output} {{ doubleOpen }}name=Buffs}} {{ doubleOpen }}description=Action Surge
Fighting Spirit (*15 temp HP)
Rapid Strike
Orcish Fury (+[[d6]])
Savage Attacker
+Savage Attacks (extra crit die)
Great Weapon Fighting}}

@{Krodhal|wtype}&{template:traits} @{Krodhal|charname_output} {{ doubleOpen }}name=Vorpal Shit}} {{ doubleOpen }}source=Weapon}} {{ doubleOpen }}description=Ignores resistance to slashing damage.}}

%{Krodhal|Vorpal-Advantage-Atk}
%{Krodhal|Vorpal-Advantage-Atk}
%{Krodhal|Vorpal-Advantage-Atk}
%{Krodhal|Vorpal-Advantage-Atk}
%{Krodhal|Vorpal-Advantage-Atk}
%{Krodhal|Vorpal-Atk}
%{Krodhal|Vorpal-Atk}

@{Krodhal|wtype}&{template:traits} @{Krodhal|charname_output} {{ doubleOpen }}name=Savage Attacker}} {{ doubleOpen }}source=Feat: }} {{ doubleOpen }}description=Once per turn when you roll damage for a melee weapon attack, you can reroll the damage dice and use either total.}}

%{Krodhal|Vorpal-Dmg}
```

And here is what ties it together, a bit of blurb, a list of attack Abilities being called, some more blurb and then an extra damage Ability.

You can definitely choose to have all the code stuffed into one Ability but separating out the repeating rolls makes it easier to read, make changes in fewer places and re-use the rolls in other Abilities (or just roll them on their own). Plus this one would be enormous.

Sidenote: If you're wondering how this character has a +17 attack modifier I managed to snag a Belt of Giant Strength and a +3 greatsword from a dragon hoard. The DM knew I'd been after the belt for a while - I knew he cared!

[Roll20 Tricks 01]: {% post_url 2019-06-15-Roll20-Useful-Tricks %}
[Nesting Abilities]: https://wiki.roll20.net/Macros#Nesting_Abilities
[Krodhal Attack Rolls]: /assets/2019-09-04-Krodhal_Attack_Rolls.png
[Krodhal Damage Rolls]: /assets/2019-09-04-Krodhal_Damage_Rolls.png
[5e OGL Roll Templates]: https://wiki.roll20.net/5e_OGL_Roll_Templates
[Dice Reference]: https://wiki.roll20.net/Dice_Reference
[Clickable Nested Character Ability]: (https://app.roll20.net/forum/post/6173350/clickable-nested-character-ability)  