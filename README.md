# FFVI_LLG
Final Fantasy VI Low-Level Game related questions, proofs and tables. See my LLG guide:
https://steamcommunity.com/sharedfiles/filedetails/?id=1820232312

## mcr_exp.xlsx
This is a file to help you find out exp distribution methods in the mine cart ride. Choose your formations in the mine cart ride and switch to that table. Filter each character's exp column to find the exp distribution that satisfies your requirement.

Status Table:
Status Number:  Number used to represent characters' status.
Locke, Char1, Char2: Character status. 1 represents alive. 0 represents KO.

Table Names:
8P: 1P, 2P, 1P, 2P, 2P for battle 1 to 5.
8P1R: 1P1R, 2P, 1P, 2P, 2P for battle 1 to 5.
6P5R: 1P1R, 4R, 1P, 2P, 2P for battle 1 to 5.

Exp Tables (8P, 8P1R, 6P5R):
Locke, Char1, Char2: Exp absorbed by that character.
c1 - c5: Status number representing the characters' status at the end of that battle. 

For example, the 16680 row in table 8P means:
Locke and Char2 survive the first battle.
Only Char2 survive the second battle.
Locke and Char2 survive the third battle.
All three survive the forth and fifth battle.
And by distributing exp like this, Locke gains 540 exp, Char1 gains 308 exp and Char2 gains 1004 exp.

## IAF_RNG.xlsx
This is a file to help you partially manipulate the IAF sequence. It should work on most versions (SNES/GBA/Android/IOS/Steam).

1. Choose your desired number of 1SF1SA formations (#) and switch to that table named “1SF1SA X #”.
2. Fly to Triangle Island. Do not walk into the forest. You want to clear your step counter and danger counter first. Walk around in the plain until you encounter some enemies.
3. Run away, but don’t move after you finish the battle. Save (not quicksave) the game and return to the title page.
4. Load the save slot you just saved and don’t move. Now you want to find out your current random seed. Every time you load a save, a random seed is generated. This random seed determines how many steps you need to walk to encounter your next enemies and the formations you will encounter. I won’t go into detail of the underlying mechanism. You just need to know how to take advantage of this. First, open the menu screen and write down your total number of steps at the bottom-right corner.
5. Now walk around in the plain until you encounter some enemies. Run away and write down your total number of steps again. Your first step count is the subtraction between your current step number and your old step number. 
6. Now look at the table. Filter the Encounter_1 column by the first step count you just got. If it results in only one row, you’re done. If it results in multiple rows, repeat step 5 & 6 to get your second (and even third) step count until you filter the Encounter_# columns and get only one row. This row’s Random_Seed is your current random seed.
7. Look at the Extra_Encounters column. This is the number of encounters you are away from the desired formations of IAF which is indicated in F1~F6 columns (F# means the #th battle; Value 1 represents 1SF1SA formation and value 0 represents 1SF2SA formation). Walk around and meet exactly Extra_Encounters groups of enemies. If Extra_Encounters is large, you may want to choose another random seed. Just reload the save and start from step (4) again.
8. Now board the airship. Congratulations. If you fly to the Floating Continent now, you will meet exactly those formations from columns F1~F6 in IAF sequence.

## iaf_exp_tables.RData
These tables have the same format as the mine cart ride tables. Since each exp table has 117,649 rows, it's not possible to put them in a excel file, so I save them as R data. You need to know a bit of R language to use them.

## veldt_table.xlsx
This table can be used to manipulate the Veldt formation and quickly find out your desired formation. It should be applicable to most of the versions. Please inform me if you encounter any problem.
You need to have some basic understanding of the mechanism behind the Veldt. If no, read this first:
http://www.rpglegion.com/ff6/algs/veldt.htm
For GBA/Android/IOS/Steam version, use this Veldt formation list instead:
https://gamefaqs.gamespot.com/boards/930370-final-fantasy-vi-advance/51923197
 
1. Fly to the Veldt. You need to reset the danger counter and step counter first. Walk around to encounter an enemy formation. After the battle, don't move. Save the game to a slot and go back to the title page.
2. Reload the slot you just saved. Open the menu page and write down the total number of steps. Now walk around to encounter you first enemy formation. You don’t want this enemy formation to be too far from your desired enemy formation. For example, if your target formation is from pack 54 and your current formation is from pack 1, then you need to go through ~50 battles to reach pack 54, which is too far. If that is your case, simply reload your save to change your random seed and your initial Veldt pack.
3. After your first battle, don’t move. Open menu page and write down your total number of steps again. Subtract this number by your last total number of steps to get your first step count. Filter Encounter_1 column by this number.
4. If the filtration results in only one row, you’re done. Otherwise, walk around to encounter your second enemy formation. Then repeat step 3 to get your second step count. Filter Encounter_2 column by this number. Check whether there is only one row now. If still not, you need to get your third step count. You should get only one row this time.
5. After you find out your current random seed, your future Veldt formations are also predictable. For example, if your next Veldt pack is 14 and column F1 is 3, you will encounter a single Harvester formation in your next battle. Similarly, If column F2 is 5, you will encounter Magna Roader (purple) and Magna Roader (red) formation in your second battle.
6. When the predicted formation is not what you want, you can skip that formation by encountering one or more enemy groups outside of the Veldt. For example, if your next formation number is 3 in column F3, but you want to meet formation 5 in that pack, which is in column F5, then you can play two battles outside of the Veldt, and then return to the Veldt to encounter your desired formation. In this way, you can determine which formation you will encounter in your next enemy pack.
7. Note that sometimes the formation number points to an empty formation slot. Then the algorithm will skip that formation number and choose the next available formation number in this pack. Available means that the corresponding formation slot is not empty and the formation has been unlocked (you have encountered that formation before).
