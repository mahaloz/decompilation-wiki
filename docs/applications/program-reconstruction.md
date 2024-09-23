# Program Reconstruction
When source code is unavailable for a compiled program, users may want to recover the source code, so they can make edits to it and recompile it. 
In the video game scene, this can be useful for modding. This can also be useful for porting a program to a new platform. 

## Video Games
Reverse engineers whom also love playing video games often reverse their favorite games. 
In some cases, they go as far as attempting to recompile the entire project. 
However, to recompile the project, they first need to recover compilable code. 
In these cases, practitioners often use a decompiler to first get pseudo-C, then modify it to make it compilable. 

The end goal of these projects is to recover program source that will recompile to a byte-match of the original binary. 
Most projects include a percent completion of the estimated program recompilation.

Many of the games in this list were collected from GitHub projects, individuals, or popular blog posts [^1].

| Game                                                                                                              | Original Platform | Completed |
|-------------------------------------------------------------------------------------------------------------------|-------------------|-----------|
| [Super Mario 3D Land](https://github.com/3dsdecomp/RedPepper)                                                     | 3DS               |           |
| [Ambermoon](https://github.com/Pyrdacor/Ambermoon.net)                                                            | Amiga             | ✅         |
| [The Settlers I](https://github.com/freeserf/freeserf)                                                            | DOS               | ✅         |
| [Donkey Kong '94](https://github.com/CelestialAmber/DKGBDisasm)                                                   | GB                | ✅         |
| [Kirby's Dream Land](https://github.com/huderlem/kirbydreamland)                                                  | GB                | ✅         |
| [Metroid II: Return of Samus](https://github.com/Vashy777/metroid2)                                               | GB                |           |
| [Pokémon Red and Blue](https://github.com/pret/pokered)                                                           | GB                | ✅         |
| [Pokémon Yellow](https://github.com/pret/pokeyellow)                                                              | GB                | ✅         |
| [Super Mario Land](https://github.com/kaspermeerts/supermarioland)                                                | GB                | ✅         |
| [Super Mario Land 3: Wario Land](https://github.com/Kak2X/wl)                                                     | GB                | ✅         |
| [Banjo Kazooie Grunty's Revenge](https://github.com/jellees/bkgr)                                                 | GBA               |           |
| [Fire Emblem: The Sacred Stones](https://github.com/FireEmblemUniverse/fireemblem8u)                              | GBA               |           |
| [Golden Sun](https://github.com/gsret/goldensun)                                                                  | GBA               |           |
| [Harvest Moon: Friends of Mineral Town](https://github.com/StanHash/fomt)                                         | GBA               |           |
| [Kirby & The Amazing Mirror](https://github.com/jiangzhengwenjz/katam)                                            | GBA               |           |
| [Metroid - Zero Mission](https://github.com/YohannDR/mzm)                                                         | GBA               |           |
| [Pokémon Emerald](https://github.com/pret/pokeemerald)                                                            | GBA               |           |
| [Pokémon FireRed and LeafGreen](https://github.com/pret/pokefirered)                                              | GBA               |           |
| [Pokémon Mystery Dungeon: Red Rescue Team](https://github.com/pret/pmd-red)                                       | GBA               |           |
| [Pokémon Ruby and Sapphire](https://github.com/pret/pokeruby)                                                     | GBA               |           |
| [Sonic Advance 2](https://github.com/freshollie/sa2)                                                              | GBA               |           |
| [The Legend of Zelda: Minish Cap](https://github.com/zeldaret/tmc)                                                | GBA               |           |
| [Links Awakening DX](https://github.com/zladx/LADX-Disassembly)                                                   | GBC               |           |
| [Pokémon Crystal](https://github.com/pret/pokecrystal)                                                            | GBC               | ✅         |
| [Pokémon Gold and Silver](https://github.com/pret/pokegold)                                                       | GBC               | ✅         |
| [Pokémon Pinball](https://github.com/pret/pokepinball)                                                            | GBC               | ✅         |
| [The Legend of Zelda: Link's Awakening DX HD](https://github.com/zladx/LADX-Disassembly)                          | GBC               | ✅         |
| [Wario Land 3](https://github.com/froggestspirit/wland3)                                                          | GBC               | ✅         |
| [Animal Crossing](https://github.com/Prakxo/ac-decomp/)                                                           | GameCube          |           |
| [Chibi-Robo: PIA](https://github.com/eavpsp/cbr_decomp)                                                           | GameCube          |           |
| [Kirby Air Ride](https://github.com/doldecomp/kar)                                                                | GameCube          |           |
| [Luigi's Mansion](https://github.com/Sage-of-Mirrors/zmansion)                                                    | GameCube          |           |
| [Mario Kart: Double Dash!!](https://github.com/SwareJonge/mkdd)                                                   | GameCube          |           |
| [Metroid Prime](https://github.com/PrimeDecomp/prime)                                                             | GameCube          |           |
| [Metroid Prime 2](https://github.com/PrimeDecomp/echoes)                                                          | GameCube          |           |
| [Paper Mario: The Thousand-Year Door](https://github.com/NWPlayer123/PaperMario2)                                 | GameCube          |           |
| [Pikmin 1 & 2](https://github.com/projectPiki/pikmin)                                                             | GameCube          |           |
| [Super Smash Bros. Melee](https://github.com/doldecomp/melee)                                                     | GameCube          |           |
| [The Legend of Zelda: Twilight Princess](https://github.com/zeldaret/tp)                                          | GameCube          |           |
| [The Legend of Zelda: Wind Waker](https://github.com/zeldaret/tww)                                                | GameCube          |           |
| [Streets of Rage 2](https://www.sor2newera.com/)                                                                  | MegaDrive         | ✅         |
| [Animal Forest](https://github.com/zeldaret/af/)                                                                  | N64               |           |
| [Banjo Kazooie](https://github.com/n64decomp/banjo-kazooie)                                                       | N64               |           |
| [Castlevania 64](https://github.com/blazkowolf/cv64)                                                              | N64               |           |
| [Diddy Kong Racing](https://github.com/DavidSM64/Diddy-Kong-Racing)                                               | N64               |           |
| [Dinosaur Planet](https://github.com/zestydevy/dinosaur-planet)                                                   | N64               |           |
| [Donkey Kong 64](https://github.com/gitlab.com/dk64_decomp/dk64)                                                  | N64               |           |
| [Dr. Mario 64](https://github.com/AngheloAlf/drmario64)                                                           | N64               |           |
| [Duke Nukem Zero Hour](https://github.com/Gillou68310/DukeNukemZeroHour)                                          | N64               |           |
| [Goldeneye 007](https://github.com/n64decomp/007)                                                                 | N64               |           |
| [Harvest Moon 64](https://github.com/harvestwhisperer/hm64-decomp)                                                | N64               |           |
| [Kirby 64: The Crystal Shards](https://github.com/farisawan-2000/kirby64)                                         | N64               |           |
| [Mario Kart 64](https://github.com/n64decomp/mk64)                                                                | N64               |           |
| [Mario Party 1](https://github.com/Rainchus/mp1)                                                                  | N64               |           |
| [Mario Party 3](https://github.com/zestydevy/marioparty3)                                                         | N64               |           |
| [Paper Mario 64](https://github.com/pmret/papermario)                                                             | N64               | ✅         |
| [Perfect Dark](https://github.com/n64decomp/perfect_dark)                                                         | N64               | ✅         |
| [Pokemon Stadium](https://github.com/pret/pokestadium)                                                            | N64               |           |
| [Quest 64](https://github.com/Mallos31/Quest)                                                                     | N64               |           |
| [Star Fox 64](https://github.com/sonicdcer/sf64)                                                                  | N64               |           |
| [Super Mario 64](https://github.com/n64decomp/sm64)                                                               | N64               | ✅         |
| [The Legend of Zelda: Majora's Mask](https://github.com/zeldaret/mm)                                              | N64               | ✅         |
| [The Legend of Zelda: Ocarina of Time](https://github.com/zeldaret/oot)                                           | N64               | ✅         |
| [Ty the Tasmanian Tiger](https://github.com/1superchip/ty-decomp)                                                 | N64               |           |
| [Wave Racer 64](https://github.com/LLONSIT/Wave-Race-64)                                                          | N64               |           |
| [Yoshi's Story](https://github.com/decompals/yoshis-story)                                                        | N64               |           |
| [Pokémon HeartGold and SoulSilver](https://github.com/pret/pokeheartgold)                                          | NDS               |           |
| [Rhythm Heaven](https://github.com/patataofcourse/rhgold)                                                         | NDS               |           |
| [Touhou Project 1](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       | ✅         |
| [Touhou Project 2](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       |           |
| [Touhou Project 3](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       |           |
| [Touhou Project 4](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       |           |
| [Touhou Project 5](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       |           |
| [Zelda II: The Adventure of Link](https://github.com/FiendsOfTheElements/z2disassembly)                           | NES               | ✅         |
| [Carmageddon](https://github.com/dethrace-labs/dethrace)                                                          | PC                |           |
| [Deus Ex: Human Revolution - Director's Cut](https://github.com/rrika/cdcEngineDXHR)                              | PC                |           |
| [Final Fantasy VII](https://github.com/ficed/Braver)                                                              | PC                |           |
| [Lego Island](https://github.com/isledecomp/isle)                                                                 | PC                |           |
| [Sonic Mania](https://github.com/Rubberduckycooly/Sonic-Mania-Decompilation)                                      | PC                |           |
| [Spider-Man (Neversoft)](https://github.com/krystalgamer/spidey-decomp)                                           | PC                |           |
| [Castlevania: Symphony of the Night](https://github.com/Xeeynamo/sotn-decomp)                                      | PS1               |           |
| [Crash Bandicoot](https://github.com/wurlyfox/c1)                                                                 | PS1               |           |
| [Crash Team Racing](https://github.com/CTR-Tools/CTR-ModSDK)                                                      | PS1               |           |
| [Doom](https://github.com/BodbDearg/PsyDoom)                                                                      | PS1               |           |
| [Driver 2](https://github.com/OpenDriver2/REDRIVER2)                                                              | PS1               | ✅         |
| [Legacy of Kain: Soul Reaver](https://github.com/Gh0stBlade/KAIN2)                                                | PS1               |           |
| [Legend of Dragoon](https://github.com/Legend-of-Dragoon-Modding/Legend-of-Dragoon-Java)                          | PS1               | ✅         |
| [Medievil 1](https://github.com/MediEvilDecompilation/medievil-decomp)                                            | PS1               |           |
| [Metal Gear Solid](https://github.com/FoxdieTeam/mgs_reversing)                                                   | PS1               |           |
| [R4: Ridge Racer Type 4](https://wakelet.com/wake/GeXGnrhyIWM-zbHJ4KURI)                                          | PS1               |           |
| [Tomb Raider 1](https://github.com/rr-/Tomb1Main)                                                                 | PS1               | ✅         |
| [Tomb Raider 1-5](https://github.com/XProger/OpenLara)                                                            | PS1               |           |
| [Vandal Hearts](https://github.com/shao113/vh)                                                                    | PS1               |           |
| [Fatal Frame 2 : Crimson Butterfly](https://github.com/wagrenier/Mikompilation)                                   | PS2               |           |
| [Jak II](https://github.com/open-goal/jak-project)                                                                | PS2               | ✅         |
| [Jak and Daxter: The Precursor Legacy](https://github.com/open-goal/jak-project)                                  | PS2               | ✅         |
| [Kingdom Hearts](https://github.com/ethteck/kh1)                                                                  | PS2               |           |
| [Earthbound / Mother 2](https://github.com/Herringway/ebsrc)                                                      | SNES              |           |
| [Final Fantasy VI](https://github.com/everything8215/ff6)                                                         | SNES              |           |
| [Super Mario World 2: Yoshi's Island](https://github.com/brunovalads/yoshisisland-disassembly)                    | SNES              | ✅         |
| [Super Metroid](https://github.com/snesrev/sm)                                                                    | SNES              | ✅         |
| [The Legend of Zelda: A Link to the Past](https://github.com/snesrev/zelda3)                                      | SNES              | ✅         |
| [Super Mario Oddysey](https://github.com/MonsterDruide1/OdysseyDecomp)                                            | Switch            |           |
| [The Legend of Zelda: Breath of the Wild](https://github.com/zeldaret/botw)                                       | Switch            |           |
| [Mario Kart Wii](https://github.com/riidefi/mkw)                                                                  | Wii               |           |
| [Super Mario Galaxy](https://github.com/shibbo/Petari)                                                            | Wii               |           |
| [Super Paper Mario](https://github.com/SeekyCt/spm-decomp)                                                        | Wii               |           |
| [Xenoblade Chronicles](https://github.com/CelestialAmber/xenoblade)                                               | Wii               |           |
| [Halo: Combat Evolved](https://github.com/halo-re/halo)                                                           | XBOX              |           |
| [Minecraft: Pocket Edition (2011)](https://github.com/ReMinecraftPE/mcpe)                                         | iOS               | ✅         |

[^1]: https://www.resetera.com/threads/decompilation-projects-ot-free-next-gen-update-for-your-favorite-classics-jak-ii-pc-port-out-in-beta.682687/