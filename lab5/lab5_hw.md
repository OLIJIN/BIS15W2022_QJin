---
title: "dplyr Superhero"
date: "2022-01-20"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
---

## Load the tidyverse

```r
library("tidyverse")
```

```
## Warning: package 'tidyverse' was built under R version 4.0.2
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
## ✓ readr   2.1.1     ✓ forcats 0.5.1
```

```
## Warning: package 'ggplot2' was built under R version 4.0.2
```

```
## Warning: package 'tibble' was built under R version 4.0.2
```

```
## Warning: package 'tidyr' was built under R version 4.0.2
```

```
## Warning: package 'readr' was built under R version 4.0.2
```

```
## Warning: package 'dplyr' was built under R version 4.0.2
```

```
## Warning: package 'forcats' was built under R version 4.0.2
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
```

```
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
```

```
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
superhero_info <- rename(superhero_info, Name='name')
names(superhero_info)
```

```
##  [1] "Name"       "Gender"     "Eye color"  "Race"       "Hair color"
##  [6] "Height"     "Publisher"  "Skin color" "Alignment"  "Weight"
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 × 168
##   hero_names  Agility `Accelerated Healing` `Lantern Power Ri… `Dimensional Awa…
##   <chr>       <lgl>   <lgl>                 <lgl>              <lgl>            
## 1 3-D Man     TRUE    FALSE                 FALSE              FALSE            
## 2 A-Bomb      FALSE   TRUE                  FALSE              FALSE            
## 3 Abe Sapien  TRUE    TRUE                  FALSE              FALSE            
## 4 Abin Sur    FALSE   FALSE                 TRUE               FALSE            
## 5 Abomination FALSE   TRUE                  FALSE              FALSE            
## 6 Abraxas     FALSE   FALSE                 FALSE              TRUE             
## # … with 163 more variables: Cold Resistance <lgl>, Durability <lgl>,
## #   Stealth <lgl>, Energy Absorption <lgl>, Flight <lgl>, Danger Sense <lgl>,
## #   Underwater breathing <lgl>, Marksmanship <lgl>, Weapons Master <lgl>,
## #   Power Augmentation <lgl>, Animal Attributes <lgl>, Longevity <lgl>,
## #   Intelligence <lgl>, Super Strength <lgl>, Cryokinesis <lgl>,
## #   Telepathy <lgl>, Energy Armor <lgl>, Energy Blasts <lgl>,
## #   Duplication <lgl>, Size Changing <lgl>, Density Control <lgl>, …
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
```

```
## Warning: package 'janitor' was built under R version 4.0.2
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
names(superhero_powers)
```

```
##   [1] "hero_names"                   "agility"                     
##   [3] "accelerated_healing"          "lantern_power_ring"          
##   [5] "dimensional_awareness"        "cold_resistance"             
##   [7] "durability"                   "stealth"                     
##   [9] "energy_absorption"            "flight"                      
##  [11] "danger_sense"                 "underwater_breathing"        
##  [13] "marksmanship"                 "weapons_master"              
##  [15] "power_augmentation"           "animal_attributes"           
##  [17] "longevity"                    "intelligence"                
##  [19] "super_strength"               "cryokinesis"                 
##  [21] "telepathy"                    "energy_armor"                
##  [23] "energy_blasts"                "duplication"                 
##  [25] "size_changing"                "density_control"             
##  [27] "stamina"                      "astral_travel"               
##  [29] "audio_control"                "dexterity"                   
##  [31] "omnitrix"                     "super_speed"                 
##  [33] "possession"                   "animal_oriented_powers"      
##  [35] "weapon_based_powers"          "electrokinesis"              
##  [37] "darkforce_manipulation"       "death_touch"                 
##  [39] "teleportation"                "enhanced_senses"             
##  [41] "telekinesis"                  "energy_beams"                
##  [43] "magic"                        "hyperkinesis"                
##  [45] "jump"                         "clairvoyance"                
##  [47] "dimensional_travel"           "power_sense"                 
##  [49] "shapeshifting"                "peak_human_condition"        
##  [51] "immortality"                  "camouflage"                  
##  [53] "element_control"              "phasing"                     
##  [55] "astral_projection"            "electrical_transport"        
##  [57] "fire_control"                 "projection"                  
##  [59] "summoning"                    "enhanced_memory"             
##  [61] "reflexes"                     "invulnerability"             
##  [63] "energy_constructs"            "force_fields"                
##  [65] "self_sustenance"              "anti_gravity"                
##  [67] "empathy"                      "power_nullifier"             
##  [69] "radiation_control"            "psionic_powers"              
##  [71] "elasticity"                   "substance_secretion"         
##  [73] "elemental_transmogrification" "technopath_cyberpath"        
##  [75] "photographic_reflexes"        "seismic_power"               
##  [77] "animation"                    "precognition"                
##  [79] "mind_control"                 "fire_resistance"             
##  [81] "power_absorption"             "enhanced_hearing"            
##  [83] "nova_force"                   "insanity"                    
##  [85] "hypnokinesis"                 "animal_control"              
##  [87] "natural_armor"                "intangibility"               
##  [89] "enhanced_sight"               "molecular_manipulation"      
##  [91] "heat_generation"              "adaptation"                  
##  [93] "gliding"                      "power_suit"                  
##  [95] "mind_blast"                   "probability_manipulation"    
##  [97] "gravity_control"              "regeneration"                
##  [99] "light_control"                "echolocation"                
## [101] "levitation"                   "toxin_and_disease_control"   
## [103] "banish"                       "energy_manipulation"         
## [105] "heat_resistance"              "natural_weapons"             
## [107] "time_travel"                  "enhanced_smell"              
## [109] "illusions"                    "thirstokinesis"              
## [111] "hair_manipulation"            "illumination"                
## [113] "omnipotent"                   "cloaking"                    
## [115] "changing_armor"               "power_cosmic"                
## [117] "biokinesis"                   "water_control"               
## [119] "radiation_immunity"           "vision_telescopic"           
## [121] "toxin_and_disease_resistance" "spatial_awareness"           
## [123] "energy_resistance"            "telepathy_resistance"        
## [125] "molecular_combustion"         "omnilingualism"              
## [127] "portal_creation"              "magnetism"                   
## [129] "mind_control_resistance"      "plant_control"               
## [131] "sonar"                        "sonic_scream"                
## [133] "time_manipulation"            "enhanced_touch"              
## [135] "magic_resistance"             "invisibility"                
## [137] "sub_mariner"                  "radiation_absorption"        
## [139] "intuitive_aptitude"           "vision_microscopic"          
## [141] "melting"                      "wind_control"                
## [143] "super_breath"                 "wallcrawling"                
## [145] "vision_night"                 "vision_infrared"             
## [147] "grim_reaping"                 "matter_absorption"           
## [149] "the_force"                    "resurrection"                
## [151] "terrakinesis"                 "vision_heat"                 
## [153] "vitakinesis"                  "radar_sense"                 
## [155] "qwardian_power_ring"          "weather_control"             
## [157] "vision_x_ray"                 "vision_thermal"              
## [159] "web_creation"                 "reality_warping"             
## [161] "odin_force"                   "symbiote_costume"            
## [163] "speed_force"                  "phoenix_force"               
## [165] "molecular_dissipation"        "vision_cryo"                 
## [167] "omnipresent"                  "omniscient"
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  


```r
tabyl(superhero_info, Alignment)
```

```
##  Alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
neutralhero <- filter(superhero_info,Alignment=='neutral')
neutralhero
```

```
## # A tibble: 24 × 10
##    Name    Gender `Eye color` Race    `Hair color` Height Publisher `Skin color`
##    <chr>   <chr>  <chr>       <chr>   <chr>         <dbl> <chr>     <chr>       
##  1 Bizarro Male   black       Bizarro Black           191 DC Comics white       
##  2 Black … Male   <NA>        God / … <NA>             NA DC Comics <NA>        
##  3 Captai… Male   brown       Human   Brown            NA DC Comics <NA>        
##  4 Copycat Female red         Mutant  White           183 Marvel C… blue        
##  5 Deadpo… Male   brown       Mutant  No Hair         188 Marvel C… <NA>        
##  6 Deaths… Male   blue        Human   White           193 DC Comics <NA>        
##  7 Etrigan Male   red         Demon   No Hair         193 DC Comics yellow      
##  8 Galact… Male   black       Cosmic… Black           876 Marvel C… <NA>        
##  9 Gladia… Male   blue        Stront… Blue            198 Marvel C… purple      
## 10 Indigo  Female <NA>        Alien   Purple           NA DC Comics <NA>        
## # … with 14 more rows, and 2 more variables: Alignment <chr>, Weight <dbl>
```

```r
select(neutralhero,'Name')
```

```
## # A tibble: 24 × 1
##    Name        
##    <chr>       
##  1 Bizarro     
##  2 Black Flash 
##  3 Captain Cold
##  4 Copycat     
##  5 Deadpool    
##  6 Deathstroke 
##  7 Etrigan     
##  8 Galactus    
##  9 Gladiator   
## 10 Indigo      
## # … with 14 more rows
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
select(superhero_info,'Name','Alignment','Race')
```

```
## # A tibble: 734 × 3
##    Name          Alignment Race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # … with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
filter(superhero_info,Race!='Human')
```

```
## # A tibble: 222 × 10
##    Name    Gender `Eye color` Race   `Hair color` Height Publisher  `Skin color`
##    <chr>   <chr>  <chr>       <chr>  <chr>         <dbl> <chr>      <chr>       
##  1 Abe Sa… Male   blue        Icthy… No Hair         191 Dark Hors… blue        
##  2 Abin S… Male   blue        Ungar… No Hair         185 DC Comics  red         
##  3 Abomin… Male   green       Human… No Hair         203 Marvel Co… <NA>        
##  4 Abraxas Male   blue        Cosmi… Black            NA Marvel Co… <NA>        
##  5 Ajax    Male   brown       Cyborg Black           193 Marvel Co… <NA>        
##  6 Alien   Male   <NA>        Xenom… No Hair         244 Dark Hors… black       
##  7 Amazo   Male   red         Andro… <NA>            257 DC Comics  <NA>        
##  8 Angel   Male   <NA>        Vampi… <NA>             NA Dark Hors… <NA>        
##  9 Angel … Female yellow      Mutant Black           165 Marvel Co… <NA>        
## 10 Anti-M… Male   yellow      God /… No Hair          61 DC Comics  <NA>        
## # … with 212 more rows, and 2 more variables: Alignment <chr>, Weight <dbl>
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- data.frame(filter(superhero_info,Alignment=='good'))
good_guys
```

```
##                          Name Gender               Eye.color              Race
## 1                      A-Bomb   Male                  yellow             Human
## 2                  Abe Sapien   Male                    blue     Icthyo Sapien
## 3                    Abin Sur   Male                    blue           Ungaran
## 4                 Adam Monroe   Male                    blue              <NA>
## 5                Adam Strange   Male                    blue             Human
## 6                    Agent 13 Female                    blue              <NA>
## 7                   Agent Bob   Male                   brown             Human
## 8                  Agent Zero   Male                    <NA>              <NA>
## 9                  Alan Scott   Male                    blue              <NA>
## 10               Alex Woolsly   Male                    <NA>              <NA>
## 11          Alfred Pennyworth   Male                    blue             Human
## 12           Allan Quatermain   Male                    <NA>              <NA>
## 13             Ando Masahashi   Male                    <NA>              <NA>
## 14                      Angel   Male                    blue              <NA>
## 15                      Angel   Male                    <NA>           Vampire
## 16                 Angel Dust Female                  yellow            Mutant
## 17            Angel Salvadore Female                   brown              <NA>
## 18                 Animal Man   Male                    blue             Human
## 19                    Ant-Man   Male                    blue             Human
## 20                 Ant-Man II   Male                    blue             Human
## 21                   Aquababy   Male                    blue              <NA>
## 22                    Aqualad   Male                    blue         Atlantean
## 23                    Aquaman   Male                    blue         Atlantean
## 24                    Arachne Female                    blue             Human
## 25                  Archangel   Male                    blue            Mutant
## 26                     Ardina Female                   white             Alien
## 27                       Ares   Male                   brown              <NA>
## 28                      Ariel Female                  purple              <NA>
## 29                      Armor Female                   black              <NA>
## 30                    Arsenal   Male                    <NA>             Human
## 31                  Astro Boy   Male                   brown              <NA>
## 32                      Atlas   Male                   brown            Mutant
## 33                       Atom   Male                    blue              <NA>
## 34                       Atom   Male                    <NA>              <NA>
## 35                  Atom Girl Female                   black              <NA>
## 36                    Atom II   Male                   brown             Human
## 37                   Atom III   Male                    <NA>              <NA>
## 38                    Atom IV   Male                   brown              <NA>
## 39                     Aurora Female                    blue            Mutant
## 40                     Azrael   Male                   brown             Human
## 41                      Aztar   Male                    <NA>              <NA>
## 42                    Banshee   Male                   green             Human
## 43                     Bantam   Male                   brown              <NA>
## 44                    Batgirl Female                    <NA>              <NA>
## 45                    Batgirl Female                   green             Human
## 46                Batgirl III Female                    <NA>              <NA>
## 47                 Batgirl IV Female                   green             Human
## 48                  Batgirl V Female                    <NA>              <NA>
## 49                 Batgirl VI Female                    blue              <NA>
## 50                     Batman   Male                    blue             Human
## 51                     Batman   Male                    blue             Human
## 52                  Batman II   Male                    blue             Human
## 53                 Battlestar   Male                   brown              <NA>
## 54                 Batwoman V Female                   green             Human
## 55                       Beak   Male                   black              <NA>
## 56                      Beast   Male                    blue            Mutant
## 57                  Beast Boy   Male                   green             Human
## 58                     Ben 10   Male                    <NA>              <NA>
## 59              Beta Ray Bill   Male                    <NA>              <NA>
## 60                   Beyonder   Male                    <NA>     God / Eternal
## 61                  Big Daddy   Male                    <NA>              <NA>
## 62                Bill Harken   Male                    <NA>             Alpha
## 63                     Binary Female                    blue              <NA>
## 64               Bionic Woman Female                    blue            Cyborg
## 65                 Bird-Brain   <NA>                    <NA>              <NA>
## 66                    Birdman   Male                    <NA>     God / Eternal
## 67                     Bishop   Male                   brown            Mutant
## 68                 Black Bolt   Male                    blue           Inhuman
## 69               Black Canary Female                    blue             Human
## 70               Black Canary Female                    blue         Metahuman
## 71                  Black Cat Female                   green             Human
## 72              Black Goliath   Male                    <NA>              <NA>
## 73           Black Knight III   Male                   brown             Human
## 74            Black Lightning   Male                   brown              <NA>
## 75              Black Panther   Male                   brown             Human
## 76                Black Widow Female                   green             Human
## 77             Black Widow II Female                    blue              <NA>
## 78                      Blade   Male                   brown           Vampire
## 79                Blaquesmith   <NA>                   black              <NA>
## 80                     Bling! Female                    <NA>              <NA>
## 81                      Blink Female                   green            Mutant
## 82                  Bloodhawk   Male                   black            Mutant
## 83                Blue Beetle   Male                    blue              <NA>
## 84                Blue Beetle   Male                    <NA>              <NA>
## 85             Blue Beetle II   Male                    blue              <NA>
## 86            Blue Beetle III   Male                   brown             Human
## 87                       Bolt   Male                    <NA>              <NA>
## 88                  Boom-Boom Female                    blue            Mutant
## 89                     Boomer Female                    <NA>              <NA>
## 90               Booster Gold   Male                    blue             Human
## 91                        Box   Male                    <NA>              <NA>
## 92                    Box III   <NA>                    blue              <NA>
## 93                     Box IV   <NA>                   brown              <NA>
## 94                 Brainiac 5   Male                   green              <NA>
## 95             Brother Voodoo   Male                   brown             Human
## 96                      Buffy Female                   green             Human
## 97                  Bumblebee Female                   brown             Human
## 98                  Bumbleboy   Male                    <NA>              <NA>
## 99                    Bushido   Male                    <NA>             Human
## 100                     Cable   Male                    blue            Mutant
## 101             Cameron Hicks   Male                    <NA>             Alpha
## 102                Cannonball   Male                    blue              <NA>
## 103           Captain America   Male                    blue             Human
## 104              Captain Atom   Male                    blue Human / Radiation
## 105           Captain Britain   Male                    blue             Human
## 106              Captain Epic   Male                    blue              <NA>
## 107         Captain Hindsight   Male                    <NA>             Human
## 108          Captain Mar-vell   Male                    blue              <NA>
## 109            Captain Marvel Female                    blue        Human-Kree
## 110            Captain Marvel   Male                    blue             Human
## 111         Captain Marvel II   Male                    blue             Human
## 112          Captain Midnight   Male                    <NA>             Human
## 113            Captain Planet   Male                     red     God / Eternal
## 114          Captain Universe   <NA>                    <NA>     God / Eternal
## 115                       Cat Female                    blue              <NA>
## 116                    Cat II Female                    <NA>              <NA>
## 117                  Catwoman Female                   green             Human
## 118             Cecilia Reyes   <NA>                   brown              <NA>
## 119                   Century   Male                   white             Alien
## 120                   Cerebra Female                    <NA>            Mutant
## 121                   Chamber   Male                   brown            Mutant
## 122              Chuck Norris   Male                    <NA>              <NA>
## 123             Citizen Steel   Male                   green             Human
## 124             Claire Bennet Female                    blue              <NA>
## 125                      Clea   <NA>                    <NA>              <NA>
## 126                     Cloak   Male                   brown              <NA>
## 127              Colin Wagner   Male                    grey              <NA>
## 128              Colossal Boy   Male                    <NA>              <NA>
## 129                  Colossus   Male                  silver            Mutant
## 130                   Corsair   Male                   brown              <NA>
## 131          Crimson Crusader   Male                    blue              <NA>
## 132            Crimson Dynamo   Male                   brown              <NA>
## 133                   Crystal Female                   green           Inhuman
## 134                    Cyborg   Male                   brown            Cyborg
## 135                   Cyclops   Male                   brown            Mutant
## 136                    Cypher   <NA>                    blue              <NA>
## 137                    Dagger Female                    blue              <NA>
## 138              Danny Cooper   Male                   brown              <NA>
## 139             Daphne Powell Female                    <NA>              <NA>
## 140                 Daredevil   Male                    blue             Human
## 141                  Darkhawk   Male                   brown             Human
## 142                   Darkman   Male                    <NA>              <NA>
## 143                  Darkstar Female                   brown            Mutant
## 144                      Dash   Male                    blue             Human
## 145                      Data   Male                  yellow           Android
## 146                   Dazzler Female                    blue            Mutant
## 147                   Deadman   Male                    blue             Human
## 148                  Deathlok   Male                   brown            Cyborg
## 149                DL Hawkins   Male                    <NA>              <NA>
## 150                Doc Samson   Male                    blue Human / Radiation
## 151               Doctor Fate   Male                    blue             Human
## 152            Doctor Strange   Male                    grey             Human
## 153                    Domino Female                    blue             Human
## 154                 Donatello   Male                   green            Mutant
## 155                Donna Troy Female                    blue            Amazon
## 156              Dr Manhattan   Male                   white    Human / Cosmic
## 157        Drax the Destroyer   Male                     red   Human / Altered
## 158                Elastigirl Female                   brown             Human
## 159                   Elektra Female                    blue             Human
## 160             Elongated Man   Male                    blue              <NA>
## 161                Emma Frost Female                    blue              <NA>
## 162               Enchantress Female                    blue             Human
## 163                    Energy Female                    <NA>              <NA>
## 164                     ERG-1   Male                    <NA>              <NA>
## 165                Ethan Hunt   Male                   brown             Human
## 166                    Falcon   Male                   brown             Human
## 167                     Feral   <NA> yellow (without irises)              <NA>
## 168           Fighting Spirit Female                    <NA>              <NA>
## 169             Fin Fang Foom   Male                     red   Kakarantharaian
## 170                  Firebird Female                   brown              <NA>
## 171                  Firelord   <NA>                   white              <NA>
## 172                  Firestar Female                   green            Mutant
## 173                 Firestorm   Male                   brown              <NA>
## 174                 Firestorm   Male                    blue             Human
## 175                     Flash   Male                    blue             Human
## 176              Flash Gordon   Male                    <NA>              <NA>
## 177                  Flash II   Male                    blue             Human
## 178                 Flash III   Male                    <NA>             Human
## 179                  Flash IV   Male                  yellow             Human
## 180                     Forge   <NA>                   brown              <NA>
## 181         Franklin Richards   Male                    blue            Mutant
## 182            Franklin Storm   <NA>                    blue              <NA>
## 183                    Frigga Female                    blue              <NA>
## 184                    Gambit   Male                     red            Mutant
## 185                    Gamora Female                  yellow     Zen-Whoberian
## 186               Garbage Man   Male                    <NA>            Mutant
## 187                 Gary Bell   Male                    <NA>             Alpha
## 188                   Genesis   Male                    blue              <NA>
## 189               Ghost Rider   Male                     red             Demon
## 190            Ghost Rider II   <NA>                    <NA>              <NA>
## 191                 Giant-Man   Male                    <NA>             Human
## 192              Giant-Man II   Male                    <NA>              <NA>
## 193                      Goku   Male                    <NA>            Saiyan
## 194                   Goliath   Male                    <NA>              <NA>
## 195                   Goliath   Male                    <NA>             Human
## 196                   Goliath   Male                    <NA>             Human
## 197                Goliath IV   Male                   brown              <NA>
## 198                   Gravity   Male                    blue             Human
## 199               Green Arrow   Male                   green             Human
## 200          Green Goblin III   Male                    <NA>              <NA>
## 201           Green Goblin IV   Male                   green              <NA>
## 202                     Groot   Male                  yellow    Flora Colossus
## 203                  Guardian   Male                   brown             Human
## 204               Guy Gardner   Male                    blue   Human-Vuldarian
## 205                Hal Jordan   Male                   brown             Human
## 206                  Han Solo   Male                   brown             Human
## 207                   Hancock   Male                   brown             Human
## 208              Harry Potter   Male                   green             Human
## 209                     Havok   Male                    blue            Mutant
## 210                      Hawk   Male                     red              <NA>
## 211                   Hawkeye   Male                    blue             Human
## 212                Hawkeye II Female                    blue             Human
## 213                  Hawkgirl Female                   green              <NA>
## 214                   Hawkman   Male                    blue              <NA>
## 215                 Hawkwoman Female                   green              <NA>
## 216              Hawkwoman II Female                    <NA>              <NA>
## 217             Hawkwoman III Female                    blue              <NA>
## 218                   Hellboy   Male                    gold             Demon
## 219                   Hellcat Female                    blue             Human
## 220                 Hellstorm   Male                     red              <NA>
## 221                  Hercules   Male                    blue          Demi-God
## 222             Hiro Nakamura   Male                    <NA>              <NA>
## 223                  Hit-Girl Female                    <NA>             Human
## 224                    Hollow Female                    blue              <NA>
## 225              Hope Summers Female                   green              <NA>
## 226           Howard the Duck   Male                   brown              <NA>
## 227                      Hulk   Male                   green Human / Radiation
## 228               Human Torch   Male                    blue Human / Radiation
## 229                  Huntress Female                    blue              <NA>
## 230                      Husk Female                    blue            Mutant
## 231                    Hybrid   Male                   brown          Symbiote
## 232                  Hyperion   Male                    blue           Eternal
## 233                    Iceman   Male                   brown            Mutant
## 234                   Impulse   Male                  yellow             Human
## 235             Indiana Jones   Male                    <NA>             Human
## 236                       Ink   Male                    blue            Mutant
## 237           Invisible Woman Female                    blue Human / Radiation
## 238                 Iron Fist   Male                    blue             Human
## 239                  Iron Man   Male                    blue             Human
## 240                      Isis Female                    <NA>              <NA>
## 241                Jack Bauer   Male                    <NA>              <NA>
## 242            Jack of Hearts   Male            blue / white             Human
## 243                 Jack-Jack   Male                    blue             Human
## 244                James Bond   Male                    blue             Human
## 245             James T. Kirk   Male                   hazel             Human
## 246             Jar Jar Binks   Male                  yellow            Gungan
## 247              Jason Bourne   Male                    <NA>             Human
## 248                 Jean Grey Female                   green            Mutant
## 249           Jean-Luc Picard   Male                    <NA>             Human
## 250             Jennifer Kale Female                    blue              <NA>
## 251               Jesse Quick Female                    <NA>             Human
## 252              Jessica Cruz Female                   green             Human
## 253             Jessica Jones Female                   brown             Human
## 254           Jessica Sanders Female                    <NA>              <NA>
## 255                Jim Powell   Male                    <NA>              <NA>
## 256                 JJ Powell   Male                    <NA>              <NA>
## 257             Johann Krauss   Male                    <NA>              <NA>
## 258          John Constantine   Male                    blue             Human
## 259              John Stewart   Male                   green             Human
## 260               John Wraith   Male                   brown              <NA>
## 261                      Jolt Female                    blue              <NA>
## 262                   Jubilee Female                     red            Mutant
## 263               Judge Dredd   Male                    <NA>             Human
## 264                   Justice   Male                   hazel             Human
## 265                  Jyn Erso Female                   green             Human
## 266                     K-2SO   Male                   white           Android
## 267                Karate Kid   Male                   brown             Human
## 268           Kathryn Janeway Female                    <NA>             Human
## 269          Katniss Everdeen Female                    <NA>             Human
## 270                  Kevin 11   Male                    <NA>             Human
## 271                  Kick-Ass   Male                    blue             Human
## 272                 Kid Flash   Male                   green             Human
## 273              Kid Flash II   Male                    <NA>              <NA>
## 274                   Kilowog   Male                     red        Bolovaxian
## 275                 King Kong   Male                  yellow            Animal
## 276              Kool-Aid Man   Male                   black              <NA>
## 277                    Krypto   Male                    blue        Kryptonian
## 278               Kyle Rayner   Male                   green             Human
## 279                     Leech   Male                    <NA>              <NA>
## 280                    Legion   Male            green / blue            Mutant
## 281                  Leonardo   Male                    blue            Mutant
## 282                Light Lass Female                    blue              <NA>
## 283             Lightning Lad   Male                    blue              <NA>
## 284               Liz Sherman Female                    <NA>              <NA>
## 285                  Longshot   Male                    blue             Human
## 286                 Luke Cage   Male                   brown             Human
## 287            Luke Skywalker   Male                    blue             Human
## 288                      Luna Female                    <NA>             Human
## 289                      Lyja Female                   green              <NA>
## 290               Machine Man   <NA>                     red              <NA>
## 291                     Magog   Male                    blue              <NA>
## 292                 Man-Thing   Male                     red              <NA>
## 293                  Man-Wolf   Male                   brown              <NA>
## 294                    Mantis Female                   green        Human-Kree
## 295         Martian Manhunter   Male                     red           Martian
## 296               Marvel Girl Female                   green              <NA>
## 297              Master Brood   Male                    blue              <NA>
## 298              Master Chief   Male                   brown   Human / Altered
## 299              Matt Parkman   Male                    <NA>              <NA>
## 300                  Maverick   Male                    blue              <NA>
## 301              Maya Herrera Female                    <NA>              <NA>
## 302                    Medusa Female                   green           Inhuman
## 303                  Meltdown Female                    blue              <NA>
## 304                      Mera Female                    blue         Atlantean
## 305                Metamorpho   Male                   black              <NA>
## 306                 Meteorite Female                    <NA>              <NA>
## 307                    Metron   Male                    blue              <NA>
## 308             Micah Sanders   Male                   brown              <NA>
## 309              Michelangelo   Male                    blue            Mutant
## 310                 Micro Lad   Male                    grey              <NA>
## 311                     Mimic   Male                   brown              <NA>
## 312              Minna Murray Female                    <NA>              <NA>
## 313                    Misfit Female                    blue              <NA>
## 314              Miss Martian Female                     red              <NA>
## 315          Mister Fantastic   Male                   brown Human / Radiation
## 316               Mockingbird Female                    blue             Human
## 317                      Mogo   Male                    <NA>            Planet
## 318           Mohinder Suresh   Male                    <NA>              <NA>
## 319                   Monarch   Male                    blue              <NA>
## 320             Monica Dawson Female                    <NA>              <NA>
## 321               Moon Knight   Male                   brown             Human
## 322                     Morph   Male                   white              <NA>
## 323               Mr Immortal   Male                    blue            Mutant
## 324             Mr Incredible   Male                    blue             Human
## 325              Ms Marvel II Female                    blue              <NA>
## 326              Multiple Man   Male                    blue              <NA>
## 327                     Namor   Male                    <NA>              <NA>
## 328                     Namor   Male                    grey         Atlantean
## 329                    Namora Female                    blue              <NA>
## 330                  Namorita Female                    blue              <NA>
## 331            Naruto Uzumaki   Male                    <NA>             Human
## 332           Nathan Petrelli   Male                   brown              <NA>
## 333 Negasonic Teenage Warhead Female                   black            Mutant
## 334                 Nick Fury   Male                   brown             Human
## 335              Nightcrawler   Male                  yellow              <NA>
## 336                 Nightwing   Male                    blue             Human
## 337              Niki Sanders Female                    blue              <NA>
## 338              Nina Theroux Female                    <NA>             Alpha
## 339               Nite Owl II   Male                    <NA>              <NA>
## 340                 Northstar   Male                    blue              <NA>
## 341                      Nova   Male                   brown             Human
## 342                      Nova Female                   white    Human / Cosmic
## 343                      Odin   Male                    blue     God / Eternal
## 344                 Offspring   Male                    <NA>              <NA>
## 345                Omniscient   Male                   brown              <NA>
## 346             One Punch Man   Male                    <NA>             Human
## 347                    Oracle Female                    blue             Human
## 348                    Osiris   Male                   brown              <NA>
## 349                Paul Blart   Male                    <NA>             Human
## 350                   Penance   <NA>                    <NA>              <NA>
## 351                 Penance I Female                    <NA>              <NA>
## 352                Penance II   Male                    blue              <NA>
## 353            Peter Petrelli   Male                    <NA>              <NA>
## 354                   Phantom   Male                    <NA>              <NA>
## 355              Phantom Girl Female                    blue              <NA>
## 356                   Phoenix Female                   green            Mutant
## 357               Plastic Lad   Male                    <NA>              <NA>
## 358               Plastic Man   Male                    blue             Human
## 359                   Polaris Female                   green            Mutant
## 360                Power Girl Female                    blue        Kryptonian
## 361                 Power Man   Male                    <NA>            Mutant
## 362               Professor X   Male                    blue            Mutant
## 363                  Psylocke Female                    blue            Mutant
## 364                  Punisher   Male                    blue             Human
## 365                   Quantum   Male                    <NA>              <NA>
## 366                  Question   Male                    blue             Human
## 367               Quicksilver   Male                    blue            Mutant
## 368                     Quill   Male                   brown              <NA>
## 369             Rachel Pirzad Female                    <NA>             Alpha
## 370                     Rambo   Male                   brown             Human
## 371                   Raphael   Male                    <NA>            Mutant
## 372                       Ray   Male                   green             Human
## 373                 Red Arrow   Male                   green             Human
## 374                 Red Robin   Male                    blue             Human
## 375               Red Tornado   Male                   green           Android
## 376              Renata Soliz Female                    <NA>              <NA>
## 377                       Rey Female                   hazel             Human
## 378                Rip Hunter   Male                    blue             Human
## 379                   Ripcord Female                   green              <NA>
## 380                     Robin   Male                    blue             Human
## 381                  Robin II   Male                    blue             Human
## 382                 Robin III   Male                    blue             Human
## 383                   Robin V   Male                    blue             Human
## 384            Rocket Raccoon   Male                   brown            Animal
## 385                     Rogue Female                   green              <NA>
## 386                     Ronin   Male                    blue             Human
## 387                 Rorschach   Male                    blue             Human
## 388                      Sage Female                    blue              <NA>
## 389                 Sasquatch   Male                     red              <NA>
## 390             Savage Dragon   Male                    <NA>              <NA>
## 391            Scarlet Spider   Male                    blue             Human
## 392         Scarlet Spider II   Male                   brown             Clone
## 393               Shadow King   <NA>                     red              <NA>
## 394               Shadow Lass Female                   black          Talokite
## 395                 Shadowcat Female                   hazel            Mutant
## 396                 Shang-Chi   Male                   brown             Human
## 397               Shatterstar   Male                   brown              <NA>
## 398                  She-Hulk Female                   green             Human
## 399                 She-Thing Female                    blue Human / Radiation
## 400                    Shriek Female           yellow / blue              <NA>
## 401          Shrinking Violet Female                    <NA>              <NA>
## 402                       Sif Female                    blue         Asgardian
## 403                      Silk Female                   brown             Human
## 404              Silk Spectre Female                    <NA>              <NA>
## 405           Silk Spectre II Female                    <NA>              <NA>
## 406             Silver Surfer   Male                   white             Alien
## 407                Silverclaw Female                   brown              <NA>
## 408                 Simon Baz   Male                    bown             Human
## 409                     Skaar   Male                   green              <NA>
## 410                  Snowbird Female                   white              <NA>
## 411                     Sobek   Male                   white              <NA>
## 412                  Songbird Female                   green              <NA>
## 413               Space Ghost   Male                    <NA>             Human
## 414                     Spawn   Male                   brown             Demon
## 415                   Spectre   Male                   white     God / Eternal
## 416                 Speedball   Male                    <NA>              <NA>
## 417                    Speedy   Male                    <NA>             Human
## 418                    Speedy Female                   green             Human
## 419               Spider-Girl Female                    blue             Human
## 420               Spider-Gwen Female                    blue             Human
## 421                Spider-Man   Male                   hazel             Human
## 422                Spider-Man   <NA>                     red             Human
## 423                Spider-Man   Male                   brown             Human
## 424              Spider-Woman Female                   green             Human
## 425           Spider-Woman II Female                    <NA>              <NA>
## 426          Spider-Woman III Female                   brown              <NA>
## 427                     Spock   Male                   brown      Human-Vulcan
## 428                     Spyke   Male                   brown            Mutant
## 429                   Stacy X Female                    <NA>              <NA>
## 430                 Star-Lord   Male                    blue     Human-Spartoi
## 431                  Stardust   Male                    <NA>              <NA>
## 432                  Starfire Female                   green        Tamaranean
## 433                  Stargirl Female                    blue             Human
## 434                    Static   Male                   brown            Mutant
## 435                     Steel   Male                   brown              <NA>
## 436          Stephanie Powell Female                    <NA>              <NA>
## 437                     Storm Female                    blue            Mutant
## 438                   Sunspot   Male                   brown            Mutant
## 439                  Superboy   Male                    blue              <NA>
## 440                 Supergirl Female                    blue        Kryptonian
## 441                  Superman   Male                    blue        Kryptonian
## 442                     Synch   Male                   brown              <NA>
## 443                   Tempest Female                   brown              <NA>
## 444                  The Cape   Male                    <NA>              <NA>
## 445                     Thing   Male                    blue Human / Radiation
## 446                      Thor   Male                    blue         Asgardian
## 447                 Thor Girl Female                    blue         Asgardian
## 448               Thunderbird   Male                   brown              <NA>
## 449            Thunderbird II   Male                    <NA>              <NA>
## 450           Thunderbird III   Male                   brown              <NA>
## 451             Thunderstrike   Male                    blue              <NA>
## 452                   Thundra Female                   green              <NA>
## 453                     Tigra Female                   green              <NA>
## 454                     Titan   Male                    <NA>              <NA>
## 455                     Toxin   Male                    blue          Symbiote
## 456                     Toxin   Male                   black          Symbiote
## 457             Tracy Strauss Female                    <NA>              <NA>
## 458           Triplicate Girl Female                  purple              <NA>
## 459                    Triton   Male                   green           Inhuman
## 460                 Ultragirl Female                    blue              <NA>
## 461                  Vagabond Female                    blue              <NA>
## 462              Valerie Hart Female                   hazel              <NA>
## 463                  Valkyrie Female                    blue              <NA>
## 464                Vertigo II Female                    blue              <NA>
## 465                      Vibe   Male                   brown             Human
## 466                Vindicator Female                   green             Human
## 467                Vindicator   Male                    <NA>              <NA>
## 468               Violet Parr Female                  violet             Human
## 469                    Vision   Male                    gold           Android
## 470                 Vision II   <NA>                     red              <NA>
## 471                     Vixen Female                   amber             Human
## 472                    Vulcan   Male                   black              <NA>
## 473               War Machine   Male                   brown             Human
## 474                   Warbird Female                    blue              <NA>
## 475                   Warlock   Male                     red              <NA>
## 476                   Warpath   Male                   brown            Mutant
## 477                      Wasp Female                    blue             Human
## 478                   Watcher   Male                    <NA>              <NA>
## 479               White Queen Female                    blue              <NA>
## 480                  Wildfire   Male                    <NA>              <NA>
## 481            Winter Soldier   Male                   brown             Human
## 482                   Wiz Kid   <NA>                   brown              <NA>
## 483                 Wolfsbane Female                   green              <NA>
## 484                 Wolverine   Male                    blue            Mutant
## 485               Wonder Girl Female                    blue          Demi-God
## 486                Wonder Man   Male                     red              <NA>
## 487              Wonder Woman Female                    blue            Amazon
## 488                    Wondra Female                    <NA>              <NA>
## 489            Wyatt Wingfoot   Male                   brown              <NA>
## 490                      X-23 Female                   green    Mutant / Clone
## 491                     X-Man   Male                    blue              <NA>
## 492              Yellowjacket   Male                    blue             Human
## 493           Yellowjacket II Female                    blue             Human
## 494                      Ymir   Male                   white       Frost Giant
## 495                      Yoda   Male                   brown    Yoda's species
## 496                   Zatanna Female                    blue             Human
##           Hair.color Height         Publisher     Skin.color Alignment Weight
## 1            No Hair  203.0     Marvel Comics           <NA>      good    441
## 2            No Hair  191.0 Dark Horse Comics           blue      good     65
## 3            No Hair  185.0         DC Comics            red      good     90
## 4              Blond     NA      NBC - Heroes           <NA>      good     NA
## 5              Blond  185.0         DC Comics           <NA>      good     88
## 6              Blond  173.0     Marvel Comics           <NA>      good     61
## 7              Brown  178.0     Marvel Comics           <NA>      good     81
## 8               <NA>  191.0     Marvel Comics           <NA>      good    104
## 9              Blond  180.0         DC Comics           <NA>      good     90
## 10              <NA>     NA      NBC - Heroes           <NA>      good     NA
## 11             Black  178.0         DC Comics           <NA>      good     72
## 12              <NA>     NA         Wildstorm           <NA>      good     NA
## 13              <NA>     NA      NBC - Heroes           <NA>      good     NA
## 14             Blond  183.0     Marvel Comics           <NA>      good     68
## 15              <NA>     NA Dark Horse Comics           <NA>      good     NA
## 16             Black  165.0     Marvel Comics           <NA>      good     57
## 17             Black  163.0     Marvel Comics           <NA>      good     54
## 18             Blond  183.0         DC Comics           <NA>      good     83
## 19             Blond  211.0     Marvel Comics           <NA>      good    122
## 20             Blond  183.0     Marvel Comics           <NA>      good     86
## 21             Blond     NA         DC Comics           <NA>      good     NA
## 22             Black  178.0         DC Comics           <NA>      good    106
## 23             Blond  185.0         DC Comics           <NA>      good    146
## 24             Blond  175.0     Marvel Comics           <NA>      good     63
## 25             Blond  183.0     Marvel Comics           blue      good     68
## 26            Orange  193.0     Marvel Comics           gold      good     98
## 27             Brown  185.0     Marvel Comics           <NA>      good    270
## 28              Pink  165.0     Marvel Comics           <NA>      good     59
## 29             Black  163.0     Marvel Comics           <NA>      good     50
## 30              <NA>     NA         DC Comics           <NA>      good     NA
## 31             Black     NA              <NA>           <NA>      good     NA
## 32               Red  183.0     Marvel Comics           <NA>      good    101
## 33               Red  178.0         DC Comics           <NA>      good     68
## 34              <NA>     NA         DC Comics           <NA>      good     NA
## 35             Black  168.0         DC Comics           <NA>      good     54
## 36            Auburn  183.0         DC Comics           <NA>      good     81
## 37               Red     NA         DC Comics           <NA>      good     NA
## 38             Black     NA         DC Comics           <NA>      good     72
## 39             Black  180.0     Marvel Comics           <NA>      good     63
## 40             Black     NA         DC Comics           <NA>      good     NA
## 41              <NA>     NA         DC Comics           <NA>      good     NA
## 42  Strawberry Blond  183.0     Marvel Comics           <NA>      good     77
## 43             Black  165.0     Marvel Comics           <NA>      good     54
## 44              <NA>     NA         DC Comics           <NA>      good     NA
## 45               Red  170.0         DC Comics           <NA>      good     57
## 46              <NA>     NA         DC Comics           <NA>      good     NA
## 47             Black  165.0         DC Comics           <NA>      good     52
## 48              <NA>     NA         DC Comics           <NA>      good     NA
## 49             Blond  168.0         DC Comics           <NA>      good     61
## 50             black  188.0         DC Comics           <NA>      good     95
## 51             Black  178.0         DC Comics           <NA>      good     77
## 52             Black  178.0         DC Comics           <NA>      good     79
## 53             Black  198.0     Marvel Comics           <NA>      good    133
## 54               Red  178.0         DC Comics           <NA>      good     NA
## 55             White  175.0     Marvel Comics           <NA>      good     63
## 56              Blue  180.0     Marvel Comics           blue      good    181
## 57             Green  173.0         DC Comics          green      good     68
## 58              <NA>     NA         DC Comics           <NA>      good     NA
## 59           No Hair  201.0     Marvel Comics           <NA>      good    216
## 60              <NA>     NA     Marvel Comics           <NA>      good     NA
## 61              <NA>     NA       Icon Comics           <NA>      good     NA
## 62              <NA>     NA              SyFy           <NA>      good     NA
## 63             Blond  180.0     Marvel Comics           <NA>      good     54
## 64             Black     NA              <NA>           <NA>      good     NA
## 65              <NA>     NA     Marvel Comics           <NA>      good     NA
## 66              <NA>     NA     Hanna-Barbera           <NA>      good     NA
## 67           No Hair  198.0     Marvel Comics           <NA>      good    124
## 68             Black  188.0     Marvel Comics           <NA>      good     95
## 69             Blond  165.0         DC Comics           <NA>      good     58
## 70             Blond  170.0         DC Comics           <NA>      good     59
## 71             Blond  178.0     Marvel Comics           <NA>      good     54
## 72              <NA>     NA     Marvel Comics           <NA>      good     NA
## 73             Brown  183.0     Marvel Comics           <NA>      good     86
## 74           No Hair  185.0         DC Comics           <NA>      good     90
## 75             Black  183.0     Marvel Comics           <NA>      good     90
## 76            Auburn  170.0     Marvel Comics           <NA>      good     59
## 77             Blond  170.0     Marvel Comics           <NA>      good     61
## 78             Black  188.0     Marvel Comics           <NA>      good     97
## 79           No Hair     NA     Marvel Comics           <NA>      good     NA
## 80              <NA>  168.0     Marvel Comics           <NA>      good     68
## 81           Magenta  165.0     Marvel Comics           pink      good     56
## 82           No Hair     NA     Marvel Comics           <NA>      good     NA
## 83             Brown     NA         DC Comics           <NA>      good     NA
## 84              <NA>     NA         DC Comics           <NA>      good     NA
## 85             Brown  183.0         DC Comics           <NA>      good     86
## 86             Black     NA         DC Comics           <NA>      good     NA
## 87              <NA>     NA     Marvel Comics           <NA>      good     NA
## 88             Blond  165.0     Marvel Comics           <NA>      good     55
## 89              <NA>     NA     Marvel Comics           <NA>      good     NA
## 90             Blond  196.0         DC Comics           <NA>      good     97
## 91              <NA>     NA     Marvel Comics           <NA>      good     NA
## 92             Blond  193.0     Marvel Comics           <NA>      good    110
## 93     Brown / Black     NA     Marvel Comics           <NA>      good     NA
## 94             Blond  170.0         DC Comics           <NA>      good     61
## 95     Brown / White  183.0     Marvel Comics           <NA>      good     99
## 96             Blond  157.0 Dark Horse Comics           <NA>      good     52
## 97             Black  170.0         DC Comics           <NA>      good     59
## 98              <NA>     NA     Marvel Comics           <NA>      good     NA
## 99              <NA>     NA         DC Comics           <NA>      good     NA
## 100            White  203.0     Marvel Comics           <NA>      good    158
## 101             <NA>     NA              SyFy           <NA>      good     NA
## 102            Blond  183.0     Marvel Comics           <NA>      good     81
## 103            blond  188.0     Marvel Comics           <NA>      good    108
## 104           Silver  193.0         DC Comics         silver      good     90
## 105            Blond  198.0     Marvel Comics           <NA>      good    116
## 106            Brown  188.0      Team Epic TV           <NA>      good     NA
## 107            Black     NA        South Park           <NA>      good     NA
## 108            Blond  188.0     Marvel Comics           <NA>      good    108
## 109            Blond  180.0     Marvel Comics           <NA>      good     74
## 110            Black  193.0         DC Comics           <NA>      good    101
## 111            Black  175.0         DC Comics           <NA>      good     74
## 112             <NA>     NA Dark Horse Comics           <NA>      good     NA
## 113            Green     NA     Marvel Comics           <NA>      good     NA
## 114             <NA>     NA     Marvel Comics           <NA>      good     NA
## 115            Blond  173.0     Marvel Comics           <NA>      good     61
## 116             <NA>     NA     Marvel Comics           <NA>      good     NA
## 117            Black  175.0         DC Comics           <NA>      good     61
## 118            Brown  170.0     Marvel Comics           <NA>      good     62
## 119            White  201.0     Marvel Comics           grey      good     97
## 120             <NA>     NA     Marvel Comics           <NA>      good     NA
## 121            Brown  175.0     Marvel Comics           <NA>      good     63
## 122             <NA>  178.0              <NA>           <NA>      good     NA
## 123              Red  183.0         DC Comics           <NA>      good    170
## 124            Blond     NA      NBC - Heroes           <NA>      good     NA
## 125            White     NA     Marvel Comics           <NA>      good     NA
## 126            black  226.0     Marvel Comics           <NA>      good     70
## 127            Brown     NA     HarperCollins           <NA>      good     NA
## 128             <NA>     NA         DC Comics           <NA>      good     NA
## 129            Black  226.0     Marvel Comics           <NA>      good    225
## 130            Brown  191.0     Marvel Comics           <NA>      good     79
## 131 Strawberry Blond     NA     Marvel Comics           <NA>      good     NA
## 132          No Hair  180.0     Marvel Comics           <NA>      good    104
## 133              Red  168.0     Marvel Comics           <NA>      good     50
## 134            Black  198.0         DC Comics           <NA>      good    173
## 135            Brown  191.0     Marvel Comics           <NA>      good     88
## 136            Blond  175.0     Marvel Comics           <NA>      good     68
## 137            Blond  165.0     Marvel Comics           <NA>      good     52
## 138            Blond     NA     HarperCollins           <NA>      good     NA
## 139             <NA>     NA       ABC Studios           <NA>      good     NA
## 140              Red  183.0     Marvel Comics           <NA>      good     90
## 141            Brown  185.0     Marvel Comics           <NA>      good     81
## 142             <NA>     NA Universal Studios           <NA>      good     NA
## 143            Blond  168.0     Marvel Comics           <NA>      good     56
## 144            Blond  122.0 Dark Horse Comics           <NA>      good     27
## 145            Brown     NA         Star Trek           <NA>      good     NA
## 146            Blond  173.0     Marvel Comics           <NA>      good     52
## 147            Black  183.0         DC Comics           <NA>      good     90
## 148             Grey  193.0     Marvel Comics           <NA>      good    178
## 149             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 150            Green  198.0     Marvel Comics           <NA>      good    171
## 151            Blond  188.0         DC Comics           <NA>      good     89
## 152            Black  188.0     Marvel Comics           <NA>      good     81
## 153            Black  173.0     Marvel Comics          white      good     54
## 154          No Hair     NA    IDW Publishing          green      good     NA
## 155            Black  175.0         DC Comics           <NA>      good     63
## 156          No Hair     NA         DC Comics           blue      good     NA
## 157          No Hair  193.0     Marvel Comics          green      good    306
## 158            Brown  168.0 Dark Horse Comics           <NA>      good     56
## 159            Black  175.0     Marvel Comics           <NA>      good     59
## 160              Red  185.0         DC Comics           <NA>      good     80
## 161            Blond  178.0     Marvel Comics           <NA>      good     65
## 162            Blond  168.0         DC Comics           <NA>      good     57
## 163             <NA>     NA     HarperCollins           <NA>      good     NA
## 164             <NA>     NA         DC Comics           <NA>      good     NA
## 165            Brown  168.0              <NA>           <NA>      good     NA
## 166            Black  188.0     Marvel Comics           <NA>      good    108
## 167   Orange / White  175.0     Marvel Comics           <NA>      good     50
## 168              Red     NA         DC Comics           <NA>      good     NA
## 169          No Hair  975.0     Marvel Comics          green      good     18
## 170            Black  165.0     Marvel Comics           <NA>      good     56
## 171           Yellow  193.0     Marvel Comics           <NA>      good     99
## 172              Red  173.0     Marvel Comics           <NA>      good     56
## 173            Black     NA         DC Comics           <NA>      good     NA
## 174           Auburn  188.0         DC Comics           <NA>      good     91
## 175    Brown / White  180.0         DC Comics           <NA>      good     81
## 176             <NA>     NA              <NA>           <NA>      good     NA
## 177            Blond  183.0         DC Comics           <NA>      good     88
## 178             <NA>  183.0         DC Comics           <NA>      good     86
## 179           Auburn  157.0         DC Comics           <NA>      good     52
## 180            Black  183.0     Marvel Comics           <NA>      good     81
## 181            Blond  142.0     Marvel Comics           <NA>      good     45
## 182             Grey  188.0     Marvel Comics           <NA>      good     92
## 183            White  180.0     Marvel Comics           <NA>      good    167
## 184            Brown  185.0     Marvel Comics           <NA>      good     81
## 185            Black  183.0     Marvel Comics          green      good     77
## 186             <NA>     NA         DC Comics           <NA>      good     NA
## 187             <NA>     NA              SyFy           <NA>      good     NA
## 188            Blond  185.0     Marvel Comics           <NA>      good     86
## 189          No Hair  188.0     Marvel Comics           <NA>      good     99
## 190             <NA>     NA     Marvel Comics           <NA>      good     NA
## 191             <NA>     NA     Marvel Comics           <NA>      good     NA
## 192             <NA>     NA     Marvel Comics           <NA>      good     NA
## 193             <NA>  175.0          Shueisha           <NA>      good     62
## 194             <NA>     NA     Marvel Comics           <NA>      good     NA
## 195             <NA>     NA     Marvel Comics           <NA>      good     NA
## 196             <NA>     NA     Marvel Comics           <NA>      good     NA
## 197            Black  183.0     Marvel Comics           <NA>      good     90
## 198            Brown  178.0     Marvel Comics           <NA>      good     79
## 199            Blond  188.0         DC Comics           <NA>      good     88
## 200             <NA>  183.0     Marvel Comics           <NA>      good     88
## 201            Brown  178.0     Marvel Comics           <NA>      good     79
## 202             <NA>  701.0     Marvel Comics           <NA>      good      4
## 203            Black     NA     Marvel Comics           <NA>      good     NA
## 204              Red  188.0         DC Comics           <NA>      good     95
## 205            Brown  188.0         DC Comics           <NA>      good     90
## 206            Brown  183.0      George Lucas           <NA>      good     79
## 207            Black  188.0     Sony Pictures           <NA>      good     NA
## 208            Black     NA     J. K. Rowling           <NA>      good     NA
## 209            Blond  183.0     Marvel Comics           <NA>      good     79
## 210            Brown  185.0         DC Comics           <NA>      good     89
## 211            Blond  191.0     Marvel Comics           <NA>      good    104
## 212            Black  165.0     Marvel Comics           <NA>      good     57
## 213              Red  175.0         DC Comics           <NA>      good     61
## 214            Brown  185.0         DC Comics           <NA>      good     88
## 215              Red  175.0         DC Comics           <NA>      good     54
## 216             <NA>     NA         DC Comics           <NA>      good     NA
## 217              Red  170.0         DC Comics           <NA>      good     65
## 218            Black  259.0 Dark Horse Comics           <NA>      good    158
## 219              Red  173.0     Marvel Comics           <NA>      good     61
## 220              Red  185.0     Marvel Comics           <NA>      good     81
## 221            Brown  196.0     Marvel Comics           <NA>      good    146
## 222             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 223             <NA>     NA       Icon Comics           <NA>      good     NA
## 224              Red  170.0     Marvel Comics           <NA>      good     NA
## 225              Red  168.0     Marvel Comics           <NA>      good     48
## 226           Yellow   79.0     Marvel Comics           <NA>      good     18
## 227            Green  244.0     Marvel Comics          green      good    630
## 228            Blond  178.0     Marvel Comics           <NA>      good     77
## 229            Black  180.0         DC Comics           <NA>      good     59
## 230            Blond  170.0     Marvel Comics           <NA>      good     58
## 231            Black  175.0     Marvel Comics           <NA>      good     77
## 232              Red  183.0     Marvel Comics           <NA>      good    207
## 233            Brown  173.0     Marvel Comics           <NA>      good     65
## 234           Auburn  170.0         DC Comics           <NA>      good     65
## 235             <NA>  183.0      George Lucas           <NA>      good     79
## 236          No Hair  180.0     Marvel Comics           <NA>      good     81
## 237            Blond  168.0     Marvel Comics           <NA>      good     54
## 238            Blond  180.0     Marvel Comics           <NA>      good     79
## 239            Black  198.0     Marvel Comics           <NA>      good    191
## 240             <NA>     NA         DC Comics           <NA>      good     NA
## 241             <NA>     NA              <NA>           <NA>      good     NA
## 242            Brown  155.0     Marvel Comics           <NA>      good     79
## 243            Brown   71.0 Dark Horse Comics           <NA>      good     14
## 244            Blond  183.0       Titan Books           <NA>      good     NA
## 245            Brown  178.0         Star Trek           <NA>      good     77
## 246             <NA>  193.0      George Lucas orange / white      good     NA
## 247             <NA>     NA              <NA>           <NA>      good     NA
## 248              Red  168.0     Marvel Comics           <NA>      good     52
## 249             <NA>     NA         Star Trek           <NA>      good     NA
## 250            Blond  168.0     Marvel Comics           <NA>      good     55
## 251             <NA>     NA         DC Comics           <NA>      good     NA
## 252            Brown     NA         DC Comics           <NA>      good     NA
## 253            Brown  170.0     Marvel Comics           <NA>      good     56
## 254             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 255             <NA>     NA       ABC Studios           <NA>      good     NA
## 256             <NA>     NA       ABC Studios           <NA>      good     NA
## 257             <NA>     NA Dark Horse Comics           <NA>      good     NA
## 258            Blond  183.0         DC Comics           <NA>      good     NA
## 259            Black  185.0         DC Comics           <NA>      good     90
## 260            Black  183.0     Marvel Comics           <NA>      good     88
## 261            Black  165.0     Marvel Comics           <NA>      good     49
## 262            Black  165.0     Marvel Comics           <NA>      good     52
## 263             <NA>  188.0         Rebellion           <NA>      good     NA
## 264            Brown  178.0     Marvel Comics           <NA>      good     81
## 265            Brown     NA      George Lucas           <NA>      good     NA
## 266          No Hair  213.0      George Lucas           gray      good     NA
## 267            Brown  173.0         DC Comics           <NA>      good     72
## 268             <NA>     NA         Star Trek           <NA>      good     NA
## 269             <NA>     NA              <NA>           <NA>      good     NA
## 270            Black     NA         DC Comics           <NA>      good     NA
## 271            Blond     NA       Icon Comics           <NA>      good     NA
## 272              Red     NA         DC Comics           <NA>      good     NA
## 273             <NA>     NA         DC Comics           <NA>      good     NA
## 274          No Hair  234.0         DC Comics           pink      good    324
## 275            Black   30.5              <NA>           <NA>      good     NA
## 276          No Hair     NA              <NA>            red      good     NA
## 277            White   64.0         DC Comics           <NA>      good     18
## 278            Black  180.0         DC Comics           <NA>      good     79
## 279             <NA>     NA     Marvel Comics           <NA>      good     NA
## 280            Black  175.0     Marvel Comics           <NA>      good     59
## 281          No Hair     NA    IDW Publishing          green      good     NA
## 282              Red  165.0         DC Comics           <NA>      good     54
## 283              Red  155.0         DC Comics           <NA>      good     65
## 284             <NA>     NA Dark Horse Comics           <NA>      good     NA
## 285            Blond  188.0     Marvel Comics           <NA>      good     36
## 286            Black  198.0     Marvel Comics           <NA>      good    191
## 287            Blond  168.0      George Lucas           <NA>      good     77
## 288             <NA>     NA     Marvel Comics           <NA>      good     NA
## 289            Green     NA     Marvel Comics           <NA>      good     NA
## 290            Black  183.0     Marvel Comics           <NA>      good    383
## 291            Blond     NA         DC Comics           <NA>      good     NA
## 292          No Hair  213.0     Marvel Comics          green      good    225
## 293           Auburn  188.0     Marvel Comics           <NA>      good     90
## 294            Black  168.0     Marvel Comics          green      good     52
## 295          No Hair  201.0         DC Comics          green      good    135
## 296              Red  170.0     Marvel Comics           <NA>      good     56
## 297            Black  183.0      Team Epic TV           <NA>      good     81
## 298            Brown  213.0         Microsoft           <NA>      good     NA
## 299             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 300            Black  193.0     Marvel Comics           <NA>      good    110
## 301             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 302              Red  180.0     Marvel Comics           <NA>      good     59
## 303            Blond  165.0     Marvel Comics           <NA>      good     54
## 304              Red  175.0         DC Comics           <NA>      good     72
## 305          No Hair  185.0         DC Comics           <NA>      good     90
## 306             <NA>     NA     Marvel Comics           <NA>      good     NA
## 307            Black  185.0         DC Comics           <NA>      good     86
## 308            Black     NA      NBC - Heroes           <NA>      good     NA
## 309             <NA>     NA    IDW Publishing          green      good     NA
## 310            Brown  183.0         DC Comics           <NA>      good     77
## 311            Brown  188.0     Marvel Comics           <NA>      good    101
## 312             <NA>     NA         Wildstorm           <NA>      good     NA
## 313              Red     NA         DC Comics           <NA>      good     NA
## 314              Red  178.0         DC Comics           <NA>      good     61
## 315            Brown  185.0     Marvel Comics           <NA>      good     81
## 316            Blond  175.0     Marvel Comics           <NA>      good     61
## 317             <NA>     NA         DC Comics           <NA>      good     NA
## 318             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 319            White  193.0         DC Comics           <NA>      good     90
## 320             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 321            Brown  188.0     Marvel Comics           <NA>      good    101
## 322          No Hair  178.0     Marvel Comics           <NA>      good     79
## 323            Blond  188.0     Marvel Comics           <NA>      good     70
## 324            Blond  201.0 Dark Horse Comics           <NA>      good    158
## 325              Red  173.0     Marvel Comics           <NA>      good     61
## 326            Brown  180.0     Marvel Comics           <NA>      good     70
## 327             <NA>     NA     Marvel Comics           <NA>      good     NA
## 328            Black  188.0     Marvel Comics           <NA>      good    125
## 329            Blond  180.0     Marvel Comics           <NA>      good     85
## 330            Blond  168.0     Marvel Comics           <NA>      good    101
## 331             <NA>  168.0          Shueisha           <NA>      good     54
## 332             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 333            Black     NA     Marvel Comics           <NA>      good     NA
## 334    Brown / White  185.0     Marvel Comics           <NA>      good     99
## 335           Indigo  175.0     Marvel Comics           <NA>      good     88
## 336            Black  178.0         DC Comics           <NA>      good     79
## 337            Blond     NA      NBC - Heroes           <NA>      good     NA
## 338             <NA>     NA              SyFy           <NA>      good     NA
## 339             <NA>     NA         DC Comics           <NA>      good     NA
## 340            Black  180.0     Marvel Comics           <NA>      good     83
## 341            Brown  185.0     Marvel Comics           <NA>      good     86
## 342              Red  163.0     Marvel Comics           gold      good     59
## 343            White  206.0     Marvel Comics           <NA>      good    293
## 344             <NA>     NA         DC Comics           <NA>      good     NA
## 345            Black  180.0      Team Epic TV           <NA>      good     65
## 346          No Hair  175.0          Shueisha           <NA>      good     69
## 347              Red  178.0         DC Comics           <NA>      good     59
## 348            Brown     NA         DC Comics           <NA>      good     NA
## 349             <NA>  170.0     Sony Pictures           <NA>      good    117
## 350             <NA>     NA     Marvel Comics           <NA>      good     NA
## 351             <NA>     NA     Marvel Comics           <NA>      good     NA
## 352            Blond  183.0     Marvel Comics           <NA>      good     89
## 353             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 354             <NA>     NA         DC Comics           <NA>      good     NA
## 355            Black  168.0         DC Comics           <NA>      good     54
## 356              Red  168.0     Marvel Comics           <NA>      good     52
## 357             <NA>     NA         DC Comics           <NA>      good     NA
## 358            Black  185.0         DC Comics           <NA>      good     80
## 359            Green  170.0     Marvel Comics           <NA>      good     52
## 360            blond  180.0         DC Comics           <NA>      good     81
## 361             <NA>     NA     Marvel Comics           <NA>      good     NA
## 362          No Hair  183.0     Marvel Comics           <NA>      good     86
## 363           Purple  180.0     Marvel Comics           <NA>      good     70
## 364            Black  183.0     Marvel Comics           <NA>      good     90
## 365             <NA>     NA     HarperCollins           <NA>      good     NA
## 366            Blond  188.0         DC Comics           <NA>      good     83
## 367           Silver  183.0     Marvel Comics           <NA>      good     79
## 368            Brown  163.0     Marvel Comics           <NA>      good     56
## 369             <NA>     NA              SyFy           <NA>      good     NA
## 370            Black  178.0              <NA>           <NA>      good     83
## 371          No Hair     NA    IDW Publishing          green      good     NA
## 372              Red  178.0         DC Comics           <NA>      good     70
## 373              Red  180.0         DC Comics           <NA>      good     83
## 374            Black  165.0         DC Comics           <NA>      good     56
## 375          No Hair  185.0         DC Comics           <NA>      good    146
## 376             <NA>     NA     HarperCollins           <NA>      good     NA
## 377            Brown  297.0      George Lucas           <NA>      good     NA
## 378            Blond     NA         DC Comics           <NA>      good     NA
## 379            Black  180.0     Marvel Comics           <NA>      good     72
## 380            Black  178.0         DC Comics           <NA>      good     79
## 381              Red  183.0         DC Comics           <NA>      good    101
## 382            Black  165.0         DC Comics           <NA>      good     56
## 383            Black  137.0         DC Comics           <NA>      good     38
## 384            Brown  122.0     Marvel Comics           <NA>      good     25
## 385    Brown / White  173.0     Marvel Comics           <NA>      good     54
## 386            Blond  191.0     Marvel Comics           <NA>      good    104
## 387              Red  168.0         DC Comics           <NA>      good     63
## 388            Black  170.0     Marvel Comics           <NA>      good     61
## 389           Orange  305.0     Marvel Comics           <NA>      good    900
## 390             <NA>     NA      Image Comics           <NA>      good     NA
## 391            Blond  178.0     Marvel Comics           <NA>      good     74
## 392            Brown  193.0     Marvel Comics           <NA>      good    113
## 393             <NA>  185.0     Marvel Comics           <NA>      good    149
## 394            Black  173.0         DC Comics           blue      good     54
## 395            Brown  168.0     Marvel Comics           <NA>      good     50
## 396            Black  178.0     Marvel Comics           <NA>      good     79
## 397              Red  191.0     Marvel Comics           <NA>      good     88
## 398            Green  201.0     Marvel Comics           <NA>      good    315
## 399          No Hair  183.0     Marvel Comics           <NA>      good    153
## 400            Black  173.0     Marvel Comics           <NA>      good     52
## 401             <NA>     NA         DC Comics           <NA>      good     NA
## 402            Black  188.0     Marvel Comics           <NA>      good    191
## 403            Black     NA     Marvel Comics           <NA>      good     NA
## 404             <NA>     NA         DC Comics           <NA>      good     NA
## 405             <NA>     NA         DC Comics           <NA>      good     NA
## 406          No Hair  193.0     Marvel Comics         silver      good    101
## 407            Black  157.0     Marvel Comics           <NA>      good     50
## 408            Black     NA         DC Comics           <NA>      good     NA
## 409            Black  198.0     Marvel Comics           <NA>      good    180
## 410            Blond  178.0     Marvel Comics           <NA>      good     49
## 411          No Hair     NA         DC Comics           <NA>      good     NA
## 412      Red / White  165.0     Marvel Comics           <NA>      good     65
## 413             <NA>  188.0         DC Comics           <NA>      good    113
## 414            Black  211.0      Image Comics           <NA>      good    405
## 415          No Hair     NA         DC Comics          white      good     NA
## 416             <NA>     NA     Marvel Comics           <NA>      good     NA
## 417             <NA>     NA         DC Comics           <NA>      good     NA
## 418            Brown     NA         DC Comics           <NA>      good     NA
## 419            Brown  170.0     Marvel Comics           <NA>      good     54
## 420            Blond  165.0     Marvel Comics           <NA>      good     56
## 421            Brown  178.0     Marvel Comics           <NA>      good     74
## 422            Brown  178.0     Marvel Comics           <NA>      good     77
## 423            Black  157.0     Marvel Comics           <NA>      good     56
## 424            Black  178.0     Marvel Comics           <NA>      good     59
## 425             <NA>     NA     Marvel Comics           <NA>      good     NA
## 426            Brown  173.0     Marvel Comics           <NA>      good     55
## 427            Black  185.0         Star Trek           <NA>      good     81
## 428            Blond  183.0     Marvel Comics           <NA>      good     83
## 429             <NA>     NA     Marvel Comics           <NA>      good     NA
## 430            Blond  188.0     Marvel Comics           <NA>      good     79
## 431             <NA>     NA     Marvel Comics           <NA>      good     NA
## 432           Auburn  193.0         DC Comics         orange      good     71
## 433            Blond  165.0         DC Comics           <NA>      good     62
## 434            Black  170.0         DC Comics           <NA>      good     63
## 435          No Hair  201.0         DC Comics           <NA>      good    131
## 436            Blond     NA       ABC Studios           <NA>      good     NA
## 437            White  180.0     Marvel Comics           <NA>      good     57
## 438            black  173.0     Marvel Comics           <NA>      good     77
## 439            Black  170.0         DC Comics           <NA>      good     68
## 440            Blond  165.0         DC Comics           <NA>      good     54
## 441            Black  191.0         DC Comics           <NA>      good    101
## 442            Black  180.0     Marvel Comics           <NA>      good     74
## 443            Black  163.0     Marvel Comics           <NA>      good     54
## 444             <NA>     NA              <NA>           <NA>      good     NA
## 445          No Hair  183.0     Marvel Comics           <NA>      good    225
## 446            Blond  198.0     Marvel Comics           <NA>      good    288
## 447            Blond  175.0     Marvel Comics           <NA>      good    143
## 448            Black  185.0     Marvel Comics           <NA>      good    101
## 449             <NA>     NA     Marvel Comics           <NA>      good     NA
## 450            Black  175.0     Marvel Comics           <NA>      good     74
## 451            Blond  198.0     Marvel Comics           <NA>      good    288
## 452              Red  218.0     Marvel Comics           <NA>      good    158
## 453           Auburn  178.0     Marvel Comics           <NA>      good     81
## 454             <NA>     NA     HarperCollins           <NA>      good     NA
## 455            Brown  188.0     Marvel Comics           <NA>      good     97
## 456            Blond  191.0     Marvel Comics           <NA>      good    117
## 457             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 458            Brown  168.0         DC Comics           <NA>      good     59
## 459          No Hair  188.0     Marvel Comics          green      good     86
## 460            Blond  168.0     Marvel Comics           <NA>      good    105
## 461 Strawberry Blond  168.0     Marvel Comics           <NA>      good     54
## 462            Black  175.0      Team Epic TV           <NA>      good     56
## 463            Blond  191.0     Marvel Comics           <NA>      good    214
## 464           Silver  168.0     Marvel Comics           <NA>      good     52
## 465            Black  178.0         DC Comics           <NA>      good     71
## 466              Red  165.0     Marvel Comics           <NA>      good     54
## 467             <NA>     NA     Marvel Comics           <NA>      good     NA
## 468            Black  137.0 Dark Horse Comics           <NA>      good     41
## 469          No Hair  191.0     Marvel Comics            red      good    135
## 470          No Hair  191.0     Marvel Comics           <NA>      good    135
## 471            Black  175.0         DC Comics           <NA>      good     63
## 472            Black     NA     Marvel Comics           <NA>      good     NA
## 473            Brown  185.0     Marvel Comics           <NA>      good     95
## 474            Blond  180.0     Marvel Comics           <NA>      good     54
## 475            Blond  188.0     Marvel Comics           <NA>      good    108
## 476            Black  218.0     Marvel Comics           <NA>      good    158
## 477           Auburn  163.0     Marvel Comics           <NA>      good     50
## 478             <NA>     NA     Marvel Comics           <NA>      good     NA
## 479            Blond  178.0     Marvel Comics           <NA>      good     65
## 480             <NA>     NA         DC Comics           <NA>      good     NA
## 481            Brown  175.0     Marvel Comics           <NA>      good    117
## 482            Black  140.0     Marvel Comics           <NA>      good     39
## 483           Auburn  366.0     Marvel Comics           <NA>      good    473
## 484            Black  160.0     Marvel Comics           <NA>      good    135
## 485            Blond  165.0         DC Comics           <NA>      good     51
## 486            Black  188.0     Marvel Comics           <NA>      good    171
## 487            Black  183.0         DC Comics           <NA>      good     74
## 488             <NA>     NA     Marvel Comics           <NA>      good     NA
## 489            Black  196.0     Marvel Comics           <NA>      good    117
## 490            Black  155.0     Marvel Comics           <NA>      good     50
## 491            Brown  175.0     Marvel Comics           <NA>      good     61
## 492            Blond  183.0     Marvel Comics           <NA>      good     83
## 493 Strawberry Blond  165.0     Marvel Comics           <NA>      good     52
## 494          No Hair  304.8     Marvel Comics          white      good     NA
## 495            White   66.0      George Lucas          green      good     17
## 496            Black  170.0         DC Comics           <NA>      good     57
```


```r
bad_guys <- data.frame(filter(superhero_info,Alignment=='bad'))
bad_guys
```

```
##                  Name Gender               Eye.color               Race
## 1         Abomination   Male                   green  Human / Radiation
## 2             Abraxas   Male                    blue      Cosmic Entity
## 3       Absorbing Man   Male                    blue              Human
## 4          Air-Walker   Male                    blue               <NA>
## 5                Ajax   Male                   brown             Cyborg
## 6         Alex Mercer   Male                    <NA>              Human
## 7               Alien   Male                    <NA>    Xenomorph XX121
## 8               Amazo   Male                     red            Android
## 9                Ammo   Male                   brown              Human
## 10             Angela Female                    <NA>               <NA>
## 11          Annihilus   Male                   green               <NA>
## 12       Anti-Monitor   Male                  yellow      God / Eternal
## 13         Anti-Spawn   Male                    <NA>               <NA>
## 14         Apocalypse   Male                     red             Mutant
## 15           Arclight Female                  violet               <NA>
## 16              Atlas   Male                    blue      God / Eternal
## 17             Azazel   Male                  yellow           Neyaphem
## 18               Bane   Male                    <NA>              Human
## 19             Beetle   Male                    <NA>               <NA>
## 20          Big Barda Female                    blue            New God
## 21            Big Man   Male                    blue               <NA>
## 22      Billy Kincaid   Male                    <NA>               <NA>
## 23           Bird-Man   Male                    <NA>              Human
## 24        Bird-Man II   Male                    <NA>              Human
## 25       Black Abbott   Male                     red               <NA>
## 26         Black Adam   Male                   brown               <NA>
## 27        Black Mamba Female                   green               <NA>
## 28        Black Manta   Male                   black              Human
## 29           Blackout   Male                     red              Demon
## 30          Blackwing   Male                    blue               <NA>
## 31           Blizzard   Male                    <NA>               <NA>
## 32           Blizzard   Male                    <NA>               <NA>
## 33        Blizzard II   Male                   brown               <NA>
## 34               Blob   Male                   brown               <NA>
## 35           Bloodaxe Female                    blue              Human
## 36        Bloodwraith   Male                   white               <NA>
## 37          Boba Fett   Male                   brown      Human / Clone
## 38         Bomb Queen Female                    <NA>               <NA>
## 39           Brainiac   Male                   green            Android
## 40           Bullseye   Male                    blue              Human
## 41           Callisto Female                    blue               <NA>
## 42            Carnage   Male                   green           Symbiote
## 43          Chameleon   Male                    <NA>               <NA>
## 44         Changeling   Male                   brown               <NA>
## 45            Cheetah Female                   green              Human
## 46         Cheetah II Female                   green              Human
## 47        Cheetah III Female                   brown              Human
## 48            Chromos   Male                   brown               <NA>
## 49         Clock King   Male                    blue              Human
## 50         Cogliostro   Male                    <NA>               <NA>
## 51        Cottonmouth   Male                   brown              Human
## 52              Curse   Male                    <NA>               <NA>
## 53             Cy-Gor   Male                    <NA>               <NA>
## 54    Cyborg Superman   Male                    blue             Cyborg
## 55           Darkseid   Male                     red            New God
## 56           Darkside   <NA>                    <NA>               <NA>
## 57         Darth Maul   Male            yellow / red Dathomirian Zabrak
## 58        Darth Vader   Male                  yellow             Cyborg
## 59           Deadshot   Male                   brown              Human
## 60         Demogoblin   Male                     red              Demon
## 61          Destroyer   Male                    <NA>               <NA>
## 62        Diamondback   Male                   brown              Human
## 63        Doctor Doom   Male                   brown              Human
## 64     Doctor Doom II   Male                   brown               <NA>
## 65     Doctor Octopus   Male                   brown              Human
## 66           Doomsday   Male                     red              Alien
## 67       Doppelganger   Male                   white               <NA>
## 68           Dormammu   Male                  yellow               <NA>
## 69                Ego   <NA>                    <NA>               <NA>
## 70            Electro   Male                    blue              Human
## 71        Elle Bishop Female                    blue               <NA>
## 72      Evil Deadpool   Male                   white             Mutant
## 73           Evilhawk   Male                     red              Alien
## 74             Exodus   Male                    blue             Mutant
## 75      Fabian Cortez   <NA>                    blue               <NA>
## 76      Fallen One II   Male                   black               <NA>
## 77              Faora Female                    <NA>         Kryptonian
## 78              Fixer   <NA>                     red               <NA>
## 79             Frenzy Female                   brown               <NA>
## 80        General Zod   Male                   black         Kryptonian
## 81            Giganta Female                   green               <NA>
## 82       Goblin Queen Female                   green               <NA>
## 83           Godzilla   <NA>                    <NA>              Kaiju
## 84                Gog   Male                    <NA>               <NA>
## 85      Gorilla Grodd   Male                  yellow            Gorilla
## 86    Granny Goodness Female                    blue               <NA>
## 87             Greedo   Male                  purple             Rodian
## 88       Green Goblin   Male                    blue              Human
## 89    Green Goblin II   Male                    blue               <NA>
## 90       Harley Quinn Female                    blue              Human
## 91          Heat Wave   Male                    blue              Human
## 92               Hela Female                   green          Asgardian
## 93          Hobgoblin   Male                    blue               <NA>
## 94          Hydro-Man   Male                   brown               <NA>
## 95        Iron Monger   Male                    blue               <NA>
## 96             Jigsaw   Male                    blue               <NA>
## 97              Joker   Male                   green              Human
## 98           Junkpile   Male                    <NA>             Mutant
## 99               Kang   Male                   brown               <NA>
## 100       Killer Croc   Male                     red          Metahuman
## 101      Killer Frost Female                    blue              Human
## 102        King Shark   Male                   black             Animal
## 103           Kingpin   Male                    blue              Human
## 104              Klaw   Male                     red              Human
## 105         Kraven II   Male                   brown              Human
## 106 Kraven the Hunter   Male                   brown              Human
## 107          Kylo Ren   Male                    <NA>              Human
## 108     Lady Bullseye Female                    <NA>               <NA>
## 109  Lady Deathstrike Female                   brown             Cyborg
## 110            Leader   Male                   green               <NA>
## 111        Lex Luthor   Male                   green              Human
## 112    Lightning Lord   Male                    blue               <NA>
## 113      Living Brain   <NA>                  yellow               <NA>
## 114            Lizard   Male                     red              Human
## 115              Loki   Male                   green          Asgardian
## 116     Luke Campbell   Male                    <NA>               <NA>
## 117           Mach-IV   Male                   brown               <NA>
## 118           Magneto   Male                    grey             Mutant
## 119             Magus   Male                   black               <NA>
## 120          Mandarin   Male                    blue              Human
## 121             Match   Male                   black               <NA>
## 122            Maxima Female                   brown               <NA>
## 123          Mephisto   Male                   white               <NA>
## 124           Metallo   Male                   green            Android
## 125     Mister Freeze   Male                    <NA>              Human
## 126      Mister Knife   Male                    blue            Spartoi
## 127   Mister Mxyzptlk   Male                    <NA>      God / Eternal
## 128   Mister Sinister   Male                     red    Human / Altered
## 129      Mister Zsasz   Male                    blue              Human
## 130             MODOK   Male                   white             Cyborg
## 131            Moloch   Male                    <NA>               <NA>
## 132        Molten Man   Male                    gold               <NA>
## 133         Moonstone Female                    blue               <NA>
## 134            Morlun   Male             white / red               <NA>
## 135      Moses Magnum   Male                   brown               <NA>
## 136          Mysterio   Male                   brown              Human
## 137          Mystique Female yellow (without irises)             Mutant
## 138            Nebula Female                    blue          Luphomoid
## 139         Omega Red   Male                     red               <NA>
## 140         Onslaught   Male                     red             Mutant
## 141         Overtkill   Male                    <NA>               <NA>
## 142        Ozymandias   Male                    blue              Human
## 143         Parademon   <NA>                    <NA>          Parademon
## 144           Penguin   Male                    blue              Human
## 145          Plantman   Male                   green             Mutant
## 146         Plastique Female                    blue               <NA>
## 147        Poison Ivy Female                   green              Human
## 148          Predator   Male                    <NA>             Yautja
## 149    Professor Zoom   Male                    blue              Human
## 150      Proto-Goblin   Male                   green               <NA>
## 151        Purple Man   Male                  purple              Human
## 152              Pyro   Male                    blue               <NA>
## 153      Ra's Al Ghul   Male                   green              Human
## 154     Razor-Fist II   Male                    blue               <NA>
## 155          Red Mist   Male                    <NA>               <NA>
## 156         Red Skull   Male                    blue               <NA>
## 157       Redeemer II   Male                    <NA>               <NA>
## 158      Redeemer III   Male                    <NA>               <NA>
## 159             Rhino   Male                   brown  Human / Radiation
## 160         Rick Flag   Male                    blue               <NA>
## 161           Riddler   Male                    <NA>               <NA>
## 162        Sabretooth   Male                   amber             Mutant
## 163            Sauron   Male                    <NA>              Maiar
## 164         Scarecrow   Male                    blue              Human
## 165     Scarlet Witch Female                    blue             Mutant
## 166           Scorpia Female                   green               <NA>
## 167          Scorpion   Male                   brown              Human
## 168    Sebastian Shaw   Male                    <NA>             Mutant
## 169           Shocker   Male                   brown              Human
## 170             Siren Female                    blue          Atlantean
## 171          Siren II Female                   black               <NA>
## 172             Siryn Female                    blue               <NA>
## 173        Snake-Eyes   Male                    <NA>             Animal
## 174    Solomon Grundy   Male                   black             Zombie
## 175    Spider-Carnage   Male                    <NA>           Symbiote
## 176   Spider-Woman IV Female                     red               <NA>
## 177       Steppenwolf   Male                     red            New God
## 178      Stormtrooper   Male                    <NA>              Human
## 179    Superboy-Prime   Male                    blue         Kryptonian
## 180       Swamp Thing   Male                     red      God / Eternal
## 181             Swarm   Male                  yellow             Mutant
## 182             Sylar   Male                    <NA>               <NA>
## 183            T-1000   Male                    <NA>            Android
## 184             T-800   Male                     red             Cyborg
## 185             T-850   Male                     red             Cyborg
## 186               T-X Female                    <NA>             Cyborg
## 187        Taskmaster   Male                   brown              Human
## 188            Thanos   Male                     red            Eternal
## 189       Tiger Shark   Male                    grey              Human
## 190          Tinkerer   Male                   brown               <NA>
## 191            Trigon   Male                  yellow      God / Eternal
## 192          Two-Face   Male                    <NA>               <NA>
## 193            Ultron   Male                     red            Android
## 194       Utgard-Loki   Male                    blue        Frost Giant
## 195          Vanisher   Male                   green               <NA>
## 196            Vegeta   Male                    <NA>             Saiyan
## 197             Venom   Male                    blue           Symbiote
## 198          Venom II   Male                   brown               <NA>
## 199         Venom III   Male                   brown           Symbiote
## 200          Violator   Male                    <NA>               <NA>
## 201           Vulture   Male                   brown              Human
## 202            Walrus   Male                    blue              Human
## 203              Warp   Male                   brown               <NA>
## 204         Weapon XI   Male                    <NA>               <NA>
## 205      White Canary Female                   brown              Human
## 206       Yellow Claw   Male                    blue               <NA>
## 207              Zoom   Male                     red               <NA>
##           Hair.color Height         Publisher  Skin.color Alignment Weight
## 1            No Hair  203.0     Marvel Comics        <NA>       bad    441
## 2              Black     NA     Marvel Comics        <NA>       bad     NA
## 3            No Hair  193.0     Marvel Comics        <NA>       bad    122
## 4              White  188.0     Marvel Comics        <NA>       bad    108
## 5              Black  193.0     Marvel Comics        <NA>       bad     90
## 6               <NA>     NA         Wildstorm        <NA>       bad     NA
## 7            No Hair  244.0 Dark Horse Comics       black       bad    169
## 8               <NA>  257.0         DC Comics        <NA>       bad    173
## 9              Black  188.0     Marvel Comics        <NA>       bad    101
## 10              <NA>     NA      Image Comics        <NA>       bad     NA
## 11           No Hair  180.0     Marvel Comics        <NA>       bad     90
## 12           No Hair   61.0         DC Comics        <NA>       bad     NA
## 13              <NA>     NA      Image Comics        <NA>       bad     NA
## 14             Black  213.0     Marvel Comics        grey       bad    135
## 15            Purple  173.0     Marvel Comics        <NA>       bad     57
## 16             Brown  198.0         DC Comics        <NA>       bad    126
## 17             Black  183.0     Marvel Comics         red       bad     67
## 18              <NA>  203.0         DC Comics        <NA>       bad    180
## 19              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 20             Black  188.0         DC Comics        <NA>       bad    135
## 21             Brown  165.0     Marvel Comics        <NA>       bad     71
## 22              <NA>     NA      Image Comics        <NA>       bad     NA
## 23              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 24              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 25             Black     NA     Marvel Comics        <NA>       bad     NA
## 26             Black  191.0         DC Comics        <NA>       bad    113
## 27             Black  170.0     Marvel Comics        <NA>       bad     52
## 28           No Hair  188.0         DC Comics        <NA>       bad     92
## 29             White  191.0     Marvel Comics       white       bad    104
## 30             Black  185.0     Marvel Comics        <NA>       bad     86
## 31              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 32             Brown     NA     Marvel Comics        <NA>       bad     NA
## 33             Brown  175.0     Marvel Comics        <NA>       bad     77
## 34             Brown  178.0     Marvel Comics        <NA>       bad    230
## 35             Brown  218.0     Marvel Comics        <NA>       bad    495
## 36           No Hair   30.5     Marvel Comics        <NA>       bad     NA
## 37             Black  183.0      George Lucas        <NA>       bad     NA
## 38              <NA>     NA      Image Comics        <NA>       bad     NA
## 39           No Hair  198.0         DC Comics       green       bad    135
## 40             blond  183.0     Marvel Comics        <NA>       bad     90
## 41             Black  175.0     Marvel Comics        <NA>       bad     74
## 42               Red  185.0     Marvel Comics        <NA>       bad     86
## 43              <NA>     NA         DC Comics        <NA>       bad     NA
## 44             Black  180.0     Marvel Comics        <NA>       bad     81
## 45             Blond  163.0         DC Comics        <NA>       bad     50
## 46             Brown  170.0         DC Comics        <NA>       bad     55
## 47             Brown  175.0         DC Comics        <NA>       bad     54
## 48        Red / Grey  185.0      Team Epic TV        <NA>       bad     86
## 49             Black  178.0         DC Comics        <NA>       bad     78
## 50              <NA>     NA      Image Comics        <NA>       bad     NA
## 51             Black  183.0     Marvel Comics        <NA>       bad     99
## 52              <NA>     NA      Image Comics        <NA>       bad     NA
## 53              <NA>     NA      Image Comics        <NA>       bad     NA
## 54             Black     NA         DC Comics        <NA>       bad     NA
## 55           No Hair  267.0         DC Comics        grey       bad    817
## 56              <NA>     NA              <NA>        <NA>       bad     NA
## 57              <NA>  170.0      George Lucas red / black       bad     NA
## 58           No Hair  198.0      George Lucas        <NA>       bad    135
## 59             Brown  185.0         DC Comics        <NA>       bad     91
## 60           No Hair  185.0     Marvel Comics        <NA>       bad     95
## 61              <NA>  188.0     Marvel Comics        <NA>       bad    383
## 62             Black  193.0     Marvel Comics        <NA>       bad     90
## 63             Brown  201.0     Marvel Comics        <NA>       bad    187
## 64             Brown  201.0     Marvel Comics        <NA>       bad    132
## 65             Brown  175.0     Marvel Comics        <NA>       bad    110
## 66             White  244.0         DC Comics        <NA>       bad    412
## 67           No Hair  196.0     Marvel Comics        <NA>       bad    104
## 68           No Hair  185.0     Marvel Comics        <NA>       bad     NA
## 69              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 70            Auburn  180.0     Marvel Comics        <NA>       bad     74
## 71             Blond     NA      NBC - Heroes        <NA>       bad     NA
## 72               Red  188.0     Marvel Comics        <NA>       bad     95
## 73             Black  191.0     Marvel Comics       green       bad    106
## 74             Black  183.0     Marvel Comics         red       bad     88
## 75             Brown  196.0     Marvel Comics        <NA>       bad     96
## 76              Blue     NA     Marvel Comics        <NA>       bad     NA
## 77              <NA>     NA         DC Comics        <NA>       bad     NA
## 78           No Hair     NA     Marvel Comics        <NA>       bad     NA
## 79             Black  211.0     Marvel Comics        <NA>       bad    104
## 80             Black     NA         DC Comics        <NA>       bad     NA
## 81               Red   62.5         DC Comics        <NA>       bad    630
## 82               Red  168.0     Marvel Comics        <NA>       bad     50
## 83              <NA>  108.0              <NA>        grey       bad     NA
## 84              <NA>     NA         DC Comics        <NA>       bad     NA
## 85             Black  198.0         DC Comics        <NA>       bad    270
## 86             White  178.0         DC Comics        <NA>       bad    115
## 87              <NA>  170.0      George Lucas       green       bad     NA
## 88            Auburn  180.0     Marvel Comics        <NA>       bad     83
## 89            Auburn  178.0     Marvel Comics        <NA>       bad     77
## 90             Blond  170.0         DC Comics        <NA>       bad     63
## 91           No Hair  180.0         DC Comics        <NA>       bad     81
## 92             Black  213.0     Marvel Comics        <NA>       bad    225
## 93              Grey  180.0     Marvel Comics        <NA>       bad     83
## 94             Brown  188.0     Marvel Comics        <NA>       bad    119
## 95           No Hair     NA     Marvel Comics        <NA>       bad      2
## 96             Black  188.0     Marvel Comics        <NA>       bad    113
## 97             Green  196.0         DC Comics       white       bad     86
## 98              <NA>     NA     Marvel Comics        <NA>       bad     NA
## 99             Brown  191.0     Marvel Comics        <NA>       bad    104
## 100          No Hair  244.0         DC Comics       green       bad    356
## 101            Blond     NA         DC Comics        blue       bad     NA
## 102          No Hair     NA         DC Comics        <NA>       bad     NA
## 103          No Hair  201.0     Marvel Comics        <NA>       bad    203
## 104          No Hair  188.0     Marvel Comics         red       bad     97
## 105            Black  191.0     Marvel Comics        <NA>       bad     99
## 106            Black  183.0     Marvel Comics        <NA>       bad    106
## 107             <NA>     NA      George Lucas        <NA>       bad     NA
## 108            Black     NA     Marvel Comics        <NA>       bad     NA
## 109            Black  175.0     Marvel Comics        <NA>       bad     58
## 110          No Hair  178.0     Marvel Comics        <NA>       bad     63
## 111          No Hair  188.0         DC Comics        <NA>       bad     95
## 112              Red  191.0         DC Comics        <NA>       bad     95
## 113             <NA>  198.0     Marvel Comics        <NA>       bad    360
## 114          No Hair  203.0     Marvel Comics        <NA>       bad    230
## 115            Black  193.0     Marvel Comics        <NA>       bad    236
## 116             <NA>     NA      NBC - Heroes        <NA>       bad     NA
## 117            Brown  180.0     Marvel Comics        <NA>       bad     79
## 118            White  188.0     Marvel Comics        <NA>       bad     86
## 119             <NA>  183.0     Marvel Comics        <NA>       bad     NA
## 120            White  188.0     Marvel Comics        <NA>       bad     97
## 121            Black     NA         DC Comics        <NA>       bad     NA
## 122              Red  180.0         DC Comics        <NA>       bad     72
## 123            Black  198.0     Marvel Comics        <NA>       bad    140
## 124            Brown  196.0         DC Comics        <NA>       bad     90
## 125             <NA>  183.0         DC Comics        <NA>       bad     86
## 126            Brown     NA     Marvel Comics        <NA>       bad     NA
## 127             <NA>     NA         DC Comics        <NA>       bad     NA
## 128            Black  196.0     Marvel Comics        <NA>       bad    128
## 129            Blond     NA         DC Comics        <NA>       bad     NA
## 130           Brownn  366.0     Marvel Comics        <NA>       bad    338
## 131             <NA>     NA         DC Comics        <NA>       bad     NA
## 132             Gold  196.0     Marvel Comics        <NA>       bad    248
## 133            Blond  180.0     Marvel Comics        <NA>       bad     59
## 134            Black  188.0     Marvel Comics        <NA>       bad     79
## 135            Black  175.0     Marvel Comics        <NA>       bad     72
## 136          No Hair  180.0     Marvel Comics        <NA>       bad     79
## 137     Red / Orange  178.0     Marvel Comics        blue       bad     54
## 138          No Hair  185.0     Marvel Comics        blue       bad     83
## 139            Blond  211.0     Marvel Comics        <NA>       bad    191
## 140          No Hair  305.0     Marvel Comics        <NA>       bad    405
## 141             <NA>     NA      Image Comics        <NA>       bad     NA
## 142            Blond     NA         DC Comics        <NA>       bad     NA
## 143             <NA>     NA         DC Comics        <NA>       bad     NA
## 144            Black  157.0         DC Comics        <NA>       bad     79
## 145             Grey  183.0     Marvel Comics        <NA>       bad     87
## 146              Red  168.0         DC Comics        <NA>       bad     55
## 147              Red  168.0         DC Comics       green       bad     50
## 148             <NA>  213.0 Dark Horse Comics        <NA>       bad    234
## 149 Strawberry Blond  180.0         DC Comics        <NA>       bad     81
## 150            Blond     NA     Marvel Comics        <NA>       bad     NA
## 151           Purple  180.0     Marvel Comics      purple       bad     74
## 152            Blond  178.0     Marvel Comics        <NA>       bad     68
## 153             Grey  193.0         DC Comics        <NA>       bad     97
## 154          No Hair  191.0     Marvel Comics        <NA>       bad    117
## 155             <NA>     NA       Icon Comics        <NA>       bad     NA
## 156          No Hair  188.0     Marvel Comics        <NA>       bad    108
## 157             <NA>     NA      Image Comics        <NA>       bad     NA
## 158             <NA>     NA      Image Comics        <NA>       bad     NA
## 159            Brown  196.0     Marvel Comics        <NA>       bad    320
## 160            Brown  185.0         DC Comics        <NA>       bad     85
## 161             <NA>     NA         DC Comics        <NA>       bad     NA
## 162            Blond  198.0     Marvel Comics        <NA>       bad    171
## 163             <NA>  279.0  J. R. R. Tolkien        <NA>       bad     NA
## 164            Brown  183.0         DC Comics        <NA>       bad     63
## 165            Brown  170.0     Marvel Comics        <NA>       bad     59
## 166              Red     NA     Marvel Comics        <NA>       bad     NA
## 167            Brown  211.0     Marvel Comics        <NA>       bad    310
## 168             <NA>     NA     Marvel Comics        <NA>       bad     NA
## 169            Brown  175.0     Marvel Comics        <NA>       bad     79
## 170           Purple  175.0         DC Comics        <NA>       bad     72
## 171             <NA>     NA         DC Comics        <NA>       bad     NA
## 172 Strawberry Blond  168.0     Marvel Comics        <NA>       bad     52
## 173             <NA>     NA     Marvel Comics        <NA>       bad     NA
## 174            White  279.0         DC Comics        <NA>       bad    437
## 175             <NA>     NA     Marvel Comics        <NA>       bad     NA
## 176            White  178.0     Marvel Comics        <NA>       bad     58
## 177            Black  183.0         DC Comics       white       bad     91
## 178             <NA>  183.0      George Lucas        <NA>       bad     NA
## 179     Black / Blue  180.0         DC Comics        <NA>       bad     77
## 180          No Hair     NA         DC Comics       green       bad     NA
## 181          No Hair  196.0     Marvel Comics      yellow       bad     47
## 182             <NA>     NA      NBC - Heroes        <NA>       bad     NA
## 183             <NA>  183.0 Dark Horse Comics      silver       bad    146
## 184             <NA>     NA Dark Horse Comics        <NA>       bad    176
## 185             <NA>     NA Dark Horse Comics        <NA>       bad    198
## 186             <NA>     NA Dark Horse Comics      silver       bad    149
## 187            Brown  188.0     Marvel Comics        <NA>       bad     99
## 188          No Hair  201.0     Marvel Comics      purple       bad    443
## 189          No Hair  185.0     Marvel Comics        grey       bad    203
## 190            White  163.0     Marvel Comics        <NA>       bad     54
## 191            Black     NA         DC Comics         red       bad     NA
## 192             <NA>  183.0         DC Comics        <NA>       bad     82
## 193             <NA>  206.0     Marvel Comics      silver       bad    331
## 194            White   15.2     Marvel Comics        <NA>       bad     58
## 195          No Hair  165.0     Marvel Comics        <NA>       bad     79
## 196            Black  168.0          Shueisha        <NA>       bad     73
## 197 Strawberry Blond  191.0     Marvel Comics        <NA>       bad    117
## 198            Black  175.0     Marvel Comics        <NA>       bad     50
## 199            Brown  229.0     Marvel Comics        <NA>       bad    334
## 200             <NA>     NA      Image Comics        <NA>       bad     NA
## 201          No Hair  180.0     Marvel Comics        <NA>       bad     79
## 202            Black  183.0     Marvel Comics        <NA>       bad    162
## 203            Black  173.0         DC Comics        <NA>       bad     67
## 204             <NA>     NA     Marvel Comics        <NA>       bad     NA
## 205            Black     NA         DC Comics        <NA>       bad     NA
## 206          No Hair  188.0     Marvel Comics        <NA>       bad     95
## 207            Brown  185.0         DC Comics        <NA>       bad     81
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good_guys, Race)
```

```
##               Race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
filter(good_guys,Race=='Asgardian')
```

```
##        Name Gender Eye.color      Race Hair.color Height     Publisher
## 1       Sif Female      blue Asgardian      Black    188 Marvel Comics
## 2      Thor   Male      blue Asgardian      Blond    198 Marvel Comics
## 3 Thor Girl Female      blue Asgardian      Blond    175 Marvel Comics
##   Skin.color Alignment Weight
## 1       <NA>      good    191
## 2       <NA>      good    288
## 3       <NA>      good    143
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
filter(bad_guys, Gender == "Male" & Height > 200)
```

```
##              Name Gender Eye.color              Race Hair.color Height
## 1     Abomination   Male     green Human / Radiation    No Hair    203
## 2           Alien   Male      <NA>   Xenomorph XX121    No Hair    244
## 3           Amazo   Male       red           Android       <NA>    257
## 4      Apocalypse   Male       red            Mutant      Black    213
## 5            Bane   Male      <NA>             Human       <NA>    203
## 6        Darkseid   Male       red           New God    No Hair    267
## 7     Doctor Doom   Male     brown             Human      Brown    201
## 8  Doctor Doom II   Male     brown              <NA>      Brown    201
## 9        Doomsday   Male       red             Alien      White    244
## 10    Killer Croc   Male       red         Metahuman    No Hair    244
## 11        Kingpin   Male      blue             Human    No Hair    201
## 12         Lizard   Male       red             Human    No Hair    203
## 13          MODOK   Male     white            Cyborg     Brownn    366
## 14      Omega Red   Male       red              <NA>      Blond    211
## 15      Onslaught   Male       red            Mutant    No Hair    305
## 16       Predator   Male      <NA>            Yautja       <NA>    213
## 17         Sauron   Male      <NA>             Maiar       <NA>    279
## 18       Scorpion   Male     brown             Human      Brown    211
## 19 Solomon Grundy   Male     black            Zombie      White    279
## 20         Thanos   Male       red           Eternal    No Hair    201
## 21         Ultron   Male       red           Android       <NA>    206
## 22      Venom III   Male     brown          Symbiote      Brown    229
##            Publisher Skin.color Alignment Weight
## 1      Marvel Comics       <NA>       bad    441
## 2  Dark Horse Comics      black       bad    169
## 3          DC Comics       <NA>       bad    173
## 4      Marvel Comics       grey       bad    135
## 5          DC Comics       <NA>       bad    180
## 6          DC Comics       grey       bad    817
## 7      Marvel Comics       <NA>       bad    187
## 8      Marvel Comics       <NA>       bad    132
## 9          DC Comics       <NA>       bad    412
## 10         DC Comics      green       bad    356
## 11     Marvel Comics       <NA>       bad    203
## 12     Marvel Comics       <NA>       bad    230
## 13     Marvel Comics       <NA>       bad    338
## 14     Marvel Comics       <NA>       bad    191
## 15     Marvel Comics       <NA>       bad    405
## 16 Dark Horse Comics       <NA>       bad    234
## 17  J. R. R. Tolkien       <NA>       bad     NA
## 18     Marvel Comics       <NA>       bad    310
## 19         DC Comics       <NA>       bad    437
## 20     Marvel Comics     purple       bad    443
## 21     Marvel Comics     silver       bad    331
## 22     Marvel Comics       <NA>       bad    334
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?
There are 37 good guys with no hair, but 35 bad guys with no hair, so good guys wins.:)

```r
tabyl(good_guys, Hair.color)
```

```
##        Hair.color   n     percent valid_percent
##            Auburn  10 0.020161290   0.026178010
##             black   3 0.006048387   0.007853403
##             Black 108 0.217741935   0.282722513
##             blond   2 0.004032258   0.005235602
##             Blond  85 0.171370968   0.222513089
##              Blue   1 0.002016129   0.002617801
##             Brown  55 0.110887097   0.143979058
##     Brown / Black   1 0.002016129   0.002617801
##     Brown / White   4 0.008064516   0.010471204
##             Green   7 0.014112903   0.018324607
##              Grey   2 0.004032258   0.005235602
##            Indigo   1 0.002016129   0.002617801
##           Magenta   1 0.002016129   0.002617801
##           No Hair  37 0.074596774   0.096858639
##            Orange   2 0.004032258   0.005235602
##    Orange / White   1 0.002016129   0.002617801
##              Pink   1 0.002016129   0.002617801
##            Purple   1 0.002016129   0.002617801
##               Red  40 0.080645161   0.104712042
##       Red / White   1 0.002016129   0.002617801
##            Silver   3 0.006048387   0.007853403
##  Strawberry Blond   4 0.008064516   0.010471204
##             White  10 0.020161290   0.026178010
##            Yellow   2 0.004032258   0.005235602
##              <NA> 114 0.229838710            NA
```

```r
tabyl(bad_guys, Hair.color)
```

```
##        Hair.color  n     percent valid_percent
##            Auburn  3 0.014492754   0.019480519
##             Black 42 0.202898551   0.272727273
##      Black / Blue  1 0.004830918   0.006493506
##             blond  1 0.004830918   0.006493506
##             Blond 11 0.053140097   0.071428571
##              Blue  1 0.004830918   0.006493506
##             Brown 27 0.130434783   0.175324675
##            Brownn  1 0.004830918   0.006493506
##              Gold  1 0.004830918   0.006493506
##             Green  1 0.004830918   0.006493506
##              Grey  3 0.014492754   0.019480519
##           No Hair 35 0.169082126   0.227272727
##            Purple  3 0.014492754   0.019480519
##               Red  9 0.043478261   0.058441558
##        Red / Grey  1 0.004830918   0.006493506
##      Red / Orange  1 0.004830918   0.006493506
##  Strawberry Blond  3 0.014492754   0.019480519
##             White 10 0.048309179   0.064935065
##              <NA> 53 0.256038647            NA
```

10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight greater than or equal to 450?

```r
filter(superhero_info, Height > 200 | Weight >= 450)
```

```
## # A tibble: 60 × 10
##    Name    Gender `Eye color` Race   `Hair color` Height Publisher  `Skin color`
##    <chr>   <chr>  <chr>       <chr>  <chr>         <dbl> <chr>      <chr>       
##  1 A-Bomb  Male   yellow      Human  No Hair         203 Marvel Co… <NA>        
##  2 Abomin… Male   green       Human… No Hair         203 Marvel Co… <NA>        
##  3 Alien   Male   <NA>        Xenom… No Hair         244 Dark Hors… black       
##  4 Amazo   Male   red         Andro… <NA>            257 DC Comics  <NA>        
##  5 Ant-Man Male   blue        Human  Blond           211 Marvel Co… <NA>        
##  6 Anti-V… Male   blue        Symbi… Blond           229 Marvel Co… <NA>        
##  7 Apocal… Male   red         Mutant Black           213 Marvel Co… grey        
##  8 Bane    Male   <NA>        Human  <NA>            203 DC Comics  <NA>        
##  9 Beta R… Male   <NA>        <NA>   No Hair         201 Marvel Co… <NA>        
## 10 Blooda… Female blue        Human  Brown           218 Marvel Co… <NA>        
## # … with 50 more rows, and 2 more variables: Alignment <chr>, Weight <dbl>
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
filter(superhero_info,Height>300)
```

```
## # A tibble: 8 × 10
##   Name    Gender `Eye color` Race     `Hair color` Height Publisher `Skin color`
##   <chr>   <chr>  <chr>       <chr>    <chr>         <dbl> <chr>     <chr>       
## 1 Fin Fa… Male   red         Kakaran… No Hair        975  Marvel C… green       
## 2 Galact… Male   black       Cosmic … Black          876  Marvel C… <NA>        
## 3 Groot   Male   yellow      Flora C… <NA>           701  Marvel C… <NA>        
## 4 MODOK   Male   white       Cyborg   Brownn         366  Marvel C… <NA>        
## 5 Onslau… Male   red         Mutant   No Hair        305  Marvel C… <NA>        
## 6 Sasqua… Male   red         <NA>     Orange         305  Marvel C… <NA>        
## 7 Wolfsb… Female green       <NA>     Auburn         366  Marvel C… <NA>        
## 8 Ymir    Male   white       Frost G… No Hair        305. Marvel C… white       
## # … with 2 more variables: Alignment <chr>, Weight <dbl>
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?
This is because the question asked hero with height greater than 200 but not 300.

```r
filter(superhero_info,Weight>450)
```

```
## # A tibble: 8 × 10
##   Name       Gender `Eye color` Race  `Hair color` Height Publisher `Skin color`
##   <chr>      <chr>  <chr>       <chr> <chr>         <dbl> <chr>     <chr>       
## 1 Bloodaxe   Female blue        Human Brown         218   Marvel C… <NA>        
## 2 Darkseid   Male   red         New … No Hair       267   DC Comics grey        
## 3 Giganta    Female green       <NA>  Red            62.5 DC Comics <NA>        
## 4 Hulk       Male   green       Huma… Green         244   Marvel C… green       
## 5 Juggernaut Male   blue        Human Red           287   Marvel C… <NA>        
## 6 Red Hulk   Male   yellow      Huma… Black         213   Marvel C… red         
## 7 Sasquatch  Male   red         <NA>  Orange        305   Marvel C… <NA>        
## 8 Wolfsbane  Female green       <NA>  Auburn        366   Marvel C… <NA>        
## # … with 2 more variables: Alignment <chr>, Weight <dbl>
```

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

```r
height_to_weight <- (superhero_info$Height/superhero_info$Weight)
superhero_info <- cbind(superhero_info,height_to_weight)
superhero_info
```

```
##                          Name Gender               Eye color               Race
## 1                      A-Bomb   Male                  yellow              Human
## 2                  Abe Sapien   Male                    blue      Icthyo Sapien
## 3                    Abin Sur   Male                    blue            Ungaran
## 4                 Abomination   Male                   green  Human / Radiation
## 5                     Abraxas   Male                    blue      Cosmic Entity
## 6               Absorbing Man   Male                    blue              Human
## 7                 Adam Monroe   Male                    blue               <NA>
## 8                Adam Strange   Male                    blue              Human
## 9                    Agent 13 Female                    blue               <NA>
## 10                  Agent Bob   Male                   brown              Human
## 11                 Agent Zero   Male                    <NA>               <NA>
## 12                 Air-Walker   Male                    blue               <NA>
## 13                       Ajax   Male                   brown             Cyborg
## 14                 Alan Scott   Male                    blue               <NA>
## 15                Alex Mercer   Male                    <NA>              Human
## 16               Alex Woolsly   Male                    <NA>               <NA>
## 17          Alfred Pennyworth   Male                    blue              Human
## 18                      Alien   Male                    <NA>    Xenomorph XX121
## 19           Allan Quatermain   Male                    <NA>               <NA>
## 20                      Amazo   Male                     red            Android
## 21                       Ammo   Male                   brown              Human
## 22             Ando Masahashi   Male                    <NA>               <NA>
## 23                      Angel   Male                    blue               <NA>
## 24                      Angel   Male                    <NA>            Vampire
## 25                 Angel Dust Female                  yellow             Mutant
## 26            Angel Salvadore Female                   brown               <NA>
## 27                     Angela Female                    <NA>               <NA>
## 28                 Animal Man   Male                    blue              Human
## 29                  Annihilus   Male                   green               <NA>
## 30                    Ant-Man   Male                    blue              Human
## 31                 Ant-Man II   Male                    blue              Human
## 32               Anti-Monitor   Male                  yellow      God / Eternal
## 33                 Anti-Spawn   Male                    <NA>               <NA>
## 34                 Anti-Venom   Male                    blue           Symbiote
## 35                 Apocalypse   Male                     red             Mutant
## 36                   Aquababy   Male                    blue               <NA>
## 37                    Aqualad   Male                    blue          Atlantean
## 38                    Aquaman   Male                    blue          Atlantean
## 39                    Arachne Female                    blue              Human
## 40                  Archangel   Male                    blue             Mutant
## 41                   Arclight Female                  violet               <NA>
## 42                     Ardina Female                   white              Alien
## 43                       Ares   Male                   brown               <NA>
## 44                      Ariel Female                  purple               <NA>
## 45                      Armor Female                   black               <NA>
## 46                    Arsenal   Male                    <NA>              Human
## 47                  Astro Boy   Male                   brown               <NA>
## 48                      Atlas   Male                   brown             Mutant
## 49                      Atlas   Male                    blue      God / Eternal
## 50                       Atom   Male                    blue               <NA>
## 51                       Atom   Male                    <NA>               <NA>
## 52                  Atom Girl Female                   black               <NA>
## 53                    Atom II   Male                   brown              Human
## 54                   Atom III   Male                    <NA>               <NA>
## 55                    Atom IV   Male                   brown               <NA>
## 56                     Aurora Female                    blue             Mutant
## 57                     Azazel   Male                  yellow           Neyaphem
## 58                     Azrael   Male                   brown              Human
## 59                      Aztar   Male                    <NA>               <NA>
## 60                       Bane   Male                    <NA>              Human
## 61                    Banshee   Male                   green              Human
## 62                     Bantam   Male                   brown               <NA>
## 63                    Batgirl Female                    <NA>               <NA>
## 64                    Batgirl Female                   green              Human
## 65                Batgirl III Female                    <NA>               <NA>
## 66                 Batgirl IV Female                   green              Human
## 67                  Batgirl V Female                    <NA>               <NA>
## 68                 Batgirl VI Female                    blue               <NA>
## 69                     Batman   Male                    blue              Human
## 70                     Batman   Male                    blue              Human
## 71                  Batman II   Male                    blue              Human
## 72                 Battlestar   Male                   brown               <NA>
## 73                 Batwoman V Female                   green              Human
## 74                       Beak   Male                   black               <NA>
## 75                      Beast   Male                    blue             Mutant
## 76                  Beast Boy   Male                   green              Human
## 77                     Beetle   Male                    <NA>               <NA>
## 78                     Ben 10   Male                    <NA>               <NA>
## 79              Beta Ray Bill   Male                    <NA>               <NA>
## 80                   Beyonder   Male                    <NA>      God / Eternal
## 81                  Big Barda Female                    blue            New God
## 82                  Big Daddy   Male                    <NA>               <NA>
## 83                    Big Man   Male                    blue               <NA>
## 84                Bill Harken   Male                    <NA>              Alpha
## 85              Billy Kincaid   Male                    <NA>               <NA>
## 86                     Binary Female                    blue               <NA>
## 87               Bionic Woman Female                    blue             Cyborg
## 88                 Bird-Brain   <NA>                    <NA>               <NA>
## 89                   Bird-Man   Male                    <NA>              Human
## 90                Bird-Man II   Male                    <NA>              Human
## 91                    Birdman   Male                    <NA>      God / Eternal
## 92                     Bishop   Male                   brown             Mutant
## 93                    Bizarro   Male                   black            Bizarro
## 94               Black Abbott   Male                     red               <NA>
## 95                 Black Adam   Male                   brown               <NA>
## 96                 Black Bolt   Male                    blue            Inhuman
## 97               Black Canary Female                    blue              Human
## 98               Black Canary Female                    blue          Metahuman
## 99                  Black Cat Female                   green              Human
## 100               Black Flash   Male                    <NA>      God / Eternal
## 101             Black Goliath   Male                    <NA>               <NA>
## 102          Black Knight III   Male                   brown              Human
## 103           Black Lightning   Male                   brown               <NA>
## 104               Black Mamba Female                   green               <NA>
## 105               Black Manta   Male                   black              Human
## 106             Black Panther   Male                   brown              Human
## 107               Black Widow Female                   green              Human
## 108            Black Widow II Female                    blue               <NA>
## 109                  Blackout   Male                     red              Demon
## 110                 Blackwing   Male                    blue               <NA>
## 111                 Blackwulf   Male                     red              Alien
## 112                     Blade   Male                   brown            Vampire
## 113               Blaquesmith   <NA>                   black               <NA>
## 114                    Bling! Female                    <NA>               <NA>
## 115                     Blink Female                   green             Mutant
## 116                  Blizzard   Male                    <NA>               <NA>
## 117                  Blizzard   Male                    <NA>               <NA>
## 118               Blizzard II   Male                   brown               <NA>
## 119                      Blob   Male                   brown               <NA>
## 120                  Bloodaxe Female                    blue              Human
## 121                 Bloodhawk   Male                   black             Mutant
## 122               Bloodwraith   Male                   white               <NA>
## 123               Blue Beetle   Male                    blue               <NA>
## 124               Blue Beetle   Male                    <NA>               <NA>
## 125            Blue Beetle II   Male                    blue               <NA>
## 126           Blue Beetle III   Male                   brown              Human
## 127                 Boba Fett   Male                   brown      Human / Clone
## 128                      Bolt   Male                    <NA>               <NA>
## 129                Bomb Queen Female                    <NA>               <NA>
## 130                 Boom-Boom Female                    blue             Mutant
## 131                    Boomer Female                    <NA>               <NA>
## 132              Booster Gold   Male                    blue              Human
## 133                       Box   Male                    <NA>               <NA>
## 134                   Box III   <NA>                    blue               <NA>
## 135                    Box IV   <NA>                   brown               <NA>
## 136                  Brainiac   Male                   green            Android
## 137                Brainiac 5   Male                   green               <NA>
## 138            Brother Voodoo   Male                   brown              Human
## 139                Brundlefly   Male                    <NA>             Mutant
## 140                     Buffy Female                   green              Human
## 141                  Bullseye   Male                    blue              Human
## 142                 Bumblebee Female                   brown              Human
## 143                 Bumbleboy   Male                    <NA>               <NA>
## 144                   Bushido   Male                    <NA>              Human
## 145                     Cable   Male                    blue             Mutant
## 146                  Callisto Female                    blue               <NA>
## 147             Cameron Hicks   Male                    <NA>              Alpha
## 148                Cannonball   Male                    blue               <NA>
## 149           Captain America   Male                    blue              Human
## 150              Captain Atom   Male                    blue  Human / Radiation
## 151           Captain Britain   Male                    blue              Human
## 152              Captain Cold   Male                   brown              Human
## 153              Captain Epic   Male                    blue               <NA>
## 154         Captain Hindsight   Male                    <NA>              Human
## 155          Captain Mar-vell   Male                    blue               <NA>
## 156            Captain Marvel Female                    blue         Human-Kree
## 157            Captain Marvel   Male                    blue              Human
## 158         Captain Marvel II   Male                    blue              Human
## 159          Captain Midnight   Male                    <NA>              Human
## 160            Captain Planet   Male                     red      God / Eternal
## 161          Captain Universe   <NA>                    <NA>      God / Eternal
## 162                   Carnage   Male                   green           Symbiote
## 163                       Cat Female                    blue               <NA>
## 164                    Cat II Female                    <NA>               <NA>
## 165                  Catwoman Female                   green              Human
## 166             Cecilia Reyes   <NA>                   brown               <NA>
## 167                   Century   Male                   white              Alien
## 168                   Cerebra Female                    <NA>             Mutant
## 169                   Chamber   Male                   brown             Mutant
## 170                 Chameleon   Male                    <NA>               <NA>
## 171                Changeling   Male                   brown               <NA>
## 172                   Cheetah Female                   green              Human
## 173                Cheetah II Female                   green              Human
## 174               Cheetah III Female                   brown              Human
## 175                   Chromos   Male                   brown               <NA>
## 176              Chuck Norris   Male                    <NA>               <NA>
## 177             Citizen Steel   Male                   green              Human
## 178             Claire Bennet Female                    blue               <NA>
## 179                      Clea   <NA>                    <NA>               <NA>
## 180                     Cloak   Male                   brown               <NA>
## 181                Clock King   Male                    blue              Human
## 182                Cogliostro   Male                    <NA>               <NA>
## 183              Colin Wagner   Male                    grey               <NA>
## 184              Colossal Boy   Male                    <NA>               <NA>
## 185                  Colossus   Male                  silver             Mutant
## 186                   Copycat Female                     red             Mutant
## 187                   Corsair   Male                   brown               <NA>
## 188               Cottonmouth   Male                   brown              Human
## 189          Crimson Crusader   Male                    blue               <NA>
## 190            Crimson Dynamo   Male                   brown               <NA>
## 191                   Crystal Female                   green            Inhuman
## 192                     Curse   Male                    <NA>               <NA>
## 193                    Cy-Gor   Male                    <NA>               <NA>
## 194                    Cyborg   Male                   brown             Cyborg
## 195           Cyborg Superman   Male                    blue             Cyborg
## 196                   Cyclops   Male                   brown             Mutant
## 197                    Cypher   <NA>                    blue               <NA>
## 198                    Dagger Female                    blue               <NA>
## 199              Danny Cooper   Male                   brown               <NA>
## 200             Daphne Powell Female                    <NA>               <NA>
## 201                 Daredevil   Male                    blue              Human
## 202                  Darkhawk   Male                   brown              Human
## 203                   Darkman   Male                    <NA>               <NA>
## 204                  Darkseid   Male                     red            New God
## 205                  Darkside   <NA>                    <NA>               <NA>
## 206                  Darkstar Female                   brown             Mutant
## 207                Darth Maul   Male            yellow / red Dathomirian Zabrak
## 208               Darth Vader   Male                  yellow             Cyborg
## 209                      Dash   Male                    blue              Human
## 210                      Data   Male                  yellow            Android
## 211                   Dazzler Female                    blue             Mutant
## 212                   Deadman   Male                    blue              Human
## 213                  Deadpool   Male                   brown             Mutant
## 214                  Deadshot   Male                   brown              Human
## 215                  Deathlok   Male                   brown             Cyborg
## 216               Deathstroke   Male                    blue              Human
## 217                Demogoblin   Male                     red              Demon
## 218                 Destroyer   Male                    <NA>               <NA>
## 219               Diamondback   Male                   brown              Human
## 220                DL Hawkins   Male                    <NA>               <NA>
## 221                Doc Samson   Male                    blue  Human / Radiation
## 222               Doctor Doom   Male                   brown              Human
## 223            Doctor Doom II   Male                   brown               <NA>
## 224               Doctor Fate   Male                    blue              Human
## 225            Doctor Octopus   Male                   brown              Human
## 226            Doctor Strange   Male                    grey              Human
## 227                    Domino Female                    blue              Human
## 228                 Donatello   Male                   green             Mutant
## 229                Donna Troy Female                    blue             Amazon
## 230                  Doomsday   Male                     red              Alien
## 231              Doppelganger   Male                   white               <NA>
## 232                  Dormammu   Male                  yellow               <NA>
## 233              Dr Manhattan   Male                   white     Human / Cosmic
## 234        Drax the Destroyer   Male                     red    Human / Altered
## 235                       Ego   <NA>                    <NA>               <NA>
## 236                Elastigirl Female                   brown              Human
## 237                   Electro   Male                    blue              Human
## 238                   Elektra Female                    blue              Human
## 239               Elle Bishop Female                    blue               <NA>
## 240             Elongated Man   Male                    blue               <NA>
## 241                Emma Frost Female                    blue               <NA>
## 242               Enchantress Female                    blue              Human
## 243                    Energy Female                    <NA>               <NA>
## 244                     ERG-1   Male                    <NA>               <NA>
## 245                Ethan Hunt   Male                   brown              Human
## 246                   Etrigan   Male                     red              Demon
## 247             Evil Deadpool   Male                   white             Mutant
## 248                  Evilhawk   Male                     red              Alien
## 249                    Exodus   Male                    blue             Mutant
## 250             Fabian Cortez   <NA>                    blue               <NA>
## 251                    Falcon   Male                   brown              Human
## 252             Fallen One II   Male                   black               <NA>
## 253                     Faora Female                    <NA>         Kryptonian
## 254                     Feral   <NA> yellow (without irises)               <NA>
## 255           Fighting Spirit Female                    <NA>               <NA>
## 256             Fin Fang Foom   Male                     red    Kakarantharaian
## 257                  Firebird Female                   brown               <NA>
## 258                  Firelord   <NA>                   white               <NA>
## 259                  Firestar Female                   green             Mutant
## 260                 Firestorm   Male                   brown               <NA>
## 261                 Firestorm   Male                    blue              Human
## 262                     Fixer   <NA>                     red               <NA>
## 263                     Flash   Male                    blue              Human
## 264              Flash Gordon   Male                    <NA>               <NA>
## 265                  Flash II   Male                    blue              Human
## 266                 Flash III   Male                    <NA>              Human
## 267                  Flash IV   Male                  yellow              Human
## 268                     Forge   <NA>                   brown               <NA>
## 269         Franklin Richards   Male                    blue             Mutant
## 270            Franklin Storm   <NA>                    blue               <NA>
## 271                    Frenzy Female                   brown               <NA>
## 272                    Frigga Female                    blue               <NA>
## 273                  Galactus   Male                   black      Cosmic Entity
## 274                    Gambit   Male                     red             Mutant
## 275                    Gamora Female                  yellow      Zen-Whoberian
## 276               Garbage Man   Male                    <NA>             Mutant
## 277                 Gary Bell   Male                    <NA>              Alpha
## 278               General Zod   Male                   black         Kryptonian
## 279                   Genesis   Male                    blue               <NA>
## 280               Ghost Rider   Male                     red              Demon
## 281            Ghost Rider II   <NA>                    <NA>               <NA>
## 282                 Giant-Man   Male                    <NA>              Human
## 283              Giant-Man II   Male                    <NA>               <NA>
## 284                   Giganta Female                   green               <NA>
## 285                 Gladiator   Male                    blue          Strontian
## 286              Goblin Queen Female                   green               <NA>
## 287                  Godzilla   <NA>                    <NA>              Kaiju
## 288                       Gog   Male                    <NA>               <NA>
## 289                      Goku   Male                    <NA>             Saiyan
## 290                   Goliath   Male                    <NA>               <NA>
## 291                   Goliath   Male                    <NA>              Human
## 292                   Goliath   Male                    <NA>              Human
## 293                Goliath IV   Male                   brown               <NA>
## 294             Gorilla Grodd   Male                  yellow            Gorilla
## 295           Granny Goodness Female                    blue               <NA>
## 296                   Gravity   Male                    blue              Human
## 297                    Greedo   Male                  purple             Rodian
## 298               Green Arrow   Male                   green              Human
## 299              Green Goblin   Male                    blue              Human
## 300           Green Goblin II   Male                    blue               <NA>
## 301          Green Goblin III   Male                    <NA>               <NA>
## 302           Green Goblin IV   Male                   green               <NA>
## 303                     Groot   Male                  yellow     Flora Colossus
## 304                  Guardian   Male                   brown              Human
## 305               Guy Gardner   Male                    blue    Human-Vuldarian
## 306                Hal Jordan   Male                   brown              Human
## 307                  Han Solo   Male                   brown              Human
## 308                   Hancock   Male                   brown              Human
## 309              Harley Quinn Female                    blue              Human
## 310              Harry Potter   Male                   green              Human
## 311                     Havok   Male                    blue             Mutant
## 312                      Hawk   Male                     red               <NA>
## 313                   Hawkeye   Male                    blue              Human
## 314                Hawkeye II Female                    blue              Human
## 315                  Hawkgirl Female                   green               <NA>
## 316                   Hawkman   Male                    blue               <NA>
## 317                 Hawkwoman Female                   green               <NA>
## 318              Hawkwoman II Female                    <NA>               <NA>
## 319             Hawkwoman III Female                    blue               <NA>
## 320                 Heat Wave   Male                    blue              Human
## 321                      Hela Female                   green          Asgardian
## 322                   Hellboy   Male                    gold              Demon
## 323                   Hellcat Female                    blue              Human
## 324                 Hellstorm   Male                     red               <NA>
## 325                  Hercules   Male                    blue           Demi-God
## 326             Hiro Nakamura   Male                    <NA>               <NA>
## 327                  Hit-Girl Female                    <NA>              Human
## 328                 Hobgoblin   Male                    blue               <NA>
## 329                    Hollow Female                    blue               <NA>
## 330              Hope Summers Female                   green               <NA>
## 331           Howard the Duck   Male                   brown               <NA>
## 332                      Hulk   Male                   green  Human / Radiation
## 333               Human Torch   Male                    blue  Human / Radiation
## 334                  Huntress Female                    blue               <NA>
## 335                      Husk Female                    blue             Mutant
## 336                    Hybrid   Male                   brown           Symbiote
## 337                 Hydro-Man   Male                   brown               <NA>
## 338                  Hyperion   Male                    blue            Eternal
## 339                    Iceman   Male                   brown             Mutant
## 340                   Impulse   Male                  yellow              Human
## 341             Indiana Jones   Male                    <NA>              Human
## 342                    Indigo Female                    <NA>              Alien
## 343                       Ink   Male                    blue             Mutant
## 344           Invisible Woman Female                    blue  Human / Radiation
## 345                 Iron Fist   Male                    blue              Human
## 346                  Iron Man   Male                    blue              Human
## 347               Iron Monger   Male                    blue               <NA>
## 348                      Isis Female                    <NA>               <NA>
## 349                Jack Bauer   Male                    <NA>               <NA>
## 350            Jack of Hearts   Male            blue / white              Human
## 351                 Jack-Jack   Male                    blue              Human
## 352                James Bond   Male                    blue              Human
## 353             James T. Kirk   Male                   hazel              Human
## 354             Jar Jar Binks   Male                  yellow             Gungan
## 355              Jason Bourne   Male                    <NA>              Human
## 356                 Jean Grey Female                   green             Mutant
## 357           Jean-Luc Picard   Male                    <NA>              Human
## 358             Jennifer Kale Female                    blue               <NA>
## 359               Jesse Quick Female                    <NA>              Human
## 360              Jessica Cruz Female                   green              Human
## 361             Jessica Jones Female                   brown              Human
## 362           Jessica Sanders Female                    <NA>               <NA>
## 363                    Jigsaw   Male                    blue               <NA>
## 364                Jim Powell   Male                    <NA>               <NA>
## 365                 JJ Powell   Male                    <NA>               <NA>
## 366             Johann Krauss   Male                    <NA>               <NA>
## 367          John Constantine   Male                    blue              Human
## 368              John Stewart   Male                   green              Human
## 369               John Wraith   Male                   brown               <NA>
## 370                     Joker   Male                   green              Human
## 371                      Jolt Female                    blue               <NA>
## 372                   Jubilee Female                     red             Mutant
## 373               Judge Dredd   Male                    <NA>              Human
## 374                Juggernaut   Male                    blue              Human
## 375                  Junkpile   Male                    <NA>             Mutant
## 376                   Justice   Male                   hazel              Human
## 377                  Jyn Erso Female                   green              Human
## 378                     K-2SO   Male                   white            Android
## 379                      Kang   Male                   brown               <NA>
## 380                Karate Kid   Male                   brown              Human
## 381           Kathryn Janeway Female                    <NA>              Human
## 382          Katniss Everdeen Female                    <NA>              Human
## 383                  Kevin 11   Male                    <NA>              Human
## 384                  Kick-Ass   Male                    blue              Human
## 385                 Kid Flash   Male                   green              Human
## 386              Kid Flash II   Male                    <NA>               <NA>
## 387               Killer Croc   Male                     red          Metahuman
## 388              Killer Frost Female                    blue              Human
## 389                   Kilowog   Male                     red         Bolovaxian
## 390                 King Kong   Male                  yellow             Animal
## 391                King Shark   Male                   black             Animal
## 392                   Kingpin   Male                    blue              Human
## 393                      Klaw   Male                     red              Human
## 394              Kool-Aid Man   Male                   black               <NA>
## 395                 Kraven II   Male                   brown              Human
## 396         Kraven the Hunter   Male                   brown              Human
## 397                    Krypto   Male                    blue         Kryptonian
## 398               Kyle Rayner   Male                   green              Human
## 399                  Kylo Ren   Male                    <NA>              Human
## 400             Lady Bullseye Female                    <NA>               <NA>
## 401          Lady Deathstrike Female                   brown             Cyborg
## 402                    Leader   Male                   green               <NA>
## 403                     Leech   Male                    <NA>               <NA>
## 404                    Legion   Male            green / blue             Mutant
## 405                  Leonardo   Male                    blue             Mutant
## 406                Lex Luthor   Male                   green              Human
## 407                Light Lass Female                    blue               <NA>
## 408             Lightning Lad   Male                    blue               <NA>
## 409            Lightning Lord   Male                    blue               <NA>
## 410              Living Brain   <NA>                  yellow               <NA>
## 411           Living Tribunal   <NA>                    blue      Cosmic Entity
## 412               Liz Sherman Female                    <NA>               <NA>
## 413                    Lizard   Male                     red              Human
## 414                      Lobo   Male                     red           Czarnian
## 415                      Loki   Male                   green          Asgardian
## 416                  Longshot   Male                    blue              Human
## 417                 Luke Cage   Male                   brown              Human
## 418             Luke Campbell   Male                    <NA>               <NA>
## 419            Luke Skywalker   Male                    blue              Human
## 420                      Luna Female                    <NA>              Human
## 421                      Lyja Female                   green               <NA>
## 422                   Mach-IV   Male                   brown               <NA>
## 423               Machine Man   <NA>                     red               <NA>
## 424                   Magneto   Male                    grey             Mutant
## 425                     Magog   Male                    blue               <NA>
## 426                     Magus   Male                   black               <NA>
## 427           Man of Miracles   <NA>                    blue      God / Eternal
## 428                   Man-Bat   Male                   brown              Human
## 429                 Man-Thing   Male                     red               <NA>
## 430                  Man-Wolf   Male                   brown               <NA>
## 431                  Mandarin   Male                    blue              Human
## 432                    Mantis Female                   green         Human-Kree
## 433         Martian Manhunter   Male                     red            Martian
## 434               Marvel Girl Female                   green               <NA>
## 435              Master Brood   Male                    blue               <NA>
## 436              Master Chief   Male                   brown    Human / Altered
## 437                     Match   Male                   black               <NA>
## 438              Matt Parkman   Male                    <NA>               <NA>
## 439                  Maverick   Male                    blue               <NA>
## 440                    Maxima Female                   brown               <NA>
## 441              Maya Herrera Female                    <NA>               <NA>
## 442                    Medusa Female                   green            Inhuman
## 443                  Meltdown Female                    blue               <NA>
## 444                  Mephisto   Male                   white               <NA>
## 445                      Mera Female                    blue          Atlantean
## 446                   Metallo   Male                   green            Android
## 447                Metamorpho   Male                   black               <NA>
## 448                 Meteorite Female                    <NA>               <NA>
## 449                    Metron   Male                    blue               <NA>
## 450             Micah Sanders   Male                   brown               <NA>
## 451              Michelangelo   Male                    blue             Mutant
## 452                 Micro Lad   Male                    grey               <NA>
## 453                     Mimic   Male                   brown               <NA>
## 454              Minna Murray Female                    <NA>               <NA>
## 455                    Misfit Female                    blue               <NA>
## 456              Miss Martian Female                     red               <NA>
## 457          Mister Fantastic   Male                   brown  Human / Radiation
## 458             Mister Freeze   Male                    <NA>              Human
## 459              Mister Knife   Male                    blue            Spartoi
## 460           Mister Mxyzptlk   Male                    <NA>      God / Eternal
## 461           Mister Sinister   Male                     red    Human / Altered
## 462              Mister Zsasz   Male                    blue              Human
## 463               Mockingbird Female                    blue              Human
## 464                     MODOK   Male                   white             Cyborg
## 465                      Mogo   Male                    <NA>             Planet
## 466           Mohinder Suresh   Male                    <NA>               <NA>
## 467                    Moloch   Male                    <NA>               <NA>
## 468                Molten Man   Male                    gold               <NA>
## 469                   Monarch   Male                    blue               <NA>
## 470             Monica Dawson Female                    <NA>               <NA>
## 471               Moon Knight   Male                   brown              Human
## 472                 Moonstone Female                    blue               <NA>
## 473                    Morlun   Male             white / red               <NA>
## 474                     Morph   Male                   white               <NA>
## 475              Moses Magnum   Male                   brown               <NA>
## 476               Mr Immortal   Male                    blue             Mutant
## 477             Mr Incredible   Male                    blue              Human
## 478              Ms Marvel II Female                    blue               <NA>
## 479              Multiple Man   Male                    blue               <NA>
## 480                  Mysterio   Male                   brown              Human
## 481                  Mystique Female yellow (without irises)             Mutant
## 482                     Namor   Male                    <NA>               <NA>
## 483                     Namor   Male                    grey          Atlantean
## 484                    Namora Female                    blue               <NA>
## 485                  Namorita Female                    blue               <NA>
## 486            Naruto Uzumaki   Male                    <NA>              Human
## 487           Nathan Petrelli   Male                   brown               <NA>
## 488                    Nebula Female                    blue          Luphomoid
## 489 Negasonic Teenage Warhead Female                   black             Mutant
## 490                 Nick Fury   Male                   brown              Human
## 491              Nightcrawler   Male                  yellow               <NA>
## 492                 Nightwing   Male                    blue              Human
## 493              Niki Sanders Female                    blue               <NA>
## 494              Nina Theroux Female                    <NA>              Alpha
## 495               Nite Owl II   Male                    <NA>               <NA>
## 496                 Northstar   Male                    blue               <NA>
## 497                      Nova   Male                   brown              Human
## 498                      Nova Female                   white     Human / Cosmic
## 499                      Odin   Male                    blue      God / Eternal
## 500                 Offspring   Male                    <NA>               <NA>
## 501                 Omega Red   Male                     red               <NA>
## 502                Omniscient   Male                   brown               <NA>
## 503             One Punch Man   Male                    <NA>              Human
## 504             One-Above-All   <NA>                    <NA>      Cosmic Entity
## 505                 Onslaught   Male                     red             Mutant
## 506                    Oracle Female                    blue              Human
## 507                    Osiris   Male                   brown               <NA>
## 508                 Overtkill   Male                    <NA>               <NA>
## 509                Ozymandias   Male                    blue              Human
## 510                 Parademon   <NA>                    <NA>          Parademon
## 511                Paul Blart   Male                    <NA>              Human
## 512                   Penance   <NA>                    <NA>               <NA>
## 513                 Penance I Female                    <NA>               <NA>
## 514                Penance II   Male                    blue               <NA>
## 515                   Penguin   Male                    blue              Human
## 516            Peter Petrelli   Male                    <NA>               <NA>
## 517                   Phantom   Male                    <NA>               <NA>
## 518              Phantom Girl Female                    blue               <NA>
## 519                   Phoenix Female                   green             Mutant
## 520                  Plantman   Male                   green             Mutant
## 521               Plastic Lad   Male                    <NA>               <NA>
## 522               Plastic Man   Male                    blue              Human
## 523                 Plastique Female                    blue               <NA>
## 524                Poison Ivy Female                   green              Human
## 525                   Polaris Female                   green             Mutant
## 526                Power Girl Female                    blue         Kryptonian
## 527                 Power Man   Male                    <NA>             Mutant
## 528                  Predator   Male                    <NA>             Yautja
## 529               Professor X   Male                    blue             Mutant
## 530            Professor Zoom   Male                    blue              Human
## 531              Proto-Goblin   Male                   green               <NA>
## 532                  Psylocke Female                    blue             Mutant
## 533                  Punisher   Male                    blue              Human
## 534                Purple Man   Male                  purple              Human
## 535                      Pyro   Male                    blue               <NA>
## 536                         Q   Male                    <NA>      God / Eternal
## 537                   Quantum   Male                    <NA>               <NA>
## 538                  Question   Male                    blue              Human
## 539               Quicksilver   Male                    blue             Mutant
## 540                     Quill   Male                   brown               <NA>
## 541              Ra's Al Ghul   Male                   green              Human
## 542             Rachel Pirzad Female                    <NA>              Alpha
## 543                     Rambo   Male                   brown              Human
## 544                   Raphael   Male                    <NA>             Mutant
## 545                     Raven Female                  indigo              Human
## 546                       Ray   Male                   green              Human
## 547             Razor-Fist II   Male                    blue               <NA>
## 548                 Red Arrow   Male                   green              Human
## 549                  Red Hood   Male                    blue              Human
## 550                  Red Hulk   Male                  yellow  Human / Radiation
## 551                  Red Mist   Male                    <NA>               <NA>
## 552                 Red Robin   Male                    blue              Human
## 553                 Red Skull   Male                    blue               <NA>
## 554               Red Tornado   Male                   green            Android
## 555               Redeemer II   Male                    <NA>               <NA>
## 556              Redeemer III   Male                    <NA>               <NA>
## 557              Renata Soliz Female                    <NA>               <NA>
## 558                       Rey Female                   hazel              Human
## 559                     Rhino   Male                   brown  Human / Radiation
## 560                 Rick Flag   Male                    blue               <NA>
## 561                   Riddler   Male                    <NA>               <NA>
## 562                Rip Hunter   Male                    blue              Human
## 563                   Ripcord Female                   green               <NA>
## 564                     Robin   Male                    blue              Human
## 565                  Robin II   Male                    blue              Human
## 566                 Robin III   Male                    blue              Human
## 567                   Robin V   Male                    blue              Human
## 568                  Robin VI Female                   green              Human
## 569            Rocket Raccoon   Male                   brown             Animal
## 570                     Rogue Female                   green               <NA>
## 571                     Ronin   Male                    blue              Human
## 572                 Rorschach   Male                    blue              Human
## 573                Sabretooth   Male                   amber             Mutant
## 574                      Sage Female                    blue               <NA>
## 575                   Sandman   Male                   brown              Human
## 576                 Sasquatch   Male                     red               <NA>
## 577                    Sauron   Male                    <NA>              Maiar
## 578             Savage Dragon   Male                    <NA>               <NA>
## 579                 Scarecrow   Male                    blue              Human
## 580            Scarlet Spider   Male                    blue              Human
## 581         Scarlet Spider II   Male                   brown              Clone
## 582             Scarlet Witch Female                    blue             Mutant
## 583                   Scorpia Female                   green               <NA>
## 584                  Scorpion   Male                   brown              Human
## 585            Sebastian Shaw   Male                    <NA>             Mutant
## 586                    Sentry   Male                    blue             Mutant
## 587               Shadow King   <NA>                     red               <NA>
## 588               Shadow Lass Female                   black           Talokite
## 589                 Shadowcat Female                   hazel             Mutant
## 590                 Shang-Chi   Male                   brown              Human
## 591               Shatterstar   Male                   brown               <NA>
## 592                  She-Hulk Female                   green              Human
## 593                 She-Thing Female                    blue  Human / Radiation
## 594                   Shocker   Male                   brown              Human
## 595                    Shriek Female           yellow / blue               <NA>
## 596          Shrinking Violet Female                    <NA>               <NA>
## 597                       Sif Female                    blue          Asgardian
## 598                      Silk Female                   brown              Human
## 599              Silk Spectre Female                    <NA>               <NA>
## 600           Silk Spectre II Female                    <NA>               <NA>
## 601             Silver Surfer   Male                   white              Alien
## 602                Silverclaw Female                   brown               <NA>
## 603                 Simon Baz   Male                    bown              Human
## 604                  Sinestro   Male                   black          Korugaran
## 605                     Siren Female                    blue          Atlantean
## 606                  Siren II Female                   black               <NA>
## 607                     Siryn Female                    blue               <NA>
## 608                     Skaar   Male                   green               <NA>
## 609                Snake-Eyes   Male                    <NA>             Animal
## 610                  Snowbird Female                   white               <NA>
## 611                     Sobek   Male                   white               <NA>
## 612            Solomon Grundy   Male                   black             Zombie
## 613                  Songbird Female                   green               <NA>
## 614               Space Ghost   Male                    <NA>              Human
## 615                     Spawn   Male                   brown              Demon
## 616                   Spectre   Male                   white      God / Eternal
## 617                 Speedball   Male                    <NA>               <NA>
## 618                    Speedy   Male                    <NA>              Human
## 619                    Speedy Female                   green              Human
## 620            Spider-Carnage   Male                    <NA>           Symbiote
## 621               Spider-Girl Female                    blue              Human
## 622               Spider-Gwen Female                    blue              Human
## 623                Spider-Man   Male                   hazel              Human
## 624                Spider-Man   <NA>                     red              Human
## 625                Spider-Man   Male                   brown              Human
## 626              Spider-Woman Female                   green              Human
## 627           Spider-Woman II Female                    <NA>               <NA>
## 628          Spider-Woman III Female                   brown               <NA>
## 629           Spider-Woman IV Female                     red               <NA>
## 630                     Spock   Male                   brown       Human-Vulcan
## 631                     Spyke   Male                   brown             Mutant
## 632                   Stacy X Female                    <NA>               <NA>
## 633                 Star-Lord   Male                    blue      Human-Spartoi
## 634                  Stardust   Male                    <NA>               <NA>
## 635                  Starfire Female                   green         Tamaranean
## 636                  Stargirl Female                    blue              Human
## 637                    Static   Male                   brown             Mutant
## 638                     Steel   Male                   brown               <NA>
## 639          Stephanie Powell Female                    <NA>               <NA>
## 640               Steppenwolf   Male                     red            New God
## 641                     Storm Female                    blue             Mutant
## 642              Stormtrooper   Male                    <NA>              Human
## 643                   Sunspot   Male                   brown             Mutant
## 644                  Superboy   Male                    blue               <NA>
## 645            Superboy-Prime   Male                    blue         Kryptonian
## 646                 Supergirl Female                    blue         Kryptonian
## 647                  Superman   Male                    blue         Kryptonian
## 648               Swamp Thing   Male                     red      God / Eternal
## 649                     Swarm   Male                  yellow             Mutant
## 650                     Sylar   Male                    <NA>               <NA>
## 651                     Synch   Male                   brown               <NA>
## 652                    T-1000   Male                    <NA>            Android
## 653                     T-800   Male                     red             Cyborg
## 654                     T-850   Male                     red             Cyborg
## 655                       T-X Female                    <NA>             Cyborg
## 656                Taskmaster   Male                   brown              Human
## 657                   Tempest Female                   brown               <NA>
## 658                    Thanos   Male                     red            Eternal
## 659                  The Cape   Male                    <NA>               <NA>
## 660              The Comedian   Male                   brown              Human
## 661                     Thing   Male                    blue  Human / Radiation
## 662                      Thor   Male                    blue          Asgardian
## 663                 Thor Girl Female                    blue          Asgardian
## 664               Thunderbird   Male                   brown               <NA>
## 665            Thunderbird II   Male                    <NA>               <NA>
## 666           Thunderbird III   Male                   brown               <NA>
## 667             Thunderstrike   Male                    blue               <NA>
## 668                   Thundra Female                   green               <NA>
## 669               Tiger Shark   Male                    grey              Human
## 670                     Tigra Female                   green               <NA>
## 671                  Tinkerer   Male                   brown               <NA>
## 672                     Titan   Male                    <NA>               <NA>
## 673                      Toad   Male                   black             Mutant
## 674                     Toxin   Male                    blue           Symbiote
## 675                     Toxin   Male                   black           Symbiote
## 676             Tracy Strauss Female                    <NA>               <NA>
## 677                 Trickster   Male                    blue              Human
## 678                    Trigon   Male                  yellow      God / Eternal
## 679           Triplicate Girl Female                  purple               <NA>
## 680                    Triton   Male                   green            Inhuman
## 681                  Two-Face   Male                    <NA>               <NA>
## 682                 Ultragirl Female                    blue               <NA>
## 683                    Ultron   Male                     red            Android
## 684               Utgard-Loki   Male                    blue        Frost Giant
## 685                  Vagabond Female                    blue               <NA>
## 686              Valerie Hart Female                   hazel               <NA>
## 687                  Valkyrie Female                    blue               <NA>
## 688                  Vanisher   Male                   green               <NA>
## 689                    Vegeta   Male                    <NA>             Saiyan
## 690                     Venom   Male                    blue           Symbiote
## 691                  Venom II   Male                   brown               <NA>
## 692                 Venom III   Male                   brown           Symbiote
## 693                 Venompool   Male                    <NA>           Symbiote
## 694                Vertigo II Female                    blue               <NA>
## 695                      Vibe   Male                   brown              Human
## 696                Vindicator Female                   green              Human
## 697                Vindicator   Male                    <NA>               <NA>
## 698                  Violator   Male                    <NA>               <NA>
## 699               Violet Parr Female                  violet              Human
## 700                    Vision   Male                    gold            Android
## 701                 Vision II   <NA>                     red               <NA>
## 702                     Vixen Female                   amber              Human
## 703                    Vulcan   Male                   black               <NA>
## 704                   Vulture   Male                   brown              Human
## 705                    Walrus   Male                    blue              Human
## 706               War Machine   Male                   brown              Human
## 707                   Warbird Female                    blue               <NA>
## 708                   Warlock   Male                     red               <NA>
## 709                      Warp   Male                   brown               <NA>
## 710                   Warpath   Male                   brown             Mutant
## 711                      Wasp Female                    blue              Human
## 712                   Watcher   Male                    <NA>               <NA>
## 713                 Weapon XI   Male                    <NA>               <NA>
## 714              White Canary Female                   brown              Human
## 715               White Queen Female                    blue               <NA>
## 716                  Wildfire   Male                    <NA>               <NA>
## 717            Winter Soldier   Male                   brown              Human
## 718                   Wiz Kid   <NA>                   brown               <NA>
## 719                 Wolfsbane Female                   green               <NA>
## 720                 Wolverine   Male                    blue             Mutant
## 721               Wonder Girl Female                    blue           Demi-God
## 722                Wonder Man   Male                     red               <NA>
## 723              Wonder Woman Female                    blue             Amazon
## 724                    Wondra Female                    <NA>               <NA>
## 725            Wyatt Wingfoot   Male                   brown               <NA>
## 726                      X-23 Female                   green     Mutant / Clone
## 727                     X-Man   Male                    blue               <NA>
## 728               Yellow Claw   Male                    blue               <NA>
## 729              Yellowjacket   Male                    blue              Human
## 730           Yellowjacket II Female                    blue              Human
## 731                      Ymir   Male                   white        Frost Giant
## 732                      Yoda   Male                   brown     Yoda's species
## 733                   Zatanna Female                    blue              Human
## 734                      Zoom   Male                     red               <NA>
##           Hair color Height         Publisher     Skin color Alignment Weight
## 1            No Hair  203.0     Marvel Comics           <NA>      good    441
## 2            No Hair  191.0 Dark Horse Comics           blue      good     65
## 3            No Hair  185.0         DC Comics            red      good     90
## 4            No Hair  203.0     Marvel Comics           <NA>       bad    441
## 5              Black     NA     Marvel Comics           <NA>       bad     NA
## 6            No Hair  193.0     Marvel Comics           <NA>       bad    122
## 7              Blond     NA      NBC - Heroes           <NA>      good     NA
## 8              Blond  185.0         DC Comics           <NA>      good     88
## 9              Blond  173.0     Marvel Comics           <NA>      good     61
## 10             Brown  178.0     Marvel Comics           <NA>      good     81
## 11              <NA>  191.0     Marvel Comics           <NA>      good    104
## 12             White  188.0     Marvel Comics           <NA>       bad    108
## 13             Black  193.0     Marvel Comics           <NA>       bad     90
## 14             Blond  180.0         DC Comics           <NA>      good     90
## 15              <NA>     NA         Wildstorm           <NA>       bad     NA
## 16              <NA>     NA      NBC - Heroes           <NA>      good     NA
## 17             Black  178.0         DC Comics           <NA>      good     72
## 18           No Hair  244.0 Dark Horse Comics          black       bad    169
## 19              <NA>     NA         Wildstorm           <NA>      good     NA
## 20              <NA>  257.0         DC Comics           <NA>       bad    173
## 21             Black  188.0     Marvel Comics           <NA>       bad    101
## 22              <NA>     NA      NBC - Heroes           <NA>      good     NA
## 23             Blond  183.0     Marvel Comics           <NA>      good     68
## 24              <NA>     NA Dark Horse Comics           <NA>      good     NA
## 25             Black  165.0     Marvel Comics           <NA>      good     57
## 26             Black  163.0     Marvel Comics           <NA>      good     54
## 27              <NA>     NA      Image Comics           <NA>       bad     NA
## 28             Blond  183.0         DC Comics           <NA>      good     83
## 29           No Hair  180.0     Marvel Comics           <NA>       bad     90
## 30             Blond  211.0     Marvel Comics           <NA>      good    122
## 31             Blond  183.0     Marvel Comics           <NA>      good     86
## 32           No Hair   61.0         DC Comics           <NA>       bad     NA
## 33              <NA>     NA      Image Comics           <NA>       bad     NA
## 34             Blond  229.0     Marvel Comics           <NA>      <NA>    358
## 35             Black  213.0     Marvel Comics           grey       bad    135
## 36             Blond     NA         DC Comics           <NA>      good     NA
## 37             Black  178.0         DC Comics           <NA>      good    106
## 38             Blond  185.0         DC Comics           <NA>      good    146
## 39             Blond  175.0     Marvel Comics           <NA>      good     63
## 40             Blond  183.0     Marvel Comics           blue      good     68
## 41            Purple  173.0     Marvel Comics           <NA>       bad     57
## 42            Orange  193.0     Marvel Comics           gold      good     98
## 43             Brown  185.0     Marvel Comics           <NA>      good    270
## 44              Pink  165.0     Marvel Comics           <NA>      good     59
## 45             Black  163.0     Marvel Comics           <NA>      good     50
## 46              <NA>     NA         DC Comics           <NA>      good     NA
## 47             Black     NA              <NA>           <NA>      good     NA
## 48               Red  183.0     Marvel Comics           <NA>      good    101
## 49             Brown  198.0         DC Comics           <NA>       bad    126
## 50               Red  178.0         DC Comics           <NA>      good     68
## 51              <NA>     NA         DC Comics           <NA>      good     NA
## 52             Black  168.0         DC Comics           <NA>      good     54
## 53            Auburn  183.0         DC Comics           <NA>      good     81
## 54               Red     NA         DC Comics           <NA>      good     NA
## 55             Black     NA         DC Comics           <NA>      good     72
## 56             Black  180.0     Marvel Comics           <NA>      good     63
## 57             Black  183.0     Marvel Comics            red       bad     67
## 58             Black     NA         DC Comics           <NA>      good     NA
## 59              <NA>     NA         DC Comics           <NA>      good     NA
## 60              <NA>  203.0         DC Comics           <NA>       bad    180
## 61  Strawberry Blond  183.0     Marvel Comics           <NA>      good     77
## 62             Black  165.0     Marvel Comics           <NA>      good     54
## 63              <NA>     NA         DC Comics           <NA>      good     NA
## 64               Red  170.0         DC Comics           <NA>      good     57
## 65              <NA>     NA         DC Comics           <NA>      good     NA
## 66             Black  165.0         DC Comics           <NA>      good     52
## 67              <NA>     NA         DC Comics           <NA>      good     NA
## 68             Blond  168.0         DC Comics           <NA>      good     61
## 69             black  188.0         DC Comics           <NA>      good     95
## 70             Black  178.0         DC Comics           <NA>      good     77
## 71             Black  178.0         DC Comics           <NA>      good     79
## 72             Black  198.0     Marvel Comics           <NA>      good    133
## 73               Red  178.0         DC Comics           <NA>      good     NA
## 74             White  175.0     Marvel Comics           <NA>      good     63
## 75              Blue  180.0     Marvel Comics           blue      good    181
## 76             Green  173.0         DC Comics          green      good     68
## 77              <NA>     NA     Marvel Comics           <NA>       bad     NA
## 78              <NA>     NA         DC Comics           <NA>      good     NA
## 79           No Hair  201.0     Marvel Comics           <NA>      good    216
## 80              <NA>     NA     Marvel Comics           <NA>      good     NA
## 81             Black  188.0         DC Comics           <NA>       bad    135
## 82              <NA>     NA       Icon Comics           <NA>      good     NA
## 83             Brown  165.0     Marvel Comics           <NA>       bad     71
## 84              <NA>     NA              SyFy           <NA>      good     NA
## 85              <NA>     NA      Image Comics           <NA>       bad     NA
## 86             Blond  180.0     Marvel Comics           <NA>      good     54
## 87             Black     NA              <NA>           <NA>      good     NA
## 88              <NA>     NA     Marvel Comics           <NA>      good     NA
## 89              <NA>     NA     Marvel Comics           <NA>       bad     NA
## 90              <NA>     NA     Marvel Comics           <NA>       bad     NA
## 91              <NA>     NA     Hanna-Barbera           <NA>      good     NA
## 92           No Hair  198.0     Marvel Comics           <NA>      good    124
## 93             Black  191.0         DC Comics          white   neutral    155
## 94             Black     NA     Marvel Comics           <NA>       bad     NA
## 95             Black  191.0         DC Comics           <NA>       bad    113
## 96             Black  188.0     Marvel Comics           <NA>      good     95
## 97             Blond  165.0         DC Comics           <NA>      good     58
## 98             Blond  170.0         DC Comics           <NA>      good     59
## 99             Blond  178.0     Marvel Comics           <NA>      good     54
## 100             <NA>     NA         DC Comics           <NA>   neutral     NA
## 101             <NA>     NA     Marvel Comics           <NA>      good     NA
## 102            Brown  183.0     Marvel Comics           <NA>      good     86
## 103          No Hair  185.0         DC Comics           <NA>      good     90
## 104            Black  170.0     Marvel Comics           <NA>       bad     52
## 105          No Hair  188.0         DC Comics           <NA>       bad     92
## 106            Black  183.0     Marvel Comics           <NA>      good     90
## 107           Auburn  170.0     Marvel Comics           <NA>      good     59
## 108            Blond  170.0     Marvel Comics           <NA>      good     61
## 109            White  191.0     Marvel Comics          white       bad    104
## 110            Black  185.0     Marvel Comics           <NA>       bad     86
## 111            White  188.0     Marvel Comics           <NA>      <NA>     88
## 112            Black  188.0     Marvel Comics           <NA>      good     97
## 113          No Hair     NA     Marvel Comics           <NA>      good     NA
## 114             <NA>  168.0     Marvel Comics           <NA>      good     68
## 115          Magenta  165.0     Marvel Comics           pink      good     56
## 116             <NA>     NA     Marvel Comics           <NA>       bad     NA
## 117            Brown     NA     Marvel Comics           <NA>       bad     NA
## 118            Brown  175.0     Marvel Comics           <NA>       bad     77
## 119            Brown  178.0     Marvel Comics           <NA>       bad    230
## 120            Brown  218.0     Marvel Comics           <NA>       bad    495
## 121          No Hair     NA     Marvel Comics           <NA>      good     NA
## 122          No Hair   30.5     Marvel Comics           <NA>       bad     NA
## 123            Brown     NA         DC Comics           <NA>      good     NA
## 124             <NA>     NA         DC Comics           <NA>      good     NA
## 125            Brown  183.0         DC Comics           <NA>      good     86
## 126            Black     NA         DC Comics           <NA>      good     NA
## 127            Black  183.0      George Lucas           <NA>       bad     NA
## 128             <NA>     NA     Marvel Comics           <NA>      good     NA
## 129             <NA>     NA      Image Comics           <NA>       bad     NA
## 130            Blond  165.0     Marvel Comics           <NA>      good     55
## 131             <NA>     NA     Marvel Comics           <NA>      good     NA
## 132            Blond  196.0         DC Comics           <NA>      good     97
## 133             <NA>     NA     Marvel Comics           <NA>      good     NA
## 134            Blond  193.0     Marvel Comics           <NA>      good    110
## 135    Brown / Black     NA     Marvel Comics           <NA>      good     NA
## 136          No Hair  198.0         DC Comics          green       bad    135
## 137            Blond  170.0         DC Comics           <NA>      good     61
## 138    Brown / White  183.0     Marvel Comics           <NA>      good     99
## 139             <NA>  193.0              <NA>           <NA>      <NA>     NA
## 140            Blond  157.0 Dark Horse Comics           <NA>      good     52
## 141            blond  183.0     Marvel Comics           <NA>       bad     90
## 142            Black  170.0         DC Comics           <NA>      good     59
## 143             <NA>     NA     Marvel Comics           <NA>      good     NA
## 144             <NA>     NA         DC Comics           <NA>      good     NA
## 145            White  203.0     Marvel Comics           <NA>      good    158
## 146            Black  175.0     Marvel Comics           <NA>       bad     74
## 147             <NA>     NA              SyFy           <NA>      good     NA
## 148            Blond  183.0     Marvel Comics           <NA>      good     81
## 149            blond  188.0     Marvel Comics           <NA>      good    108
## 150           Silver  193.0         DC Comics         silver      good     90
## 151            Blond  198.0     Marvel Comics           <NA>      good    116
## 152            Brown     NA         DC Comics           <NA>   neutral     NA
## 153            Brown  188.0      Team Epic TV           <NA>      good     NA
## 154            Black     NA        South Park           <NA>      good     NA
## 155            Blond  188.0     Marvel Comics           <NA>      good    108
## 156            Blond  180.0     Marvel Comics           <NA>      good     74
## 157            Black  193.0         DC Comics           <NA>      good    101
## 158            Black  175.0         DC Comics           <NA>      good     74
## 159             <NA>     NA Dark Horse Comics           <NA>      good     NA
## 160            Green     NA     Marvel Comics           <NA>      good     NA
## 161             <NA>     NA     Marvel Comics           <NA>      good     NA
## 162              Red  185.0     Marvel Comics           <NA>       bad     86
## 163            Blond  173.0     Marvel Comics           <NA>      good     61
## 164             <NA>     NA     Marvel Comics           <NA>      good     NA
## 165            Black  175.0         DC Comics           <NA>      good     61
## 166            Brown  170.0     Marvel Comics           <NA>      good     62
## 167            White  201.0     Marvel Comics           grey      good     97
## 168             <NA>     NA     Marvel Comics           <NA>      good     NA
## 169            Brown  175.0     Marvel Comics           <NA>      good     63
## 170             <NA>     NA         DC Comics           <NA>       bad     NA
## 171            Black  180.0     Marvel Comics           <NA>       bad     81
## 172            Blond  163.0         DC Comics           <NA>       bad     50
## 173            Brown  170.0         DC Comics           <NA>       bad     55
## 174            Brown  175.0         DC Comics           <NA>       bad     54
## 175       Red / Grey  185.0      Team Epic TV           <NA>       bad     86
## 176             <NA>  178.0              <NA>           <NA>      good     NA
## 177              Red  183.0         DC Comics           <NA>      good    170
## 178            Blond     NA      NBC - Heroes           <NA>      good     NA
## 179            White     NA     Marvel Comics           <NA>      good     NA
## 180            black  226.0     Marvel Comics           <NA>      good     70
## 181            Black  178.0         DC Comics           <NA>       bad     78
## 182             <NA>     NA      Image Comics           <NA>       bad     NA
## 183            Brown     NA     HarperCollins           <NA>      good     NA
## 184             <NA>     NA         DC Comics           <NA>      good     NA
## 185            Black  226.0     Marvel Comics           <NA>      good    225
## 186            White  183.0     Marvel Comics           blue   neutral     67
## 187            Brown  191.0     Marvel Comics           <NA>      good     79
## 188            Black  183.0     Marvel Comics           <NA>       bad     99
## 189 Strawberry Blond     NA     Marvel Comics           <NA>      good     NA
## 190          No Hair  180.0     Marvel Comics           <NA>      good    104
## 191              Red  168.0     Marvel Comics           <NA>      good     50
## 192             <NA>     NA      Image Comics           <NA>       bad     NA
## 193             <NA>     NA      Image Comics           <NA>       bad     NA
## 194            Black  198.0         DC Comics           <NA>      good    173
## 195            Black     NA         DC Comics           <NA>       bad     NA
## 196            Brown  191.0     Marvel Comics           <NA>      good     88
## 197            Blond  175.0     Marvel Comics           <NA>      good     68
## 198            Blond  165.0     Marvel Comics           <NA>      good     52
## 199            Blond     NA     HarperCollins           <NA>      good     NA
## 200             <NA>     NA       ABC Studios           <NA>      good     NA
## 201              Red  183.0     Marvel Comics           <NA>      good     90
## 202            Brown  185.0     Marvel Comics           <NA>      good     81
## 203             <NA>     NA Universal Studios           <NA>      good     NA
## 204          No Hair  267.0         DC Comics           grey       bad    817
## 205             <NA>     NA              <NA>           <NA>       bad     NA
## 206            Blond  168.0     Marvel Comics           <NA>      good     56
## 207             <NA>  170.0      George Lucas    red / black       bad     NA
## 208          No Hair  198.0      George Lucas           <NA>       bad    135
## 209            Blond  122.0 Dark Horse Comics           <NA>      good     27
## 210            Brown     NA         Star Trek           <NA>      good     NA
## 211            Blond  173.0     Marvel Comics           <NA>      good     52
## 212            Black  183.0         DC Comics           <NA>      good     90
## 213          No Hair  188.0     Marvel Comics           <NA>   neutral     95
## 214            Brown  185.0         DC Comics           <NA>       bad     91
## 215             Grey  193.0     Marvel Comics           <NA>      good    178
## 216            White  193.0         DC Comics           <NA>   neutral    101
## 217          No Hair  185.0     Marvel Comics           <NA>       bad     95
## 218             <NA>  188.0     Marvel Comics           <NA>       bad    383
## 219            Black  193.0     Marvel Comics           <NA>       bad     90
## 220             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 221            Green  198.0     Marvel Comics           <NA>      good    171
## 222            Brown  201.0     Marvel Comics           <NA>       bad    187
## 223            Brown  201.0     Marvel Comics           <NA>       bad    132
## 224            Blond  188.0         DC Comics           <NA>      good     89
## 225            Brown  175.0     Marvel Comics           <NA>       bad    110
## 226            Black  188.0     Marvel Comics           <NA>      good     81
## 227            Black  173.0     Marvel Comics          white      good     54
## 228          No Hair     NA    IDW Publishing          green      good     NA
## 229            Black  175.0         DC Comics           <NA>      good     63
## 230            White  244.0         DC Comics           <NA>       bad    412
## 231          No Hair  196.0     Marvel Comics           <NA>       bad    104
## 232          No Hair  185.0     Marvel Comics           <NA>       bad     NA
## 233          No Hair     NA         DC Comics           blue      good     NA
## 234          No Hair  193.0     Marvel Comics          green      good    306
## 235             <NA>     NA     Marvel Comics           <NA>       bad     NA
## 236            Brown  168.0 Dark Horse Comics           <NA>      good     56
## 237           Auburn  180.0     Marvel Comics           <NA>       bad     74
## 238            Black  175.0     Marvel Comics           <NA>      good     59
## 239            Blond     NA      NBC - Heroes           <NA>       bad     NA
## 240              Red  185.0         DC Comics           <NA>      good     80
## 241            Blond  178.0     Marvel Comics           <NA>      good     65
## 242            Blond  168.0         DC Comics           <NA>      good     57
## 243             <NA>     NA     HarperCollins           <NA>      good     NA
## 244             <NA>     NA         DC Comics           <NA>      good     NA
## 245            Brown  168.0              <NA>           <NA>      good     NA
## 246          No Hair  193.0         DC Comics         yellow   neutral    203
## 247              Red  188.0     Marvel Comics           <NA>       bad     95
## 248            Black  191.0     Marvel Comics          green       bad    106
## 249            Black  183.0     Marvel Comics            red       bad     88
## 250            Brown  196.0     Marvel Comics           <NA>       bad     96
## 251            Black  188.0     Marvel Comics           <NA>      good    108
## 252             Blue     NA     Marvel Comics           <NA>       bad     NA
## 253             <NA>     NA         DC Comics           <NA>       bad     NA
## 254   Orange / White  175.0     Marvel Comics           <NA>      good     50
## 255              Red     NA         DC Comics           <NA>      good     NA
## 256          No Hair  975.0     Marvel Comics          green      good     18
## 257            Black  165.0     Marvel Comics           <NA>      good     56
## 258           Yellow  193.0     Marvel Comics           <NA>      good     99
## 259              Red  173.0     Marvel Comics           <NA>      good     56
## 260            Black     NA         DC Comics           <NA>      good     NA
## 261           Auburn  188.0         DC Comics           <NA>      good     91
## 262          No Hair     NA     Marvel Comics           <NA>       bad     NA
## 263    Brown / White  180.0         DC Comics           <NA>      good     81
## 264             <NA>     NA              <NA>           <NA>      good     NA
## 265            Blond  183.0         DC Comics           <NA>      good     88
## 266             <NA>  183.0         DC Comics           <NA>      good     86
## 267           Auburn  157.0         DC Comics           <NA>      good     52
## 268            Black  183.0     Marvel Comics           <NA>      good     81
## 269            Blond  142.0     Marvel Comics           <NA>      good     45
## 270             Grey  188.0     Marvel Comics           <NA>      good     92
## 271            Black  211.0     Marvel Comics           <NA>       bad    104
## 272            White  180.0     Marvel Comics           <NA>      good    167
## 273            Black  876.0     Marvel Comics           <NA>   neutral     16
## 274            Brown  185.0     Marvel Comics           <NA>      good     81
## 275            Black  183.0     Marvel Comics          green      good     77
## 276             <NA>     NA         DC Comics           <NA>      good     NA
## 277             <NA>     NA              SyFy           <NA>      good     NA
## 278            Black     NA         DC Comics           <NA>       bad     NA
## 279            Blond  185.0     Marvel Comics           <NA>      good     86
## 280          No Hair  188.0     Marvel Comics           <NA>      good     99
## 281             <NA>     NA     Marvel Comics           <NA>      good     NA
## 282             <NA>     NA     Marvel Comics           <NA>      good     NA
## 283             <NA>     NA     Marvel Comics           <NA>      good     NA
## 284              Red   62.5         DC Comics           <NA>       bad    630
## 285             Blue  198.0     Marvel Comics         purple   neutral    268
## 286              Red  168.0     Marvel Comics           <NA>       bad     50
## 287             <NA>  108.0              <NA>           grey       bad     NA
## 288             <NA>     NA         DC Comics           <NA>       bad     NA
## 289             <NA>  175.0          Shueisha           <NA>      good     62
## 290             <NA>     NA     Marvel Comics           <NA>      good     NA
## 291             <NA>     NA     Marvel Comics           <NA>      good     NA
## 292             <NA>     NA     Marvel Comics           <NA>      good     NA
## 293            Black  183.0     Marvel Comics           <NA>      good     90
## 294            Black  198.0         DC Comics           <NA>       bad    270
## 295            White  178.0         DC Comics           <NA>       bad    115
## 296            Brown  178.0     Marvel Comics           <NA>      good     79
## 297             <NA>  170.0      George Lucas          green       bad     NA
## 298            Blond  188.0         DC Comics           <NA>      good     88
## 299           Auburn  180.0     Marvel Comics           <NA>       bad     83
## 300           Auburn  178.0     Marvel Comics           <NA>       bad     77
## 301             <NA>  183.0     Marvel Comics           <NA>      good     88
## 302            Brown  178.0     Marvel Comics           <NA>      good     79
## 303             <NA>  701.0     Marvel Comics           <NA>      good      4
## 304            Black     NA     Marvel Comics           <NA>      good     NA
## 305              Red  188.0         DC Comics           <NA>      good     95
## 306            Brown  188.0         DC Comics           <NA>      good     90
## 307            Brown  183.0      George Lucas           <NA>      good     79
## 308            Black  188.0     Sony Pictures           <NA>      good     NA
## 309            Blond  170.0         DC Comics           <NA>       bad     63
## 310            Black     NA     J. K. Rowling           <NA>      good     NA
## 311            Blond  183.0     Marvel Comics           <NA>      good     79
## 312            Brown  185.0         DC Comics           <NA>      good     89
## 313            Blond  191.0     Marvel Comics           <NA>      good    104
## 314            Black  165.0     Marvel Comics           <NA>      good     57
## 315              Red  175.0         DC Comics           <NA>      good     61
## 316            Brown  185.0         DC Comics           <NA>      good     88
## 317              Red  175.0         DC Comics           <NA>      good     54
## 318             <NA>     NA         DC Comics           <NA>      good     NA
## 319              Red  170.0         DC Comics           <NA>      good     65
## 320          No Hair  180.0         DC Comics           <NA>       bad     81
## 321            Black  213.0     Marvel Comics           <NA>       bad    225
## 322            Black  259.0 Dark Horse Comics           <NA>      good    158
## 323              Red  173.0     Marvel Comics           <NA>      good     61
## 324              Red  185.0     Marvel Comics           <NA>      good     81
## 325            Brown  196.0     Marvel Comics           <NA>      good    146
## 326             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 327             <NA>     NA       Icon Comics           <NA>      good     NA
## 328             Grey  180.0     Marvel Comics           <NA>       bad     83
## 329              Red  170.0     Marvel Comics           <NA>      good     NA
## 330              Red  168.0     Marvel Comics           <NA>      good     48
## 331           Yellow   79.0     Marvel Comics           <NA>      good     18
## 332            Green  244.0     Marvel Comics          green      good    630
## 333            Blond  178.0     Marvel Comics           <NA>      good     77
## 334            Black  180.0         DC Comics           <NA>      good     59
## 335            Blond  170.0     Marvel Comics           <NA>      good     58
## 336            Black  175.0     Marvel Comics           <NA>      good     77
## 337            Brown  188.0     Marvel Comics           <NA>       bad    119
## 338              Red  183.0     Marvel Comics           <NA>      good    207
## 339            Brown  173.0     Marvel Comics           <NA>      good     65
## 340           Auburn  170.0         DC Comics           <NA>      good     65
## 341             <NA>  183.0      George Lucas           <NA>      good     79
## 342           Purple     NA         DC Comics           <NA>   neutral     NA
## 343          No Hair  180.0     Marvel Comics           <NA>      good     81
## 344            Blond  168.0     Marvel Comics           <NA>      good     54
## 345            Blond  180.0     Marvel Comics           <NA>      good     79
## 346            Black  198.0     Marvel Comics           <NA>      good    191
## 347          No Hair     NA     Marvel Comics           <NA>       bad      2
## 348             <NA>     NA         DC Comics           <NA>      good     NA
## 349             <NA>     NA              <NA>           <NA>      good     NA
## 350            Brown  155.0     Marvel Comics           <NA>      good     79
## 351            Brown   71.0 Dark Horse Comics           <NA>      good     14
## 352            Blond  183.0       Titan Books           <NA>      good     NA
## 353            Brown  178.0         Star Trek           <NA>      good     77
## 354             <NA>  193.0      George Lucas orange / white      good     NA
## 355             <NA>     NA              <NA>           <NA>      good     NA
## 356              Red  168.0     Marvel Comics           <NA>      good     52
## 357             <NA>     NA         Star Trek           <NA>      good     NA
## 358            Blond  168.0     Marvel Comics           <NA>      good     55
## 359             <NA>     NA         DC Comics           <NA>      good     NA
## 360            Brown     NA         DC Comics           <NA>      good     NA
## 361            Brown  170.0     Marvel Comics           <NA>      good     56
## 362             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 363            Black  188.0     Marvel Comics           <NA>       bad    113
## 364             <NA>     NA       ABC Studios           <NA>      good     NA
## 365             <NA>     NA       ABC Studios           <NA>      good     NA
## 366             <NA>     NA Dark Horse Comics           <NA>      good     NA
## 367            Blond  183.0         DC Comics           <NA>      good     NA
## 368            Black  185.0         DC Comics           <NA>      good     90
## 369            Black  183.0     Marvel Comics           <NA>      good     88
## 370            Green  196.0         DC Comics          white       bad     86
## 371            Black  165.0     Marvel Comics           <NA>      good     49
## 372            Black  165.0     Marvel Comics           <NA>      good     52
## 373             <NA>  188.0         Rebellion           <NA>      good     NA
## 374              Red  287.0     Marvel Comics           <NA>   neutral    855
## 375             <NA>     NA     Marvel Comics           <NA>       bad     NA
## 376            Brown  178.0     Marvel Comics           <NA>      good     81
## 377            Brown     NA      George Lucas           <NA>      good     NA
## 378          No Hair  213.0      George Lucas           gray      good     NA
## 379            Brown  191.0     Marvel Comics           <NA>       bad    104
## 380            Brown  173.0         DC Comics           <NA>      good     72
## 381             <NA>     NA         Star Trek           <NA>      good     NA
## 382             <NA>     NA              <NA>           <NA>      good     NA
## 383            Black     NA         DC Comics           <NA>      good     NA
## 384            Blond     NA       Icon Comics           <NA>      good     NA
## 385              Red     NA         DC Comics           <NA>      good     NA
## 386             <NA>     NA         DC Comics           <NA>      good     NA
## 387          No Hair  244.0         DC Comics          green       bad    356
## 388            Blond     NA         DC Comics           blue       bad     NA
## 389          No Hair  234.0         DC Comics           pink      good    324
## 390            Black   30.5              <NA>           <NA>      good     NA
## 391          No Hair     NA         DC Comics           <NA>       bad     NA
## 392          No Hair  201.0     Marvel Comics           <NA>       bad    203
## 393          No Hair  188.0     Marvel Comics            red       bad     97
## 394          No Hair     NA              <NA>            red      good     NA
## 395            Black  191.0     Marvel Comics           <NA>       bad     99
## 396            Black  183.0     Marvel Comics           <NA>       bad    106
## 397            White   64.0         DC Comics           <NA>      good     18
## 398            Black  180.0         DC Comics           <NA>      good     79
## 399             <NA>     NA      George Lucas           <NA>       bad     NA
## 400            Black     NA     Marvel Comics           <NA>       bad     NA
## 401            Black  175.0     Marvel Comics           <NA>       bad     58
## 402          No Hair  178.0     Marvel Comics           <NA>       bad     63
## 403             <NA>     NA     Marvel Comics           <NA>      good     NA
## 404            Black  175.0     Marvel Comics           <NA>      good     59
## 405          No Hair     NA    IDW Publishing          green      good     NA
## 406          No Hair  188.0         DC Comics           <NA>       bad     95
## 407              Red  165.0         DC Comics           <NA>      good     54
## 408              Red  155.0         DC Comics           <NA>      good     65
## 409              Red  191.0         DC Comics           <NA>       bad     95
## 410             <NA>  198.0     Marvel Comics           <NA>       bad    360
## 411          No Hair     NA     Marvel Comics           gold   neutral     NA
## 412             <NA>     NA Dark Horse Comics           <NA>      good     NA
## 413          No Hair  203.0     Marvel Comics           <NA>       bad    230
## 414            Black  229.0         DC Comics     blue-white   neutral    288
## 415            Black  193.0     Marvel Comics           <NA>       bad    236
## 416            Blond  188.0     Marvel Comics           <NA>      good     36
## 417            Black  198.0     Marvel Comics           <NA>      good    191
## 418             <NA>     NA      NBC - Heroes           <NA>       bad     NA
## 419            Blond  168.0      George Lucas           <NA>      good     77
## 420             <NA>     NA     Marvel Comics           <NA>      good     NA
## 421            Green     NA     Marvel Comics           <NA>      good     NA
## 422            Brown  180.0     Marvel Comics           <NA>       bad     79
## 423            Black  183.0     Marvel Comics           <NA>      good    383
## 424            White  188.0     Marvel Comics           <NA>       bad     86
## 425            Blond     NA         DC Comics           <NA>      good     NA
## 426             <NA>  183.0     Marvel Comics           <NA>       bad     NA
## 427           Silver     NA      Image Comics           <NA>      <NA>     NA
## 428            Brown     NA         DC Comics           <NA>   neutral     NA
## 429          No Hair  213.0     Marvel Comics          green      good    225
## 430           Auburn  188.0     Marvel Comics           <NA>      good     90
## 431            White  188.0     Marvel Comics           <NA>       bad     97
## 432            Black  168.0     Marvel Comics          green      good     52
## 433          No Hair  201.0         DC Comics          green      good    135
## 434              Red  170.0     Marvel Comics           <NA>      good     56
## 435            Black  183.0      Team Epic TV           <NA>      good     81
## 436            Brown  213.0         Microsoft           <NA>      good     NA
## 437            Black     NA         DC Comics           <NA>       bad     NA
## 438             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 439            Black  193.0     Marvel Comics           <NA>      good    110
## 440              Red  180.0         DC Comics           <NA>       bad     72
## 441             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 442              Red  180.0     Marvel Comics           <NA>      good     59
## 443            Blond  165.0     Marvel Comics           <NA>      good     54
## 444            Black  198.0     Marvel Comics           <NA>       bad    140
## 445              Red  175.0         DC Comics           <NA>      good     72
## 446            Brown  196.0         DC Comics           <NA>       bad     90
## 447          No Hair  185.0         DC Comics           <NA>      good     90
## 448             <NA>     NA     Marvel Comics           <NA>      good     NA
## 449            Black  185.0         DC Comics           <NA>      good     86
## 450            Black     NA      NBC - Heroes           <NA>      good     NA
## 451             <NA>     NA    IDW Publishing          green      good     NA
## 452            Brown  183.0         DC Comics           <NA>      good     77
## 453            Brown  188.0     Marvel Comics           <NA>      good    101
## 454             <NA>     NA         Wildstorm           <NA>      good     NA
## 455              Red     NA         DC Comics           <NA>      good     NA
## 456              Red  178.0         DC Comics           <NA>      good     61
## 457            Brown  185.0     Marvel Comics           <NA>      good     81
## 458             <NA>  183.0         DC Comics           <NA>       bad     86
## 459            Brown     NA     Marvel Comics           <NA>       bad     NA
## 460             <NA>     NA         DC Comics           <NA>       bad     NA
## 461            Black  196.0     Marvel Comics           <NA>       bad    128
## 462            Blond     NA         DC Comics           <NA>       bad     NA
## 463            Blond  175.0     Marvel Comics           <NA>      good     61
## 464           Brownn  366.0     Marvel Comics           <NA>       bad    338
## 465             <NA>     NA         DC Comics           <NA>      good     NA
## 466             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 467             <NA>     NA         DC Comics           <NA>       bad     NA
## 468             Gold  196.0     Marvel Comics           <NA>       bad    248
## 469            White  193.0         DC Comics           <NA>      good     90
## 470             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 471            Brown  188.0     Marvel Comics           <NA>      good    101
## 472            Blond  180.0     Marvel Comics           <NA>       bad     59
## 473            Black  188.0     Marvel Comics           <NA>       bad     79
## 474          No Hair  178.0     Marvel Comics           <NA>      good     79
## 475            Black  175.0     Marvel Comics           <NA>       bad     72
## 476            Blond  188.0     Marvel Comics           <NA>      good     70
## 477            Blond  201.0 Dark Horse Comics           <NA>      good    158
## 478              Red  173.0     Marvel Comics           <NA>      good     61
## 479            Brown  180.0     Marvel Comics           <NA>      good     70
## 480          No Hair  180.0     Marvel Comics           <NA>       bad     79
## 481     Red / Orange  178.0     Marvel Comics           blue       bad     54
## 482             <NA>     NA     Marvel Comics           <NA>      good     NA
## 483            Black  188.0     Marvel Comics           <NA>      good    125
## 484            Blond  180.0     Marvel Comics           <NA>      good     85
## 485            Blond  168.0     Marvel Comics           <NA>      good    101
## 486             <NA>  168.0          Shueisha           <NA>      good     54
## 487             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 488          No Hair  185.0     Marvel Comics           blue       bad     83
## 489            Black     NA     Marvel Comics           <NA>      good     NA
## 490    Brown / White  185.0     Marvel Comics           <NA>      good     99
## 491           Indigo  175.0     Marvel Comics           <NA>      good     88
## 492            Black  178.0         DC Comics           <NA>      good     79
## 493            Blond     NA      NBC - Heroes           <NA>      good     NA
## 494             <NA>     NA              SyFy           <NA>      good     NA
## 495             <NA>     NA         DC Comics           <NA>      good     NA
## 496            Black  180.0     Marvel Comics           <NA>      good     83
## 497            Brown  185.0     Marvel Comics           <NA>      good     86
## 498              Red  163.0     Marvel Comics           gold      good     59
## 499            White  206.0     Marvel Comics           <NA>      good    293
## 500             <NA>     NA         DC Comics           <NA>      good     NA
## 501            Blond  211.0     Marvel Comics           <NA>       bad    191
## 502            Black  180.0      Team Epic TV           <NA>      good     65
## 503          No Hair  175.0          Shueisha           <NA>      good     69
## 504             <NA>     NA     Marvel Comics           <NA>   neutral     NA
## 505          No Hair  305.0     Marvel Comics           <NA>       bad    405
## 506              Red  178.0         DC Comics           <NA>      good     59
## 507            Brown     NA         DC Comics           <NA>      good     NA
## 508             <NA>     NA      Image Comics           <NA>       bad     NA
## 509            Blond     NA         DC Comics           <NA>       bad     NA
## 510             <NA>     NA         DC Comics           <NA>       bad     NA
## 511             <NA>  170.0     Sony Pictures           <NA>      good    117
## 512             <NA>     NA     Marvel Comics           <NA>      good     NA
## 513             <NA>     NA     Marvel Comics           <NA>      good     NA
## 514            Blond  183.0     Marvel Comics           <NA>      good     89
## 515            Black  157.0         DC Comics           <NA>       bad     79
## 516             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 517             <NA>     NA         DC Comics           <NA>      good     NA
## 518            Black  168.0         DC Comics           <NA>      good     54
## 519              Red  168.0     Marvel Comics           <NA>      good     52
## 520             Grey  183.0     Marvel Comics           <NA>       bad     87
## 521             <NA>     NA         DC Comics           <NA>      good     NA
## 522            Black  185.0         DC Comics           <NA>      good     80
## 523              Red  168.0         DC Comics           <NA>       bad     55
## 524              Red  168.0         DC Comics          green       bad     50
## 525            Green  170.0     Marvel Comics           <NA>      good     52
## 526            blond  180.0         DC Comics           <NA>      good     81
## 527             <NA>     NA     Marvel Comics           <NA>      good     NA
## 528             <NA>  213.0 Dark Horse Comics           <NA>       bad    234
## 529          No Hair  183.0     Marvel Comics           <NA>      good     86
## 530 Strawberry Blond  180.0         DC Comics           <NA>       bad     81
## 531            Blond     NA     Marvel Comics           <NA>       bad     NA
## 532           Purple  180.0     Marvel Comics           <NA>      good     70
## 533            Black  183.0     Marvel Comics           <NA>      good     90
## 534           Purple  180.0     Marvel Comics         purple       bad     74
## 535            Blond  178.0     Marvel Comics           <NA>       bad     68
## 536             <NA>     NA         Star Trek           <NA>      <NA>     NA
## 537             <NA>     NA     HarperCollins           <NA>      good     NA
## 538            Blond  188.0         DC Comics           <NA>      good     83
## 539           Silver  183.0     Marvel Comics           <NA>      good     79
## 540            Brown  163.0     Marvel Comics           <NA>      good     56
## 541             Grey  193.0         DC Comics           <NA>       bad     97
## 542             <NA>     NA              SyFy           <NA>      good     NA
## 543            Black  178.0              <NA>           <NA>      good     83
## 544          No Hair     NA    IDW Publishing          green      good     NA
## 545            Black  165.0         DC Comics           <NA>   neutral     50
## 546              Red  178.0         DC Comics           <NA>      good     70
## 547          No Hair  191.0     Marvel Comics           <NA>       bad    117
## 548              Red  180.0         DC Comics           <NA>      good     83
## 549            Black  183.0         DC Comics           <NA>   neutral     81
## 550            Black  213.0     Marvel Comics            red   neutral    630
## 551             <NA>     NA       Icon Comics           <NA>       bad     NA
## 552            Black  165.0         DC Comics           <NA>      good     56
## 553          No Hair  188.0     Marvel Comics           <NA>       bad    108
## 554          No Hair  185.0         DC Comics           <NA>      good    146
## 555             <NA>     NA      Image Comics           <NA>       bad     NA
## 556             <NA>     NA      Image Comics           <NA>       bad     NA
## 557             <NA>     NA     HarperCollins           <NA>      good     NA
## 558            Brown  297.0      George Lucas           <NA>      good     NA
## 559            Brown  196.0     Marvel Comics           <NA>       bad    320
## 560            Brown  185.0         DC Comics           <NA>       bad     85
## 561             <NA>     NA         DC Comics           <NA>       bad     NA
## 562            Blond     NA         DC Comics           <NA>      good     NA
## 563            Black  180.0     Marvel Comics           <NA>      good     72
## 564            Black  178.0         DC Comics           <NA>      good     79
## 565              Red  183.0         DC Comics           <NA>      good    101
## 566            Black  165.0         DC Comics           <NA>      good     56
## 567            Black  137.0         DC Comics           <NA>      good     38
## 568              Red     NA         DC Comics           <NA>   neutral     NA
## 569            Brown  122.0     Marvel Comics           <NA>      good     25
## 570    Brown / White  173.0     Marvel Comics           <NA>      good     54
## 571            Blond  191.0     Marvel Comics           <NA>      good    104
## 572              Red  168.0         DC Comics           <NA>      good     63
## 573            Blond  198.0     Marvel Comics           <NA>       bad    171
## 574            Black  170.0     Marvel Comics           <NA>      good     61
## 575            Brown  185.0     Marvel Comics           <NA>   neutral    203
## 576           Orange  305.0     Marvel Comics           <NA>      good    900
## 577             <NA>  279.0  J. R. R. Tolkien           <NA>       bad     NA
## 578             <NA>     NA      Image Comics           <NA>      good     NA
## 579            Brown  183.0         DC Comics           <NA>       bad     63
## 580            Blond  178.0     Marvel Comics           <NA>      good     74
## 581            Brown  193.0     Marvel Comics           <NA>      good    113
## 582            Brown  170.0     Marvel Comics           <NA>       bad     59
## 583              Red     NA     Marvel Comics           <NA>       bad     NA
## 584            Brown  211.0     Marvel Comics           <NA>       bad    310
## 585             <NA>     NA     Marvel Comics           <NA>       bad     NA
## 586            Blond  188.0     Marvel Comics           <NA>   neutral     87
## 587             <NA>  185.0     Marvel Comics           <NA>      good    149
## 588            Black  173.0         DC Comics           blue      good     54
## 589            Brown  168.0     Marvel Comics           <NA>      good     50
## 590            Black  178.0     Marvel Comics           <NA>      good     79
## 591              Red  191.0     Marvel Comics           <NA>      good     88
## 592            Green  201.0     Marvel Comics           <NA>      good    315
## 593          No Hair  183.0     Marvel Comics           <NA>      good    153
## 594            Brown  175.0     Marvel Comics           <NA>       bad     79
## 595            Black  173.0     Marvel Comics           <NA>      good     52
## 596             <NA>     NA         DC Comics           <NA>      good     NA
## 597            Black  188.0     Marvel Comics           <NA>      good    191
## 598            Black     NA     Marvel Comics           <NA>      good     NA
## 599             <NA>     NA         DC Comics           <NA>      good     NA
## 600             <NA>     NA         DC Comics           <NA>      good     NA
## 601          No Hair  193.0     Marvel Comics         silver      good    101
## 602            Black  157.0     Marvel Comics           <NA>      good     50
## 603            Black     NA         DC Comics           <NA>      good     NA
## 604            Black  201.0         DC Comics            red   neutral     92
## 605           Purple  175.0         DC Comics           <NA>       bad     72
## 606             <NA>     NA         DC Comics           <NA>       bad     NA
## 607 Strawberry Blond  168.0     Marvel Comics           <NA>       bad     52
## 608            Black  198.0     Marvel Comics           <NA>      good    180
## 609             <NA>     NA     Marvel Comics           <NA>       bad     NA
## 610            Blond  178.0     Marvel Comics           <NA>      good     49
## 611          No Hair     NA         DC Comics           <NA>      good     NA
## 612            White  279.0         DC Comics           <NA>       bad    437
## 613      Red / White  165.0     Marvel Comics           <NA>      good     65
## 614             <NA>  188.0         DC Comics           <NA>      good    113
## 615            Black  211.0      Image Comics           <NA>      good    405
## 616          No Hair     NA         DC Comics          white      good     NA
## 617             <NA>     NA     Marvel Comics           <NA>      good     NA
## 618             <NA>     NA         DC Comics           <NA>      good     NA
## 619            Brown     NA         DC Comics           <NA>      good     NA
## 620             <NA>     NA     Marvel Comics           <NA>       bad     NA
## 621            Brown  170.0     Marvel Comics           <NA>      good     54
## 622            Blond  165.0     Marvel Comics           <NA>      good     56
## 623            Brown  178.0     Marvel Comics           <NA>      good     74
## 624            Brown  178.0     Marvel Comics           <NA>      good     77
## 625            Black  157.0     Marvel Comics           <NA>      good     56
## 626            Black  178.0     Marvel Comics           <NA>      good     59
## 627             <NA>     NA     Marvel Comics           <NA>      good     NA
## 628            Brown  173.0     Marvel Comics           <NA>      good     55
## 629            White  178.0     Marvel Comics           <NA>       bad     58
## 630            Black  185.0         Star Trek           <NA>      good     81
## 631            Blond  183.0     Marvel Comics           <NA>      good     83
## 632             <NA>     NA     Marvel Comics           <NA>      good     NA
## 633            Blond  188.0     Marvel Comics           <NA>      good     79
## 634             <NA>     NA     Marvel Comics           <NA>      good     NA
## 635           Auburn  193.0         DC Comics         orange      good     71
## 636            Blond  165.0         DC Comics           <NA>      good     62
## 637            Black  170.0         DC Comics           <NA>      good     63
## 638          No Hair  201.0         DC Comics           <NA>      good    131
## 639            Blond     NA       ABC Studios           <NA>      good     NA
## 640            Black  183.0         DC Comics          white       bad     91
## 641            White  180.0     Marvel Comics           <NA>      good     57
## 642             <NA>  183.0      George Lucas           <NA>       bad     NA
## 643            black  173.0     Marvel Comics           <NA>      good     77
## 644            Black  170.0         DC Comics           <NA>      good     68
## 645     Black / Blue  180.0         DC Comics           <NA>       bad     77
## 646            Blond  165.0         DC Comics           <NA>      good     54
## 647            Black  191.0         DC Comics           <NA>      good    101
## 648          No Hair     NA         DC Comics          green       bad     NA
## 649          No Hair  196.0     Marvel Comics         yellow       bad     47
## 650             <NA>     NA      NBC - Heroes           <NA>       bad     NA
## 651            Black  180.0     Marvel Comics           <NA>      good     74
## 652             <NA>  183.0 Dark Horse Comics         silver       bad    146
## 653             <NA>     NA Dark Horse Comics           <NA>       bad    176
## 654             <NA>     NA Dark Horse Comics           <NA>       bad    198
## 655             <NA>     NA Dark Horse Comics         silver       bad    149
## 656            Brown  188.0     Marvel Comics           <NA>       bad     99
## 657            Black  163.0     Marvel Comics           <NA>      good     54
## 658          No Hair  201.0     Marvel Comics         purple       bad    443
## 659             <NA>     NA              <NA>           <NA>      good     NA
## 660            Black  188.0         DC Comics           <NA>   neutral    101
## 661          No Hair  183.0     Marvel Comics           <NA>      good    225
## 662            Blond  198.0     Marvel Comics           <NA>      good    288
## 663            Blond  175.0     Marvel Comics           <NA>      good    143
## 664            Black  185.0     Marvel Comics           <NA>      good    101
## 665             <NA>     NA     Marvel Comics           <NA>      good     NA
## 666            Black  175.0     Marvel Comics           <NA>      good     74
## 667            Blond  198.0     Marvel Comics           <NA>      good    288
## 668              Red  218.0     Marvel Comics           <NA>      good    158
## 669          No Hair  185.0     Marvel Comics           grey       bad    203
## 670           Auburn  178.0     Marvel Comics           <NA>      good     81
## 671            White  163.0     Marvel Comics           <NA>       bad     54
## 672             <NA>     NA     HarperCollins           <NA>      good     NA
## 673            Brown  175.0     Marvel Comics          green   neutral     76
## 674            Brown  188.0     Marvel Comics           <NA>      good     97
## 675            Blond  191.0     Marvel Comics           <NA>      good    117
## 676             <NA>     NA      NBC - Heroes           <NA>      good     NA
## 677            Blond  183.0         DC Comics           <NA>      <NA>     81
## 678            Black     NA         DC Comics            red       bad     NA
## 679            Brown  168.0         DC Comics           <NA>      good     59
## 680          No Hair  188.0     Marvel Comics          green      good     86
## 681             <NA>  183.0         DC Comics           <NA>       bad     82
## 682            Blond  168.0     Marvel Comics           <NA>      good    105
## 683             <NA>  206.0     Marvel Comics         silver       bad    331
## 684            White   15.2     Marvel Comics           <NA>       bad     58
## 685 Strawberry Blond  168.0     Marvel Comics           <NA>      good     54
## 686            Black  175.0      Team Epic TV           <NA>      good     56
## 687            Blond  191.0     Marvel Comics           <NA>      good    214
## 688          No Hair  165.0     Marvel Comics           <NA>       bad     79
## 689            Black  168.0          Shueisha           <NA>       bad     73
## 690 Strawberry Blond  191.0     Marvel Comics           <NA>       bad    117
## 691            Black  175.0     Marvel Comics           <NA>       bad     50
## 692            Brown  229.0     Marvel Comics           <NA>       bad    334
## 693             <NA>  226.0     Marvel Comics           <NA>      <NA>     NA
## 694           Silver  168.0     Marvel Comics           <NA>      good     52
## 695            Black  178.0         DC Comics           <NA>      good     71
## 696              Red  165.0     Marvel Comics           <NA>      good     54
## 697             <NA>     NA     Marvel Comics           <NA>      good     NA
## 698             <NA>     NA      Image Comics           <NA>       bad     NA
## 699            Black  137.0 Dark Horse Comics           <NA>      good     41
## 700          No Hair  191.0     Marvel Comics            red      good    135
## 701          No Hair  191.0     Marvel Comics           <NA>      good    135
## 702            Black  175.0         DC Comics           <NA>      good     63
## 703            Black     NA     Marvel Comics           <NA>      good     NA
## 704          No Hair  180.0     Marvel Comics           <NA>       bad     79
## 705            Black  183.0     Marvel Comics           <NA>       bad    162
## 706            Brown  185.0     Marvel Comics           <NA>      good     95
## 707            Blond  180.0     Marvel Comics           <NA>      good     54
## 708            Blond  188.0     Marvel Comics           <NA>      good    108
## 709            Black  173.0         DC Comics           <NA>       bad     67
## 710            Black  218.0     Marvel Comics           <NA>      good    158
## 711           Auburn  163.0     Marvel Comics           <NA>      good     50
## 712             <NA>     NA     Marvel Comics           <NA>      good     NA
## 713             <NA>     NA     Marvel Comics           <NA>       bad     NA
## 714            Black     NA         DC Comics           <NA>       bad     NA
## 715            Blond  178.0     Marvel Comics           <NA>      good     65
## 716             <NA>     NA         DC Comics           <NA>      good     NA
## 717            Brown  175.0     Marvel Comics           <NA>      good    117
## 718            Black  140.0     Marvel Comics           <NA>      good     39
## 719           Auburn  366.0     Marvel Comics           <NA>      good    473
## 720            Black  160.0     Marvel Comics           <NA>      good    135
## 721            Blond  165.0         DC Comics           <NA>      good     51
## 722            Black  188.0     Marvel Comics           <NA>      good    171
## 723            Black  183.0         DC Comics           <NA>      good     74
## 724             <NA>     NA     Marvel Comics           <NA>      good     NA
## 725            Black  196.0     Marvel Comics           <NA>      good    117
## 726            Black  155.0     Marvel Comics           <NA>      good     50
## 727            Brown  175.0     Marvel Comics           <NA>      good     61
## 728          No Hair  188.0     Marvel Comics           <NA>       bad     95
## 729            Blond  183.0     Marvel Comics           <NA>      good     83
## 730 Strawberry Blond  165.0     Marvel Comics           <NA>      good     52
## 731          No Hair  304.8     Marvel Comics          white      good     NA
## 732            White   66.0      George Lucas          green      good     17
## 733            Black  170.0         DC Comics           <NA>      good     57
## 734            Brown  185.0         DC Comics           <NA>       bad     81
##     height_to_weight
## 1         0.46031746
## 2         2.93846154
## 3         2.05555556
## 4         0.46031746
## 5                 NA
## 6         1.58196721
## 7                 NA
## 8         2.10227273
## 9         2.83606557
## 10        2.19753086
## 11        1.83653846
## 12        1.74074074
## 13        2.14444444
## 14        2.00000000
## 15                NA
## 16                NA
## 17        2.47222222
## 18        1.44378698
## 19                NA
## 20        1.48554913
## 21        1.86138614
## 22                NA
## 23        2.69117647
## 24                NA
## 25        2.89473684
## 26        3.01851852
## 27                NA
## 28        2.20481928
## 29        2.00000000
## 30        1.72950820
## 31        2.12790698
## 32                NA
## 33                NA
## 34        0.63966480
## 35        1.57777778
## 36                NA
## 37        1.67924528
## 38        1.26712329
## 39        2.77777778
## 40        2.69117647
## 41        3.03508772
## 42        1.96938776
## 43        0.68518519
## 44        2.79661017
## 45        3.26000000
## 46                NA
## 47                NA
## 48        1.81188119
## 49        1.57142857
## 50        2.61764706
## 51                NA
## 52        3.11111111
## 53        2.25925926
## 54                NA
## 55                NA
## 56        2.85714286
## 57        2.73134328
## 58                NA
## 59                NA
## 60        1.12777778
## 61        2.37662338
## 62        3.05555556
## 63                NA
## 64        2.98245614
## 65                NA
## 66        3.17307692
## 67                NA
## 68        2.75409836
## 69        1.97894737
## 70        2.31168831
## 71        2.25316456
## 72        1.48872180
## 73                NA
## 74        2.77777778
## 75        0.99447514
## 76        2.54411765
## 77                NA
## 78                NA
## 79        0.93055556
## 80                NA
## 81        1.39259259
## 82                NA
## 83        2.32394366
## 84                NA
## 85                NA
## 86        3.33333333
## 87                NA
## 88                NA
## 89                NA
## 90                NA
## 91                NA
## 92        1.59677419
## 93        1.23225806
## 94                NA
## 95        1.69026549
## 96        1.97894737
## 97        2.84482759
## 98        2.88135593
## 99        3.29629630
## 100               NA
## 101               NA
## 102       2.12790698
## 103       2.05555556
## 104       3.26923077
## 105       2.04347826
## 106       2.03333333
## 107       2.88135593
## 108       2.78688525
## 109       1.83653846
## 110       2.15116279
## 111       2.13636364
## 112       1.93814433
## 113               NA
## 114       2.47058824
## 115       2.94642857
## 116               NA
## 117               NA
## 118       2.27272727
## 119       0.77391304
## 120       0.44040404
## 121               NA
## 122               NA
## 123               NA
## 124               NA
## 125       2.12790698
## 126               NA
## 127               NA
## 128               NA
## 129               NA
## 130       3.00000000
## 131               NA
## 132       2.02061856
## 133               NA
## 134       1.75454545
## 135               NA
## 136       1.46666667
## 137       2.78688525
## 138       1.84848485
## 139               NA
## 140       3.01923077
## 141       2.03333333
## 142       2.88135593
## 143               NA
## 144               NA
## 145       1.28481013
## 146       2.36486486
## 147               NA
## 148       2.25925926
## 149       1.74074074
## 150       2.14444444
## 151       1.70689655
## 152               NA
## 153               NA
## 154               NA
## 155       1.74074074
## 156       2.43243243
## 157       1.91089109
## 158       2.36486486
## 159               NA
## 160               NA
## 161               NA
## 162       2.15116279
## 163       2.83606557
## 164               NA
## 165       2.86885246
## 166       2.74193548
## 167       2.07216495
## 168               NA
## 169       2.77777778
## 170               NA
## 171       2.22222222
## 172       3.26000000
## 173       3.09090909
## 174       3.24074074
## 175       2.15116279
## 176               NA
## 177       1.07647059
## 178               NA
## 179               NA
## 180       3.22857143
## 181       2.28205128
## 182               NA
## 183               NA
## 184               NA
## 185       1.00444444
## 186       2.73134328
## 187       2.41772152
## 188       1.84848485
## 189               NA
## 190       1.73076923
## 191       3.36000000
## 192               NA
## 193               NA
## 194       1.14450867
## 195               NA
## 196       2.17045455
## 197       2.57352941
## 198       3.17307692
## 199               NA
## 200               NA
## 201       2.03333333
## 202       2.28395062
## 203               NA
## 204       0.32680539
## 205               NA
## 206       3.00000000
## 207               NA
## 208       1.46666667
## 209       4.51851852
## 210               NA
## 211       3.32692308
## 212       2.03333333
## 213       1.97894737
## 214       2.03296703
## 215       1.08426966
## 216       1.91089109
## 217       1.94736842
## 218       0.49086162
## 219       2.14444444
## 220               NA
## 221       1.15789474
## 222       1.07486631
## 223       1.52272727
## 224       2.11235955
## 225       1.59090909
## 226       2.32098765
## 227       3.20370370
## 228               NA
## 229       2.77777778
## 230       0.59223301
## 231       1.88461538
## 232               NA
## 233               NA
## 234       0.63071895
## 235               NA
## 236       3.00000000
## 237       2.43243243
## 238       2.96610169
## 239               NA
## 240       2.31250000
## 241       2.73846154
## 242       2.94736842
## 243               NA
## 244               NA
## 245               NA
## 246       0.95073892
## 247       1.97894737
## 248       1.80188679
## 249       2.07954545
## 250       2.04166667
## 251       1.74074074
## 252               NA
## 253               NA
## 254       3.50000000
## 255               NA
## 256      54.16666667
## 257       2.94642857
## 258       1.94949495
## 259       3.08928571
## 260               NA
## 261       2.06593407
## 262               NA
## 263       2.22222222
## 264               NA
## 265       2.07954545
## 266       2.12790698
## 267       3.01923077
## 268       2.25925926
## 269       3.15555556
## 270       2.04347826
## 271       2.02884615
## 272       1.07784431
## 273      54.75000000
## 274       2.28395062
## 275       2.37662338
## 276               NA
## 277               NA
## 278               NA
## 279       2.15116279
## 280       1.89898990
## 281               NA
## 282               NA
## 283               NA
## 284       0.09920635
## 285       0.73880597
## 286       3.36000000
## 287               NA
## 288               NA
## 289       2.82258065
## 290               NA
## 291               NA
## 292               NA
## 293       2.03333333
## 294       0.73333333
## 295       1.54782609
## 296       2.25316456
## 297               NA
## 298       2.13636364
## 299       2.16867470
## 300       2.31168831
## 301       2.07954545
## 302       2.25316456
## 303     175.25000000
## 304               NA
## 305       1.97894737
## 306       2.08888889
## 307       2.31645570
## 308               NA
## 309       2.69841270
## 310               NA
## 311       2.31645570
## 312       2.07865169
## 313       1.83653846
## 314       2.89473684
## 315       2.86885246
## 316       2.10227273
## 317       3.24074074
## 318               NA
## 319       2.61538462
## 320       2.22222222
## 321       0.94666667
## 322       1.63924051
## 323       2.83606557
## 324       2.28395062
## 325       1.34246575
## 326               NA
## 327               NA
## 328       2.16867470
## 329               NA
## 330       3.50000000
## 331       4.38888889
## 332       0.38730159
## 333       2.31168831
## 334       3.05084746
## 335       2.93103448
## 336       2.27272727
## 337       1.57983193
## 338       0.88405797
## 339       2.66153846
## 340       2.61538462
## 341       2.31645570
## 342               NA
## 343       2.22222222
## 344       3.11111111
## 345       2.27848101
## 346       1.03664921
## 347               NA
## 348               NA
## 349               NA
## 350       1.96202532
## 351       5.07142857
## 352               NA
## 353       2.31168831
## 354               NA
## 355               NA
## 356       3.23076923
## 357               NA
## 358       3.05454545
## 359               NA
## 360               NA
## 361       3.03571429
## 362               NA
## 363       1.66371681
## 364               NA
## 365               NA
## 366               NA
## 367               NA
## 368       2.05555556
## 369       2.07954545
## 370       2.27906977
## 371       3.36734694
## 372       3.17307692
## 373               NA
## 374       0.33567251
## 375               NA
## 376       2.19753086
## 377               NA
## 378               NA
## 379       1.83653846
## 380       2.40277778
## 381               NA
## 382               NA
## 383               NA
## 384               NA
## 385               NA
## 386               NA
## 387       0.68539326
## 388               NA
## 389       0.72222222
## 390               NA
## 391               NA
## 392       0.99014778
## 393       1.93814433
## 394               NA
## 395       1.92929293
## 396       1.72641509
## 397       3.55555556
## 398       2.27848101
## 399               NA
## 400               NA
## 401       3.01724138
## 402       2.82539683
## 403               NA
## 404       2.96610169
## 405               NA
## 406       1.97894737
## 407       3.05555556
## 408       2.38461538
## 409       2.01052632
## 410       0.55000000
## 411               NA
## 412               NA
## 413       0.88260870
## 414       0.79513889
## 415       0.81779661
## 416       5.22222222
## 417       1.03664921
## 418               NA
## 419       2.18181818
## 420               NA
## 421               NA
## 422       2.27848101
## 423       0.47780679
## 424       2.18604651
## 425               NA
## 426               NA
## 427               NA
## 428               NA
## 429       0.94666667
## 430       2.08888889
## 431       1.93814433
## 432       3.23076923
## 433       1.48888889
## 434       3.03571429
## 435       2.25925926
## 436               NA
## 437               NA
## 438               NA
## 439       1.75454545
## 440       2.50000000
## 441               NA
## 442       3.05084746
## 443       3.05555556
## 444       1.41428571
## 445       2.43055556
## 446       2.17777778
## 447       2.05555556
## 448               NA
## 449       2.15116279
## 450               NA
## 451               NA
## 452       2.37662338
## 453       1.86138614
## 454               NA
## 455               NA
## 456       2.91803279
## 457       2.28395062
## 458       2.12790698
## 459               NA
## 460               NA
## 461       1.53125000
## 462               NA
## 463       2.86885246
## 464       1.08284024
## 465               NA
## 466               NA
## 467               NA
## 468       0.79032258
## 469       2.14444444
## 470               NA
## 471       1.86138614
## 472       3.05084746
## 473       2.37974684
## 474       2.25316456
## 475       2.43055556
## 476       2.68571429
## 477       1.27215190
## 478       2.83606557
## 479       2.57142857
## 480       2.27848101
## 481       3.29629630
## 482               NA
## 483       1.50400000
## 484       2.11764706
## 485       1.66336634
## 486       3.11111111
## 487               NA
## 488       2.22891566
## 489               NA
## 490       1.86868687
## 491       1.98863636
## 492       2.25316456
## 493               NA
## 494               NA
## 495               NA
## 496       2.16867470
## 497       2.15116279
## 498       2.76271186
## 499       0.70307167
## 500               NA
## 501       1.10471204
## 502       2.76923077
## 503       2.53623188
## 504               NA
## 505       0.75308642
## 506       3.01694915
## 507               NA
## 508               NA
## 509               NA
## 510               NA
## 511       1.45299145
## 512               NA
## 513               NA
## 514       2.05617978
## 515       1.98734177
## 516               NA
## 517               NA
## 518       3.11111111
## 519       3.23076923
## 520       2.10344828
## 521               NA
## 522       2.31250000
## 523       3.05454545
## 524       3.36000000
## 525       3.26923077
## 526       2.22222222
## 527               NA
## 528       0.91025641
## 529       2.12790698
## 530       2.22222222
## 531               NA
## 532       2.57142857
## 533       2.03333333
## 534       2.43243243
## 535       2.61764706
## 536               NA
## 537               NA
## 538       2.26506024
## 539       2.31645570
## 540       2.91071429
## 541       1.98969072
## 542               NA
## 543       2.14457831
## 544               NA
## 545       3.30000000
## 546       2.54285714
## 547       1.63247863
## 548       2.16867470
## 549       2.25925926
## 550       0.33809524
## 551               NA
## 552       2.94642857
## 553       1.74074074
## 554       1.26712329
## 555               NA
## 556               NA
## 557               NA
## 558               NA
## 559       0.61250000
## 560       2.17647059
## 561               NA
## 562               NA
## 563       2.50000000
## 564       2.25316456
## 565       1.81188119
## 566       2.94642857
## 567       3.60526316
## 568               NA
## 569       4.88000000
## 570       3.20370370
## 571       1.83653846
## 572       2.66666667
## 573       1.15789474
## 574       2.78688525
## 575       0.91133005
## 576       0.33888889
## 577               NA
## 578               NA
## 579       2.90476190
## 580       2.40540541
## 581       1.70796460
## 582       2.88135593
## 583               NA
## 584       0.68064516
## 585               NA
## 586       2.16091954
## 587       1.24161074
## 588       3.20370370
## 589       3.36000000
## 590       2.25316456
## 591       2.17045455
## 592       0.63809524
## 593       1.19607843
## 594       2.21518987
## 595       3.32692308
## 596               NA
## 597       0.98429319
## 598               NA
## 599               NA
## 600               NA
## 601       1.91089109
## 602       3.14000000
## 603               NA
## 604       2.18478261
## 605       2.43055556
## 606               NA
## 607       3.23076923
## 608       1.10000000
## 609               NA
## 610       3.63265306
## 611               NA
## 612       0.63844394
## 613       2.53846154
## 614       1.66371681
## 615       0.52098765
## 616               NA
## 617               NA
## 618               NA
## 619               NA
## 620               NA
## 621       3.14814815
## 622       2.94642857
## 623       2.40540541
## 624       2.31168831
## 625       2.80357143
## 626       3.01694915
## 627               NA
## 628       3.14545455
## 629       3.06896552
## 630       2.28395062
## 631       2.20481928
## 632               NA
## 633       2.37974684
## 634               NA
## 635       2.71830986
## 636       2.66129032
## 637       2.69841270
## 638       1.53435115
## 639               NA
## 640       2.01098901
## 641       3.15789474
## 642               NA
## 643       2.24675325
## 644       2.50000000
## 645       2.33766234
## 646       3.05555556
## 647       1.89108911
## 648               NA
## 649       4.17021277
## 650               NA
## 651       2.43243243
## 652       1.25342466
## 653               NA
## 654               NA
## 655               NA
## 656       1.89898990
## 657       3.01851852
## 658       0.45372460
## 659               NA
## 660       1.86138614
## 661       0.81333333
## 662       0.68750000
## 663       1.22377622
## 664       1.83168317
## 665               NA
## 666       2.36486486
## 667       0.68750000
## 668       1.37974684
## 669       0.91133005
## 670       2.19753086
## 671       3.01851852
## 672               NA
## 673       2.30263158
## 674       1.93814433
## 675       1.63247863
## 676               NA
## 677       2.25925926
## 678               NA
## 679       2.84745763
## 680       2.18604651
## 681       2.23170732
## 682       1.60000000
## 683       0.62235650
## 684       0.26206897
## 685       3.11111111
## 686       3.12500000
## 687       0.89252336
## 688       2.08860759
## 689       2.30136986
## 690       1.63247863
## 691       3.50000000
## 692       0.68562874
## 693               NA
## 694       3.23076923
## 695       2.50704225
## 696       3.05555556
## 697               NA
## 698               NA
## 699       3.34146341
## 700       1.41481481
## 701       1.41481481
## 702       2.77777778
## 703               NA
## 704       2.27848101
## 705       1.12962963
## 706       1.94736842
## 707       3.33333333
## 708       1.74074074
## 709       2.58208955
## 710       1.37974684
## 711       3.26000000
## 712               NA
## 713               NA
## 714               NA
## 715       2.73846154
## 716               NA
## 717       1.49572650
## 718       3.58974359
## 719       0.77378436
## 720       1.18518519
## 721       3.23529412
## 722       1.09941520
## 723       2.47297297
## 724               NA
## 725       1.67521368
## 726       3.10000000
## 727       2.86885246
## 728       1.97894737
## 729       2.20481928
## 730       3.17307692
## 731               NA
## 732       3.88235294
## 733       2.98245614
## 734       2.28395062
```

```r
max(height_to_weight,na.rm=T)
```

```
## [1] 175.25
```

```r
filter(superhero_info,height_to_weight==175.25)
```

```
##    Name Gender Eye color           Race Hair color Height     Publisher
## 1 Groot   Male    yellow Flora Colossus       <NA>    701 Marvel Comics
##   Skin color Alignment Weight height_to_weight
## 1       <NA>      good      4           175.25
```

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                   <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Abin …
## $ agility                      <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE, F…
## $ accelerated_healing          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE, FA…
## $ lantern_power_ring           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALSE, …
## $ dimensional_awareness        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ cold_resistance              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ durability                   <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, T…
## $ stealth                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ flight                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ danger_sense                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ underwater_breathing         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ marksmanship                 <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ weapons_master               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ power_augmentation           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animal_attributes            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ longevity                    <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, F…
## $ intelligence                 <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, FA…
## $ super_strength               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, TRUE…
## $ cryokinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ telepathy                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ energy_armor                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_blasts                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ duplication                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ size_changing                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ density_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ stamina                      <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, FAL…
## $ astral_travel                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ audio_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ dexterity                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnitrix                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ super_speed                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE, FA…
## $ possession                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animal_oriented_powers       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ weapon_based_powers          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ electrokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ darkforce_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ death_touch                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ teleportation                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ enhanced_senses              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ telekinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_beams                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ magic                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ hyperkinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ jump                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ clairvoyance                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ dimensional_travel           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ power_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ shapeshifting                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ peak_human_condition         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ immortality                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE, F…
## $ camouflage                   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, …
## $ element_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ phasing                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ astral_projection            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ electrical_transport         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ fire_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ projection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ summoning                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_memory              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ reflexes                     <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ invulnerability              <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE, T…
## $ energy_constructs            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ force_fields                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ self_sustenance              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, …
## $ anti_gravity                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ empathy                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_nullifier              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ radiation_control            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ psionic_powers               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ elasticity                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ substance_secretion          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ elemental_transmogrification <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ technopath_cyberpath         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ photographic_reflexes        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ seismic_power                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animation                    <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, …
## $ precognition                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ mind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ fire_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_absorption             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_hearing             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ nova_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ insanity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ hypnokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animal_control               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ natural_armor                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ intangibility                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_sight               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ molecular_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ heat_generation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ adaptation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ gliding                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_suit                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ mind_blast                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ probability_manipulation     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ gravity_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ regeneration                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ light_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ echolocation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ levitation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ toxin_and_disease_control    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ banish                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_manipulation          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ heat_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ natural_weapons              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ time_travel                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_smell               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ illusions                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ thirstokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ hair_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ illumination                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnipotent                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ cloaking                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ changing_armor               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_cosmic                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ biokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ water_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ radiation_immunity           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_telescopic            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ toxin_and_disease_resistance <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ spatial_awareness            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_resistance            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ telepathy_resistance         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ molecular_combustion         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnilingualism               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ portal_creation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ magnetism                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ mind_control_resistance      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ plant_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ sonar                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ sonic_scream                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ time_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_touch               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ magic_resistance             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ invisibility                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ sub_mariner                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ radiation_absorption         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ intuitive_aptitude           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_microscopic           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ melting                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ wind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ super_breath                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, …
## $ wallcrawling                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_night                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_infrared              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ grim_reaping                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ matter_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ the_force                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ resurrection                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ terrakinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_heat                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vitakinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ radar_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ qwardian_power_ring          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ weather_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_x_ray                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_thermal               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ web_creation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ reality_warping              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ odin_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ symbiote_costume             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ speed_force                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ phoenix_force                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ molecular_dissipation        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_cryo                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnipresent                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omniscient                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
superhero_powers %>%
  select(hero_names,durability,accelerated_healing,super_strength)%>%
  filter(durability=="TRUE")%>%
  filter(accelerated_healing=="TRUE")%>%
  filter(super_strength=="TRUE")
```

```
## # A tibble: 97 × 4
##    hero_names   durability accelerated_healing super_strength
##    <chr>        <lgl>      <lgl>               <lgl>         
##  1 A-Bomb       TRUE       TRUE                TRUE          
##  2 Abe Sapien   TRUE       TRUE                TRUE          
##  3 Angel        TRUE       TRUE                TRUE          
##  4 Anti-Monitor TRUE       TRUE                TRUE          
##  5 Anti-Venom   TRUE       TRUE                TRUE          
##  6 Aquaman      TRUE       TRUE                TRUE          
##  7 Arachne      TRUE       TRUE                TRUE          
##  8 Archangel    TRUE       TRUE                TRUE          
##  9 Ardina       TRUE       TRUE                TRUE          
## 10 Ares         TRUE       TRUE                TRUE          
## # … with 87 more rows
```

## Your Favorite
15. Pick your favorite superhero and let's see their powers!

```r
myfavorite <- filter(superhero_powers,hero_names=='Groot')
glimpse(myfavorite)
```

```
## Rows: 1
## Columns: 168
## $ hero_names                   <chr> "Groot"
## $ agility                      <lgl> FALSE
## $ accelerated_healing          <lgl> FALSE
## $ lantern_power_ring           <lgl> FALSE
## $ dimensional_awareness        <lgl> FALSE
## $ cold_resistance              <lgl> FALSE
## $ durability                   <lgl> TRUE
## $ stealth                      <lgl> FALSE
## $ energy_absorption            <lgl> FALSE
## $ flight                       <lgl> FALSE
## $ danger_sense                 <lgl> FALSE
## $ underwater_breathing         <lgl> FALSE
## $ marksmanship                 <lgl> FALSE
## $ weapons_master               <lgl> FALSE
## $ power_augmentation           <lgl> FALSE
## $ animal_attributes            <lgl> FALSE
## $ longevity                    <lgl> TRUE
## $ intelligence                 <lgl> TRUE
## $ super_strength               <lgl> TRUE
## $ cryokinesis                  <lgl> FALSE
## $ telepathy                    <lgl> FALSE
## $ energy_armor                 <lgl> FALSE
## $ energy_blasts                <lgl> FALSE
## $ duplication                  <lgl> TRUE
## $ size_changing                <lgl> TRUE
## $ density_control              <lgl> FALSE
## $ stamina                      <lgl> TRUE
## $ astral_travel                <lgl> FALSE
## $ audio_control                <lgl> FALSE
## $ dexterity                    <lgl> FALSE
## $ omnitrix                     <lgl> FALSE
## $ super_speed                  <lgl> FALSE
## $ possession                   <lgl> FALSE
## $ animal_oriented_powers       <lgl> FALSE
## $ weapon_based_powers          <lgl> FALSE
## $ electrokinesis               <lgl> FALSE
## $ darkforce_manipulation       <lgl> FALSE
## $ death_touch                  <lgl> FALSE
## $ teleportation                <lgl> FALSE
## $ enhanced_senses              <lgl> FALSE
## $ telekinesis                  <lgl> FALSE
## $ energy_beams                 <lgl> FALSE
## $ magic                        <lgl> FALSE
## $ hyperkinesis                 <lgl> FALSE
## $ jump                         <lgl> FALSE
## $ clairvoyance                 <lgl> FALSE
## $ dimensional_travel           <lgl> FALSE
## $ power_sense                  <lgl> FALSE
## $ shapeshifting                <lgl> FALSE
## $ peak_human_condition         <lgl> FALSE
## $ immortality                  <lgl> FALSE
## $ camouflage                   <lgl> FALSE
## $ element_control              <lgl> FALSE
## $ phasing                      <lgl> FALSE
## $ astral_projection            <lgl> FALSE
## $ electrical_transport         <lgl> FALSE
## $ fire_control                 <lgl> FALSE
## $ projection                   <lgl> FALSE
## $ summoning                    <lgl> FALSE
## $ enhanced_memory              <lgl> FALSE
## $ reflexes                     <lgl> FALSE
## $ invulnerability              <lgl> TRUE
## $ energy_constructs            <lgl> FALSE
## $ force_fields                 <lgl> FALSE
## $ self_sustenance              <lgl> FALSE
## $ anti_gravity                 <lgl> FALSE
## $ empathy                      <lgl> FALSE
## $ power_nullifier              <lgl> FALSE
## $ radiation_control            <lgl> FALSE
## $ psionic_powers               <lgl> FALSE
## $ elasticity                   <lgl> FALSE
## $ substance_secretion          <lgl> FALSE
## $ elemental_transmogrification <lgl> FALSE
## $ technopath_cyberpath         <lgl> FALSE
## $ photographic_reflexes        <lgl> FALSE
## $ seismic_power                <lgl> FALSE
## $ animation                    <lgl> FALSE
## $ precognition                 <lgl> FALSE
## $ mind_control                 <lgl> FALSE
## $ fire_resistance              <lgl> TRUE
## $ power_absorption             <lgl> FALSE
## $ enhanced_hearing             <lgl> FALSE
## $ nova_force                   <lgl> FALSE
## $ insanity                     <lgl> FALSE
## $ hypnokinesis                 <lgl> FALSE
## $ animal_control               <lgl> FALSE
## $ natural_armor                <lgl> FALSE
## $ intangibility                <lgl> FALSE
## $ enhanced_sight               <lgl> FALSE
## $ molecular_manipulation       <lgl> FALSE
## $ heat_generation              <lgl> FALSE
## $ adaptation                   <lgl> FALSE
## $ gliding                      <lgl> FALSE
## $ power_suit                   <lgl> FALSE
## $ mind_blast                   <lgl> FALSE
## $ probability_manipulation     <lgl> FALSE
## $ gravity_control              <lgl> FALSE
## $ regeneration                 <lgl> TRUE
## $ light_control                <lgl> FALSE
## $ echolocation                 <lgl> FALSE
## $ levitation                   <lgl> FALSE
## $ toxin_and_disease_control    <lgl> FALSE
## $ banish                       <lgl> FALSE
## $ energy_manipulation          <lgl> FALSE
## $ heat_resistance              <lgl> FALSE
## $ natural_weapons              <lgl> FALSE
## $ time_travel                  <lgl> FALSE
## $ enhanced_smell               <lgl> FALSE
## $ illusions                    <lgl> FALSE
## $ thirstokinesis               <lgl> FALSE
## $ hair_manipulation            <lgl> FALSE
## $ illumination                 <lgl> FALSE
## $ omnipotent                   <lgl> FALSE
## $ cloaking                     <lgl> FALSE
## $ changing_armor               <lgl> FALSE
## $ power_cosmic                 <lgl> FALSE
## $ biokinesis                   <lgl> FALSE
## $ water_control                <lgl> FALSE
## $ radiation_immunity           <lgl> FALSE
## $ vision_telescopic            <lgl> FALSE
## $ toxin_and_disease_resistance <lgl> FALSE
## $ spatial_awareness            <lgl> FALSE
## $ energy_resistance            <lgl> FALSE
## $ telepathy_resistance         <lgl> FALSE
## $ molecular_combustion         <lgl> FALSE
## $ omnilingualism               <lgl> FALSE
## $ portal_creation              <lgl> FALSE
## $ magnetism                    <lgl> FALSE
## $ mind_control_resistance      <lgl> FALSE
## $ plant_control                <lgl> TRUE
## $ sonar                        <lgl> FALSE
## $ sonic_scream                 <lgl> FALSE
## $ time_manipulation            <lgl> FALSE
## $ enhanced_touch               <lgl> FALSE
## $ magic_resistance             <lgl> FALSE
## $ invisibility                 <lgl> FALSE
## $ sub_mariner                  <lgl> FALSE
## $ radiation_absorption         <lgl> FALSE
## $ intuitive_aptitude           <lgl> FALSE
## $ vision_microscopic           <lgl> FALSE
## $ melting                      <lgl> FALSE
## $ wind_control                 <lgl> FALSE
## $ super_breath                 <lgl> FALSE
## $ wallcrawling                 <lgl> FALSE
## $ vision_night                 <lgl> FALSE
## $ vision_infrared              <lgl> FALSE
## $ grim_reaping                 <lgl> FALSE
## $ matter_absorption            <lgl> TRUE
## $ the_force                    <lgl> FALSE
## $ resurrection                 <lgl> TRUE
## $ terrakinesis                 <lgl> FALSE
## $ vision_heat                  <lgl> FALSE
## $ vitakinesis                  <lgl> FALSE
## $ radar_sense                  <lgl> FALSE
## $ qwardian_power_ring          <lgl> FALSE
## $ weather_control              <lgl> FALSE
## $ vision_x_ray                 <lgl> FALSE
## $ vision_thermal               <lgl> FALSE
## $ web_creation                 <lgl> FALSE
## $ reality_warping              <lgl> FALSE
## $ odin_force                   <lgl> FALSE
## $ symbiote_costume             <lgl> FALSE
## $ speed_force                  <lgl> FALSE
## $ phoenix_force                <lgl> FALSE
## $ molecular_dissipation        <lgl> FALSE
## $ vision_cryo                  <lgl> FALSE
## $ omnipresent                  <lgl> FALSE
## $ omniscient                   <lgl> FALSE
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
