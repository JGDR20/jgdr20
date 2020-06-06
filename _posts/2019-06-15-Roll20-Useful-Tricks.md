---
layout: post
title:  "Roll20 Useful Tricks"
date:   2019-06-15 00:00:00 +0000
categories: [Games, D&D]
author: Joe
---
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

As this takes over the calculation for the Higher Casting effect you can blank out that section.

![][Cure_Wounds_template]
> Cure Wounds settings, note the macro text in the Healing field and 0d6+0 in the Higher Level Cast Damage field

![][Cure_Wounds_roll]
> Cure Wounds cast at level 5 with expanded roll, showing the 5 dice rolls + 5 extra for spell level + 2 bonus (+5 for Int)

## Blessed Healer

Kicks back some bonus healing to the caster using the same calculation as Disciple of Life above, allowing them to focus more on healing others than themselves.
Again, bit of a bodge; uses the Range text to show the bonus healing.
Alternatively could be used in Damage2 whilst the Disciple of Life bonus could go in Damage[1].

Tack the below onto the end of whatever is in the Range field:

` - Cast using level [[?{Cast at what level?|1,0|2,1|3,2|4,3|5,4|6,5|7,6|8,7|9,8}+1]] slot. Blessed Healer: Caster is also healed for [[?{Cast at what level?|1,0|2,1|3,2|4,3|5,4|6,5|7,6|8,7|9,8}+1 + 2]] HP for a total of [[{@{hp|max},(?{Cast at what level?|1,0|2,1|3,2|4,3|5,4|6,5|7,6|8,7|9,8}+1 + 2 + @{hp})}dh1]] if they healed someone other than themself.`

![][Cure_Wounds_roll_range]
> Cure Wounds cast at level 2

As you can see in the screenshot, this seems to break the spell slot tracking – so… needs more work.

[Arcane_Ward]: /assets/2019-06-15-Arcane_Ward.png "Example output of Shield spell with additional Temp HP from Arcane Ward displayed"
[Cure_Wounds_template]: /assets/2019-06-15-Cure_Wounds_template.png
[Cure_Wounds_roll]: /assets/2019-06-15-Cure_Wounds_roll.png
[Cure_Wounds_roll_range]: /assets/2019-06-15-Cure_Wounds_roll_range.png
