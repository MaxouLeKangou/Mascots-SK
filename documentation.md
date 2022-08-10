<h1 align="center">Mascots-SK - Documentation</h1>
<p align="center">Mascots-SK est un skript permettant d'avoir des mascottes comme sur <a href="https://rinaorc.com/">Rinaorc</a>.</p><br />

## 🏹 Plus d'informations ?
Vous souhaitez avoir des informations sur comment télécharger et installer ce code ?
Rendez vous sur cette page : [README.md](https://github.com/MaxouLeKangou/Mascots-SK/blob/main/README.md)

## 👨‍💻 COMMANDE
- **_Cette commande si dessous, va vous permettre d'afficher le gui des mascottes._**
```
/mascot
```
- **_Cette commande si dessous, va vous permettre d'ajouter / supprimer des mascottes. => SEULEMENT POUR LES OP_**
```
/mascotadm [<give / giveall / delete / deleteall>] [<nom>]
```

## 👨‍💻 MASCOTS_gui
- **_Cette fonction si dessous, va vous permettre d'afficher le gui des mascottes._**
```
function MASCOTS_gui(p: player, page: number, filter-1: number, filter-2: number):
    set {_uuid} to uuid of {_p}
    set {MASCOTS::gui::%{_uuid}%::1} to {_page}
    set {MASCOTS::gui::%{_uuid}%::2} to {_filter-1}
    set {MASCOTS::gui::%{_uuid}%::3} to {_filter-2}


    loop directory {@DIRECTORY}'s files:
        set {_value} to loop-value
        replace all "/home/container/" with "" in {_value}

        load yaml {_value} as {_file}
        {MASCOTS::%{_uuid}%::*} contains skript-yaml value "id" from {_file}:
            set {_loaded} to skull of {_p} named "%skript-yaml value "name" from {_file}%" with nbt "%skript-yaml value "head" from {_file}%"
        else:
            set {_loaded} to gray dye named "%skript-yaml value "name" from {_file}%"
        
        add "{custom:{id:%skript-yaml value "id" from {_file}%}}" to nbt of {_loaded}
        add "{custom:{rarity:%skript-yaml value "rarity" from {_file}%}}" to nbt of {_loaded}
        add "{custom:{payable:%skript-yaml value "payable" from {_file}%}}" to nbt of {_loaded}
        add "{custom:{price:%skript-yaml value "price" from {_file}%}}" to nbt of {_loaded}

        add {_loaded} to {_load::*}
        unload yaml {_file}

    
    open chest with 5 row named "%{@GUI_DISPLAY}% %{_page}%" to {_p}


    {_filter-1} = 0:
        set {_filter-1::*} to {_load::*}
    else:
        loop {_load::*}:
            set {_nbt} to nbt compound of loop-value
            set {_id} to tag "tag;custom;id" of {_nbt}
            {_filter-1} = 1:
                {MASCOTS::%{_uuid}%::*} contains {_id}:
                    add loop-value to {_filter-1::*}
            else:
                {MASCOTS::%{_uuid}%::*} doesn't contain {_id}:
                    add loop-value to {_filter-1::*}

    {_filter-2} != 0:
        loop {_filter-1::*}:
            set {_nbt} to nbt compound of loop-value
            set {_rarity} to tag "tag;custom;rarity" of {_nbt}
            {_rarity} = {_filter-2}:
                add loop-value to {_filter-2::*}
    else:
        set {_filter-2::*} to {_filter-1::*}

    loop (size of {MASCOTS::rarity::*}) times:
        add 1 to {_count}
        loop {_filter-2::*}:
            set {_nbt} to nbt compound of loop-value-2
            set {_rarity} to tag "tag;custom;rarity" of {_nbt}
            {_rarity} = {_count}:
                add loop-value-2 to {_mascots::*}


    set {_slot} to 10
    loop {_mascots::*}:
        loop-index parsed as integer > (21*({_page}-1)):
            set {_nbt} to nbt compound of loop-value
            set {_id} to tag "tag;custom;id" of {_nbt}
            set {_rarity} to tag "tag;custom;rarity" of {_nbt}

            set {_stars} to ""
            loop {_rarity} times:
                set {_stars} to "%{_stars}%&e%{@GUI_RARITY_DISPLAY}%&r"
            loop ((size of {MASCOTS::rarity::*})-{_rarity}) times:
                set {_stars} to "%{_stars}%&7%{@GUI_RARITY_DISPLAY}%&r"

            {MASCOTS::select::%{_uuid}%} = {_id}:
                set slot {_slot} of {_p}'s current inventory to loop-value with lore "&8&oObjet de collection!", "", "&7Faites apparaitre une mascotte", "&7qui vous suivra sur tous les serveurs", "&7de %{@GUI_SERVER_DISPLAY}% !", "", "&7⁃ &fRareté: %{MASCOT::rarity::%{_rarity}%}% %{_stars}%", "" and "&a&lSélectionné!"
            else if {MASCOTS::%{_uuid}%::*} contains {_id}:
                set slot {_slot} of {_p}'s current inventory to loop-value with lore "&8&oObjet de collection!", "", "&7Faites apparaitre une mascotte", "&7qui vous suivra sur tous les serveurs", "&7de %{@GUI_SERVER_DISPLAY}% !", "", "&7⁃ &fRareté: %{MASCOT::rarity::%{_rarity}%}% %{_stars}%", "" and "&a➥ &eCliquez pour sélectionner !"
            else:
                set {_payable} to tag "tag;custom;payable" of {_nbt}
                {_payable} = 1:
                    set {_price} to tag "tag;custom;price" of {_nbt}
                    set {_price} to MASCOTS_convert({_price})
                    set slot {_slot} of {_p}'s current inventory to loop-value with lore "&8&oObjet de collection!", "", "&7Faites apparaitre une mascotte", "&7qui vous suivra sur tous les serveurs", "&7de %{@GUI_SERVER_DISPLAY}% !", "", "&7⁃ &fRareté: %{MASCOT::rarity::%{_rarity}%}% %{_stars}%", "", "&7⁃ &fPrix: &b%{_price}% %{@MONEY_DISPLAY}%", "&7⁃ &fActuellement obtenable: &a&l✔", "" and "&a➥ &eCliquez pour acheter !"
                else:
                    set slot {_slot} of {_p}'s current inventory to loop-value with lore "&8&oObjet de collection!", "", "&7Faites apparaitre une mascotte", "&7qui vous suivra sur tous les serveurs", "&7de %{@GUI_SERVER_DISPLAY}% !", "", "&7⁃ &fRareté: %{MASCOT::rarity::%{_rarity}%}% %{_stars}%", "", "&7⁃ &fActuellement obtenable: &c&l✘", "" and "&cImpossible de loot ou acheter !"
            {_slot} = 16 or 25:
                add 2 to {_slot}
            add 1 to {_slot}
            {_slot} = 35:
                exit loop


    {_filter-1} = 0:
        set {_filter} to "&bTous"
    else if {_filter-1} = 1:
        set {_filter} to "&aDébloqués"
    else:
        set {_filter} to "&cNon Débloqués"
    set slot 36 of {_p}'s current inventory to sign item named "&eFiltre de Possession: %{_filter}%"
    
    {_filter-2} != 0:
        set {_filter} to {MASCOTS::rarity::%{_filter-2}%}
    else:
        set {_filter} to "&bTous"
    set slot 37 of {_p}'s current inventory to emerald named "&eFiltre de Rareté: %{_filter}%"


    set slot 40 of {_p}'s current inventory to oak door item named "&c&nRetour en jeu"
    set slot 44 of {_p}'s current inventory to barrier named "&c&lDéséquiper"

    size of {_mascots::*} > 21*{_page}:
        set slot 41 of {_p}'s current inventory to arrow named "&a&lPage Suivante"
    {_page} > 1:
        set slot 39 of {_p}'s current inventory to arrow named "&a&lPage Précédente"


inventory click:
    name of current inventory contains {@GUI_DISPLAY}:
        cancel event

        
        name of event-item = "&c&nRetour en jeu":
            close player's inventory
        else if name of event-item = "&c&lDéséquiper":
            {MASCOTS::select::%uuid of player%} is set:
                MASCOTS(player, "undefined")
                MASCOTS_gui(player, {MASCOTS::gui::%uuid of player%::1}, {MASCOTS::gui::%uuid of player%::2}, {MASCOTS::gui::%uuid of player%::3})
            else:
                send "&cVous n'avez aucune Mascotte sélectionné !" to player
        

        else if name of event-item = "&a&lPage Suivante":
            MASCOTS_gui(player, {MASCOTS::gui::%uuid of player%::1} + 1, {MASCOTS::gui::%uuid of player%::2}, {MASCOTS::gui::%uuid of player%::3})
        else if name of event-item = "&a&lPage Précédente":
            MASCOTS_gui(player, {MASCOTS::gui::%uuid of player%::1} - 1, {MASCOTS::gui::%uuid of player%::2}, {MASCOTS::gui::%uuid of player%::3})


        else if event-item = sign:
            {MASCOTS::gui::%uuid of player%::2} = 2:
                set {MASCOTS::gui::%uuid of player%::2} to 0
            else:
                set {MASCOTS::gui::%uuid of player%::2} to ({MASCOTS::gui::%uuid of player%::2} + 1)
            MASCOTS_gui(player, 1, {MASCOTS::gui::%uuid of player%::2}, {MASCOTS::gui::%uuid of player%::3})
        else if event-item = emerald:
            {MASCOTS::gui::%uuid of player%::3} = (size of {MASCOTS::rarity::*}):
                set {MASCOTS::gui::%uuid of player%::3} to 0
            else:
                set {MASCOTS::gui::%uuid of player%::3} to ({MASCOTS::gui::%uuid of player%::3} + 1)
            MASCOTS_gui(player, 1, {MASCOTS::gui::%uuid of player%::2}, {MASCOTS::gui::%uuid of player%::3})


        else:
            set {_nbt} to nbt compound of event-item
            set {_id} to tag "tag;custom;id" of {_nbt}
            event-item != gray dye:
                {MASCOTS::select::%uuid of player%} != {_id}:
                    {_id} is set:
                        close player's inventory
                        MASCOTS(player, {_id})
            else:
                set {_payable} to tag "tag;custom;payable" of {_nbt}
                {_payable} = 1:
                    set {_price} to tag "tag;custom;price" of {_nbt}
                    {@MONEY} >= {_price}:
                        close player's inventory
                        remove {_price} from {@MONEY}
                        add {_id} to {MASCOTS::%uuid of player%::*}
                        send "&aVous avez acheté la mascotte &b%{_id}% &a!" to player
                    else:
                        send "&cVous n'avez pas assez de %{@MONEY_DISPLAY}% &c!" to player
```

- **_Pour faire spawn une mascotte, utiliser cette fonction :_** ⚠️**REMPLACER "default" PAR LE NOM DE VOTRE MASCOTTE**
```
mascots(player, "default", player's location)
```
- **_Pour faire despawn une mascotte, utiliser cette fonction :_**
```
mascots(player, "none", player's location)
```

## 👩🏻‍💻 MASCOTS
- **_Cette fonction si dessous, va permettre aux mascottes de ce déplacer :_**
```
function MASCOTS(p: player, id: text):
    set {_uuid} to uuid of {_p}
    kill all entities in radius 3 of {_p} where [name of entity input is "%{_p}%'s Mascot"]

    {_id} = "undefined":
        delete {MASCOTS::select::%{_uuid}%}
        send "&cVous venez de désactiver votre Mascotte !" to {_p}
    else:
        set {MASCOTS::select::%{_uuid}%} to {_id}

        spawn armor stand at location of {_p}
        set {_mascot} to spawned entity
        add "{Small:1b,NoGravity:1b,NoBasePlate:1b,ShowArms:1b,Invulnerable:1}" to nbt of spawned entity
        set name of last spawned armor stand to "%{_p}%'s Mascot"

        set {_design::*} to MASCOTS_Design({_id})
        set spawned entity's helmet to skull of {_p} with nbt "%{_design::1}%"
        set spawned entity's chestplate to leather chestplate with nbt "%{_design::2}%"
        set spawned entity's leggings to leather leggings with nbt "%{_design::3}%"
        set spawned entity's boots to leather boots with nbt "%{_design::4}%"

        send "&aVotre Mascotte vient d'apparaitre !" to {_p}

        while true:
            {_p} is online:
                set {_loc} to location of {_p}
                add 1 to y-coordinate of {_loc}
                add sin ((({_p}'s yaw)*3.14159265358979323846264338327950288419716939937510582/180)+5*3.14159265358979323846264338327950288419716939937510582/4) to z-coordinate of {_loc}
                add cos ((({_p}'s yaw)*3.14159265358979323846264338327950288419716939937510582/180)+5*3.14159265358979323846264338327950288419716939937510582/4) to x-coordinate of {_loc}
                teleport {_mascot} to {_loc}
                wait tick
            else:
                kill {_mascot}
                stop
```

## 👁️ DESIGN
Vous pourrez vous aidez des mascottes déjà pré-configuré dans le dossier Mascots/data

- **_Pour trouver des têtes aller [ici](https://minecraft-heads.com/custom-heads)_**

- **_Pour trouver les couleurs pour les armures aller [ici](https://htmlcolorcodes.com/fr/) puis [ici](https://minecraftcommand.science/armor-color)_**
