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

## 👨‍💻 Mascot_Gui
- **_Cette fonction si dessous, va vous permettre d'afficher le gui des mascottes._**
```
function Mascot_Gui(p: player, page: number):
    set {_uuid} to uuid of {_p}
    set {mascot::gui::%{_uuid}%} to {_page}

    loop directory {@Directory}'s files:
        set {_preview} to loop-value
        replace all "/home/container/" with "" in {_preview}
        load yaml {_preview} as {_file}

        {mascot::list::%{_uuid}%::*} contains skript-yaml value "id" from {_file}:
            set {_head} to skull of {_p} named "%skript-yaml value "name" from {_file}%" with nbt "%skript-yaml value "head" from {_file}%"
            add "{custom:{id:%skript-yaml value "id" from {_file}%}}" to nbt of {_head}
        else:
            set {_head} to gray dye named "%skript-yaml value "name" from {_file}%"
            add "{custom:{id:%skript-yaml value "id" from {_file}%}}" to nbt of {_head}

        add {_head} to {_items::*}
        unload yaml {_file}

    open chest with 5 row named "&8&lMascots &7&l%{_page}%" to {_p}
    set {_slot} to 10
    loop {_items::*}:
        loop-index parsed as integer > (21*({_page}-1)):
            set {_nbt} to nbt compound of loop-value
            set {_id} to tag "tag;custom;id" of {_nbt}
            {mascot::%{_uuid}%} = {_id}:
                set slot {_slot} of {_p}'s current inventory to loop-value with lore "&8&oObjet de collection!", "", "&7Faites apparaitre une mascotte", "&7qui vous suivra sur tous les serveurs", "&7de Bessarion !", "" and "&a&lSélectionné!"
            else if {mascot::list::%{_uuid}%::*} contains {_id}:
                set slot {_slot} of {_p}'s current inventory to loop-value with lore "&8&oObjet de collection!", "", "&7Faites apparaitre une mascotte", "&7qui vous suivra sur tous les serveurs", "&7de Bessarion !", "" and "&a➥ &eCliquez pour sélectionner !"
            else:
                set slot {_slot} of {_p}'s current inventory to loop-value with lore "&8&oObjet de collection!", "", "&7Faites apparaitre une mascotte", "&7qui vous suivra sur tous les serveurs", "&7de Bessarion !", "", "&7⁃ &fPrix: &b3.500 Coins", "&7⁃ &fActuellement obtenable: &a&l✔", "" and "&a➥ &eCliquez pour acheter !"
            {_slot} = 16 or 25:
                add 2 to {_slot}
            add 1 to {_slot}
            {_slot} = 35:
                exit loop

    set slot 40 of {_p}'s current inventory to barrier named "&c&nRetour en jeu"
    set slot 44 of {_p}'s current inventory to item frame named "&6&lRetirez votre Mascotte"

    size of {_items::*} > 21*{_page}:
        set slot 41 of {_p}'s current inventory to arrow named "&a▶"
    {_page} > 1:
        set slot 39 of {_p}'s current inventory to arrow named "&a◀"
    

inventory click:
    name of current inventory contains "&8&lMascots":
        cancel event
        name of event-item = "&c&nRetour en jeu":
            close player's inventory
        else if name of event-item = "&6&lRetirez votre Mascotte":
            {mascot::%uuid of player%} is set:
                Mascot(player, "undefined")
                Mascot_Gui(player, {mascot::gui::%uuid of player%})
            else:
                send "%{@Mascot}% &cVous n'avez aucune Mascotte sélectionné !" to player
        else if name of event-item = "&a▶":
            Mascot_Gui(player, ({mascot::gui::%uuid of player%} + 1))
        else if name of event-item = "&a◀":
            Mascot_Gui(player, ({mascot::gui::%uuid of player%} - 1))
        else:
            set {_nbt} to nbt compound of event-item
            set {_id} to tag "tag;custom;id" of {_nbt}
            event-item != gray dye:
                {mascot::%uuid of player%} != {_id}:
                    {_id} is set:
                        close player's inventory
                        Mascot(player, {_id})
            else:
                {@Money} >= 3500:
                    close player's inventory
                    remove 3500 from {@Money}
                    add {_id} to {mascot::list::%uuid of player%::*}
                    send "%{@Mascot}% &aVous avez acheté la mascotte &b%{_id}% &a!" to player
                else:
                    send "%{@Mascot}% &cVous n'avez pas assez de &c&lCoins &c!" to player
```

- **_Pour faire spawn une mascotte, utiliser cette fonction :_** ⚠️**REMPLACER "default" PAR LE NOM DE VOTRE MASCOTTE**
```
mascots(player, "default", player's location)
```
- **_Pour faire despawn une mascotte, utiliser cette fonction :_**
```
mascots(player, "none", player's location)
```

## 👩🏻‍💻 Mascot
- **_Cette fonction si dessous, va permettre aux mascottes de ce déplacer :_**
```
function Mascot(p: player, id: text):
    set {_uuid} to uuid of {_p}
    kill all entities in radius 3 of {_p} where [name of entity input is "%{_p}%'s Mascot"]

    {_id} = "undefined":
        delete {mascot::%{_uuid}%}
        send "%{@Mascot}% &cVous venez de désactiver votre Mascotte !" to {_p}
    else:
        set {mascot::%{_uuid}%} to {_id}

        spawn armor stand at location of {_p}
        set {_mascot} to spawned entity
        add "{Small:1b,NoGravity:1b,NoBasePlate:1b,ShowArms:1b,Invulnerable:1}" to nbt of spawned entity
        set name of last spawned armor stand to "%{_p}%'s Mascot"

        set {_design::*} to Mascot_GetDesign({_id})
        set spawned entity's helmet to skull of {_p} with nbt "%{_design::1}%"
        set spawned entity's chestplate to leather chestplate with nbt "%{_design::2}%"
        set spawned entity's leggings to leather leggings with nbt "%{_design::3}%"
        set spawned entity's boots to leather boots with nbt "%{_design::4}%"

        send "%{@Mascot}% &aVotre Mascotte vient d'apparaitre !" to {_p}

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
