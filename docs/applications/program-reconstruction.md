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
| [Paper Mario: Sticker Star](https://github.com/darxoon/leaflitter)                                                | 3DS               |           |
| [Super Mario 3D Land](https://github.com/3dsdecomp/RedPepper)                                                     | 3DS               |           |
| [The Legend of Zelda: Ocarina of Time 3D](https://github.com/zeldaret/oot3d)                                      | 3DS               |           |
| [Ambermoon](https://github.com/Pyrdacor/Ambermoon.net)                                                            | Amiga             | ✅         |
| [The Settlers I](https://github.com/freeserf/freeserf)                                                            | DOS               | ✅         |
| [Tokyo Bus Guide](https://github.com/lhsazevedo/tbg-decomp)                                                       | Dreamcast         |           |
| [Donkey Kong '94](https://github.com/CelestialAmber/DKGBDisasm)                                                   | GB                | ✅         |
| [Kirby's Dream Land](https://github.com/huderlem/kirbydreamland)                                                  | GB                | ✅         |
| [Metroid II: Return of Samus](https://github.com/Vashy777/metroid2)                                               | GB                |           |
| [Pokémon Red and Blue](https://github.com/pret/pokered)                                                           | GB                | ✅         |
| [Pokémon Yellow](https://github.com/pret/pokeyellow)                                                              | GB                | ✅         |
| [Super Mario Land](https://github.com/kaspermeerts/supermarioland)                                                | GB                | ✅         |
| [Super Mario Land 3: Wario Land](https://github.com/Kak2X/wl)                                                     | GB                | ✅         |
| [Banjo-Kazooie: Grunty's Revenge](https://github.com/jellees/bkgr)                                                | GBA               |           |
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
| [The Legend of Zelda: Minish Cap](https://github.com/zeldaret/tmc)                                                | GBA               | ✅         |
| [Links Awakening DX](https://github.com/zladx/LADX-Disassembly)                                                   | GBC               |           |
| [Pokémon Crystal](https://github.com/pret/pokecrystal)                                                            | GBC               | ✅         |
| [Pokémon Gold and Silver](https://github.com/pret/pokegold)                                                       | GBC               | ✅         |
| [Pokémon Pinball](https://github.com/pret/pokepinball)                                                            | GBC               | ✅         |
| [The Legend of Zelda: Link's Awakening DX HD](https://github.com/zladx/LADX-Disassembly)                          | GBC               | ✅         |
| [Wario Land 3](https://github.com/froggestspirit/wland3)                                                          | GBC               | ✅         |
| [Animal Crossing](https://github.com/acreteam/ac-decomp)                                                          | GameCube          |           |
| [Chibi-Robo: PIA](https://github.com/eavpsp/cbr_decomp)                                                           | GameCube          |           |
| [Doshin the Giant](https://github.com/break-core/doshin-gc)                                                       | GameCube          |           |
| [Harvest Moon: A Wonderful Life](https://github.com/ChrisNonyminus/hmawl)                                         | GameCube          |           |
| [Homeland](https://github.com/bttrdrgn/homeland)                                                                  | GameCube          |           |
| [Kirby Air Ride](https://github.com/doldecomp/kar)                                                                | GameCube          |           |
| [Luigi's Mansion](https://github.com/Moddimation/Yasiki)                                                          | GameCube          |           |
| [Mario Kart: Double Dash!!](https://github.com/SwareJonge/mkdd)                                                   | GameCube          |           |
| [Mario Party 4](https://github.com/mariopartyrd/marioparty4)                                                      | GameCube          |           |
| [Mario Party 5](https://github.com/mariopartyrd/marioparty5)                                                      | GameCube          |           |
| [Mario Party 6](https://github.com/mariopartyrd/marioparty6)                                                      | GameCube          |           |
| [Mario Party 7](https://github.com/mariopartyrd/marioparty7)                                                      | GameCube          |           |
| [Mario Superstar Baseball](https://github.com/roeming/mssbdecomp)                                                 | GameCube          |           |
| [Metroid Prime 1](https://github.com/PrimeDecomp/prime)                                                           | GameCube          |           |
| [Metroid Prime 2](https://github.com/PrimeDecomp/echoes)                                                          | GameCube          |           |
| [Naruto: Gekitō Ninja Taisen! 4](https://github.com/doldecomp/gnt4)                                               | GameCube          |           |
| [Need for Speed: Most Wanted](https://github.com/dbalatoni13/nfsmw)                                               | GameCube          |           |
| [Need for Speed: Underground](https://github.com/dbalatoni13/nfsug)                                               | GameCube          |           |
| [Paper Mario: The Thousand-Year Door](https://github.com/doldecomp/ttyd)                                          | GameCube          |           |
| [Pikmin 1](https://github.com/projectPiki/pikmin)                                                                 | GameCube          |           |
| [Pikmin 2](https://github.com/projectPiki/pikmin2)                                                                | GameCube          |           |
| [Ratatouille](https://github.com/zounadecomp/ratdecomp)                                                           | GameCube          |           |
| [Rocket: Robot on Wheels](https://github.com/rocketret/rocket-robot-on-wheels)                                    | GameCube          |           |
| [Skies of Arcadia Legends](https://github.com/rainchus/skiesofarcadialegends)                                     | GameCube          |           |
| [Sonic Adventure DX](https://github.com/doldecomp/sadx)                                                           | GameCube          |           |
| [Sonic Riders](https://github.com/doldecomp/sonicriders)                                                          | GameCube          |           |
| [SpongeBob SquarePants: Battle for Bikini Bottom](https://github.com/bfbbdecomp/bfbb)                             | GameCube          |           |
| [Summoner: A Goddess Reborn](https://github.com/Charlese2/sgr)                                                    | GameCube          |           |
| [Super Mario Sunshine](https://github.com/doldecomp/sms)                                                          | GameCube          |           |
| [Super Smash Bros. Melee](https://github.com/doldecomp/melee)                                                     | GameCube          |           |
| [The Incredibles](https://github.com/seilweiss/incredibles)                                                       | GameCube          |           |
| [The Legend of Zelda: The Wind Waker](https://github.com/zeldaret/tww)                                            | GameCube          |           |
| [The Legend of Zelda: Twilight Princess](https://github.com/zeldaret/tp)                                          | GameCube          |           |
| [Ty the Tasmanian Tiger](https://github.com/1superchip/ty-decomp)                                                 | GameCube          |           |
| [Phantasy Star II](https://github.com/lory90/ps2disasm)                                                           | MegaDrive         |           |
| [Phantasy Star III](https://github.com/lory90/ps3disasm)                                                          | MegaDrive         |           |
| [Phantasy Star IV](https://github.com/lory90/ps4disasm)                                                           | MegaDrive         |           |
| [Ristar](https://github.com/sonicretro/ristar)                                                                    | MegaDrive         |           |
| [Streets of Rage 2](https://www.sor2newera.com/)                                                                  | MegaDrive         | ✅         |
| [AeroGauge](https://github.com/llonsit/aerogauge)                                                                 | N64               |           |
| [Aidyn Chronicles: The First Mage](https://github.com/blackgamma7/aidyn)                                          | N64               |           |
| [Animal Forest](https://github.com/zeldaret/af)                                                                   | N64               |           |
| [Banjo-Kazooie](https://gitlab.com/banjo.decomp/banjo-kazooie)                                                    | N64               | ✅         |
| [Banjo-Tooie](https://github.com/mr-wiseguy/banjo-tooie)                                                          | N64               |           |
| [Blast Corps](https://github.com/retroplastic/blastcorps)                                                         | N64               |           |
| [Body Harvest](https://github.com/deltaniumindustries/bodyharvestdecomp)                                          | N64               |           |
| [Bomberman 64](https://github.com/bomberhackers/bm64)                                                             | N64               |           |
| [Bomberman 64: The Second Attack!](https://github.com/bomberhackers/tsa)                                          | N64               |           |
| [Bomberman Hero](https://github.com/bomberhackers/bmhero)                                                         | N64               |           |
| [Castlevania 64](https://github.com/k64ret/cv64)                                                                  | N64               |           |
| [Conker's Bad Fur Day](https://github.com/mkst/conker)                                                            | N64               |           |
| [Chameleon Twist 1](https://github.com/chameleontwistret/ctv1.0-jp)                                               | N64               |           |
| [Chameleon Twist 2](https://github.com/chameleontwistret/ct2v1.0-jp)                                              | N64               |           |
| [Dark Rift](https://github.com/unnunu/darkrift)                                                                   | N64               |           |
| [Diddy Kong Racing](https://github.com/DavidSM64/Diddy-Kong-Racing)                                               | N64               |           |
| [Dinosaur Planet](https://github.com/zestydevy/dinosaur-planet)                                                   | N64               |           |
| [Donkey Kong 64](https:/gitlab.com/dk64_decomp/dk64)                                                              | N64               |           |
| [Doom 64](https://github.com/erick194/doom64-re)                                                                  | N64               | ✅         |
| [Doraemon: Nobita to Mittsu no Seireiseki](https://github.com/prakxo/doraemon1)                                   | N64               |           |
| [Dr. Mario 64](https://github.com/AngheloAlf/drmario64)                                                           | N64               |           |
| [Duke Nukem: Zero Hour](https://github.com/Gillou68310/DukeNukemZeroHour)                                         | N64               |           |
| [F-Zero X](https://github.com/inspectredc/fzerox)                                                                 | N64               |           |
| [F-Zero X Expansion Kit](https://github.com/inspectredc/fzerox-expansion-kit)                                     | N64               |           |
| [Gauntlet Legends](https://github.com/drahsid/gauntlet-legends)                                                   | N64               |           |
| [Gex 64: Enter the Gecko](https://github.com/matbourgon/gex64decomp)                                              | N64               |           |
| [Glover](https://github.com/rainchus/glover)                                                                      | N64               |           |
| [Goldeneye 007](https://gitlab.com/kholdfuzion/goldeneye_src)                                                     | N64               |           |
| [Harvest Moon 64](https://github.com/harvestwhisperer/hm64-decomp)                                                | N64               |           |
| [Kirby 64: The Crystal Shards](https://github.com/kirby64ret/kirby64)                                             | N64               |           |
| [Lego Racers](https://github.com/marijnvdwerf/lego-racers)                                                        | N64               |           |
| [Mario Golf](https://github.com/monde-lointain/mariogolf64)                                                       | N64               |           |
| [Mario Kart 64](https://github.com/n64decomp/mk64)                                                                | N64               | ✅        |              
| [Mario Party 1](https://github.com/mariopartyrd/marioparty)                                                       | N64               |           |
| [Mario Party 2](https://github.com/mariopartyrd/marioparty2)                                                      | N64               |           |
| [Mario Party 3](https://github.com/mariopartyrd/marioparty3)                                                      | N64               |           |
| [Mario Tennis](https://github.com/dellm-79/mariotennis64)                                                         | N64               |           |
| [Mischief Makers](https://github.com/drahsid/mischief-makers)                                                     | N64               |           |
| [Neon Genesis Evangelion 64](https://github.com/farisawan-2000/evangelion)                                        | N64               |           |
| [Paper Mario](https://github.com/pmret/papermario)                                                                | N64               | ✅         |
| [Perfect Dark](https://gitlab.com/ryandwyer/perfect-dark)                                                         | N64               | ✅         |
| [Pokemon Puzzle League](https://github.com/angheloalf/puzzleleague64)                                             | N64               |           |
| [Pokemon Snap](https://github.com/ethteck/pokemonsnap)                                                            | N64               |           |
| [Pokemon Stadium](https://github.com/pret/pokestadium)                                                            | N64               |           |
| [Pokemon Stadium 2](https://github.com/pret/pokestadiumgs)                                                        | N64               |           |
| [Quest 64](https://github.com/rainchus/quest64-decomp)                                                            | N64               |           |
| [Shadowgate 64](https://github.com/rainchus/shadowgate64)                                                         | N64               |           |
| [Space Station Silicon Valley](https://github.com/mkst/sssv)                                                      | N64               |           |
| [Star Fox 64](https://github.com/sonicdcer/sf64)                                                                  | N64               |           |
| [Star Wars: Shadows of the Empire](https://github.com/eltalelibrarian/sote)                                       | N64               |           |
| [Super Mario 64](https://github.com/n64decomp/sm64)                                                               | N64               | ✅         |
| [Super Smash Bros.](https://github.com/vetritheretri/ssb-decomp-re)                                               | N64               |           |
| [Superman 64](https://github.com/farisawan-2000/superman)                                                         | N64               |           |
| [The Legend of Zelda: Majora's Mask](https://github.com/zeldaret/mm)                                              | N64               | ✅         |
| [The Legend of Zelda: Ocarina of Time](https://github.com/zeldaret/oot)                                           | N64               | ✅         |
| [The New Tetris](https://github.com/kiritodv/tnt)                                                                 | N64               |           |
| [Virtual Pool 64](https://github.com/llonsit/virtualpool64)                                                       | N64               |           |
| [Virtual Pro Wrestling 2](https://github.com/aki-club/vpw2)                                                       | N64               |           |
| [Wave Race 64](https://github.com/LLONSIT/Wave-Race-64)                                                           | N64               |           |
| [Yoshi's Story](https://github.com/decompals/yoshis-story)                                                        | N64               |           |
| [Castlevania: Order of Ecclesia](https://github.com/lagolunatic/ooe)                                              | NDS               |           |
| [Mario & Luigi - Partners in Time](https://github.com/rainchus/partnersintime-decomp)                             | NDS               |           |
| [Mario Party DS](https://github.com/mariopartyrd/mariopartyds)                                                    | NDS               |           |
| [Pokémon Diamond and Pearl](https://github.com/pret/pokediamond)                                                  | NDS               |           |
| [Pokémon HeartGold and SoulSilver](https://github.com/pret/pokeheartgold)                                         | NDS               |           |
| [Rhythm Heaven](https://github.com/patataofcourse/rhgold)                                                         | NDS               |           |
| [The Legend of Zelda: Phantom Hourglass](https://github.com/zeldaret/ph)                                          | NDS               |           |
| [The Legend of Zelda: Spirit Tracks](https://github.com/yanis002/st)                                              | NDS               |           |
| [Touhou Project 1](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       | ✅         |
| [Touhou Project 2](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       |           |
| [Touhou Project 3](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       |           |
| [Touhou Project 4](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       |           |
| [Touhou Project 5](https://rec98.nmlgc.net/)                                                                      | NEC PC-9800       |           |
| [Zelda II: The Adventure of Link](https://github.com/FiendsOfTheElements/z2disassembly)                           | NES               | ✅         |
| [Carmageddon](https://github.com/dethrace-labs/dethrace)                                                          | PC                |           |
| [Deus Ex: Human Revolution - Director's Cut](https://github.com/rrika/cdcEngineDXHR)                              | PC                |           |
| [Final Fantasy VII](https://github.com/ficed/Braver)                                                              | PC                |           |
| [Lego Island](https://github.com/isledecomp/isle)                                                                 | PC                | ✅         |
| [Sonic Mania](https://github.com/Rubberduckycooly/Sonic-Mania-Decompilation)                                      | PC                |           |
| [Spider-Man (Neversoft)](https://github.com/krystalgamer/spidey-decomp)                                           | PC                |           |
| [Castlevania: Symphony of the Night](https://github.com/Xeeynamo/sotn-decomp)                                     | PS1               |           |
| [Crash Bandicoot](https://github.com/wurlyfox/c1)                                                                 | PS1               |           |
| [Crash Bandicoot 2: Cortex Strikes Back](https://github.com/ughman/c2c)                                           | PS1               |           |
| [Crash Team Racing](https://github.com/CTR-Tools/CTR-ModSDK)                                                      | PS1               |           |
| [Croc: Legend of the Gobbos](https://github.com/xeeynamo/croc)                                                    | PS1               |           |
| [Doom](https://github.com/BodbDearg/PsyDoom)                                                                      | PS1               |           |
| [Driver 2](https://github.com/OpenDriver2/REDRIVER2)                                                              | PS1               | ✅         |
| [Legacy of Kain: Soul Reaver](https://github.com/fmil95/soul-re)                                                  | PS1               |           |
| [Legend of Dragoon](https://github.com/Legend-of-Dragoon-Modding/Legend-of-Dragoon-Java)                          | PS1               | ✅         |
| [Medievil 1](https://github.com/MediEvilDecompilation/medievil-decomp)                                            | PS1               |           |
| [Metal Gear Solid](https://github.com/FoxdieTeam/mgs_reversing)                                                   | PS1               |           |
| [R4: Ridge Racer Type 4](https://wakelet.com/wake/GeXGnrhyIWM-zbHJ4KURI)                                          | PS1               |           |
| [Silent Hill](https://github.com/Vatuu/silent-hill-decomp)                                                        | PS1               |           |
| [Spyro the Dragon](https://github.com/TheMobyCollective/spyro-1)                                                  | PS1               |           |
| [Tomb Raider 1](https://github.com/rr-/Tomb1Main)                                                                 | PS1               | ✅         |
| [Tomb Raider 1-5](https://github.com/XProger/OpenLara)                                                            | PS1               |           |
| [Vandal Hearts](https://github.com/shao113/vh)                                                                    | PS1               |           |
| [Xenogears](https://github.com/ladysilverberg/xenogears-decomp)                                                   | PS1               |           |
| [Fatal Frame](https://github.com//Mikompilation/Himuro)                                                           | PS2               |           |
| [Fatal Frame 2: Crimson Butterfly](https://github.com/Mikompilation/Minakami)                                     | PS2               |           |
| [Jak II](https://github.com/open-goal/jak-project)                                                                | PS2               | ✅         |
| [Jak and Daxter: The Precursor Legacy](https://github.com/open-goal/jak-project)                                  | PS2               | ✅         |
| [Kingdom Hearts](https://github.com/ethteck/kh1)                                                                  | PS2               |           |
| [Klonoa 2: Lunatea's Veil](https://github.com/entriphy/kl2_lv_decomp)                                             | PS2               |           |
| [Metal Gear Solid 2: Sons of Liberty](https://github.com/GirianSeed/mgs2)                                         | PS2               |           |
| [PaRappa the Rapper 2](https://github.com/parappadev/parappa2)                                                    | PS2               |           |
| [Resident Evil - Code: Veronica X](https://github.com/fmil95/recvx-decomp)                                        | PS2               |           |
| [Sly Cooper and the Thievius Raccoonus](https://github.com/TheOnlyZac/sly1)                                       | PS2               |           |
| [Street Fighter III: 3rd Strike](https://github.com/apstygo/sfiii-decomp)                                         | PS2               |           |
| [Twisted Metal: Black](https://github.com/abelbriggs1/tmb_decomp)                                                 | PS2               |           |
| [Xenosaga Episode 1: Der Wille zur Macht](https://github.com/squareman/xenosaga)                                  | PS2               |           |
| [Demon's Crest](https://github.com/fredyeye/various-game-disassembly)                                             | SNES              |           |
| [Donkey Kong Country 1](https://github.com/Yoshifanatic1/Donkey-Kong-Country-1-Disassembly)                       | SNES              |           |
| [Donkey Kong Country 2](https://github.com/p4plus2/DKC2-disassembly)                                              | SNES              |           |
| [Donkey Kong Country 3](https://github.com/Yoshifanatic1/Donkey-Kong-Country-3-Disassembly)                       | SNES              |           |
| [Earthbound / Mother 2](https://github.com/Herringway/ebsrc)                                                      | SNES              |           |
| [Final Fantasy IV](https://github.com/everything8215/ff4)                                                         | SNES              |           |
| [Final Fantasy VI](https://github.com/everything8215/ff6)                                                         | SNES              |           |
| [Goof Troop](https://github.com/Yoshifanatic1/Goof-Troop-Disassembly)                                             | SNES              |           |
| [Super Ghouls n' Ghosts](https://github.com/fredyeye/super-ghouls-n-ghosts-disassembly)                           | SNES              |           |
| [Super Mario RPG](https://github.com/Yoshifanatic1/Super-Mario-RPG-Disassembly)                                   | SNES              |           |
| [Super Mario World](https://github.com/IsoFrieze/SMWDisX)                                                         | SNES              |           |
| [Super Mario World 2: Yoshi's Island](https://github.com/brunovalads/yoshisisland-disassembly)                    | SNES              | ✅         |
| [Super Metroid](https://github.com/snesrev/sm)                                                                    | SNES              | ✅         |
| [Super Punch-Out!!](https://github.com/Yoshifanatic1/Super-Punch-Out-Disassembly)                                 | SNES              |           |
| [The Legend of Zelda: A Link to the Past](https://github.com/snesrev/zelda3)                                      | SNES              | ✅         |
| [Minecraft: Nintendo Switch Edition](https://github.com/GRAnimated/MinecraftLCE)                                  | Switch            |           |
| [Super Mario 3D World + Bowser's Fury](https://github.com/3dwcommunity/3dcomp)                                    | Switch            |           |
| [Super Mario Odyssey](https://github.com/MonsterDruide1/OdysseyDecomp)                                            | Switch            |           |
| [The Legend of Zelda: Breath of the Wild](https://github.com/zeldaret/botw)                                       | Switch            |           |
| [Inazuma Eleven Strikers](https://github.com/SwareJonge/IEStrikers)                                               | Wii               |           |
| [Kirby's Epic Yarn](https://github.com/swiftshine/key)                                                            | Wii               |           |
| [Mario Kart Wii](https://github.com/riidefi/mkw)                                                                  | Wii               |           |
| [Mario Party 8](https://github.com/mariopartyrd/marioparty8)                                                      | Wii               |           |
| [Mario Party 9](https://github.com/mariopartyrd/marioparty9)                                                      | Wii               |           |
| [Pokemon Battle Revolution](https://github.com/pret/pokerevo)                                                     | Wii               |           |
| [Super Mario Galaxy](https://github.com/SMGCommunity/Petari)                                                      | Wii               |           |
| [Super Mario Galaxy 2](https://github.com/SMGCommunity/Garigari)                                                  | Wii               |           |
| [Super Paper Mario](https://github.com/SeekyCt/spm-decomp)                                                        | Wii               |           |
| [Super Smash Bros. Brawl](https://github.com/doldecomp/brawl)                                                     | Wii               |           |
| [The Legend of Zelda: Skyward Sword](https://github.com/zeldaret/ss)                                              | Wii               |           |
| [Xenoblade Chronicles](https://github.com/xbret/xenoblade)                                                        | Wii               |           |
| [New Super Mario Bros. U](https://github.com/aboood40091/red-pro2/tree/master)                                    | Wii U             |           |
| [Halo: Combat Evolved](https://github.com/halo-re/halo)                                                           | Xbox              |           |
| [Minecraft: Xbox 360 Edition](https://github.com/AleBello7276/Minecraft-Xbox-360-Decompilation)                   | Xbox 360          |           |
| [Minecraft: Pocket Edition (2011)](https://github.com/ReMinecraftPE/mcpe)                                         | iOS               | ✅         |

[^1]: https://www.resetera.com/threads/decompilation-projects-ot-free-next-gen-update-for-your-favorite-classics-jak-ii-pc-port-out-in-beta.682687/
