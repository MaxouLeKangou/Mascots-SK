<h1 align="center">Mascots-SK - Documentation</h1>
<p align="center">Mascots-SK est un skript permettant d'avoir des mascottes comme sur <a href="https://rinaorc.com/">Rinaorc</a>.</p><br />

## 🏹 Plus d'informations ?
Vous souhaitez avoir des informations sur comment télécharger et installer ce code ?
Rendez vous sur cette page : [README.md](https://github.com/MaxouLeKangou/Mascots-SK/blob/main/README.md)

## 👨‍💻 mascots
- **_Cette fonction si dessous, va vous permettre de retirer et/ou de faire spawn une mascotte._**
```
function mascots(p: player, n: text, l: location):
    set {_u} to uuid of {_p}

    kill all entities in radius 7 of {_p} where [name of entity input is "%{_p}%'s Mascot"]

    {_n} = "none":
        send "&cVous venez de retirer votre mascotte !" to {_p}
        delete {mascot::%{_u}%}
    else:
        set {mascot::%{_u}%} to {_n}

        spawn an armor stand at {_l}
        set {_m} to spawned entity
        add "{Small:1b,NoGravity:1b,NoBasePlate:1b,ShowArms:1b,Invulnerable:1}" to nbt of spawned entity
        set name of last spawned armor stand to "%{_p}%'s Mascot"

        set {_skull} to mascots_design({_n}, "skull")
        set {_chestplate} to mascots_design({_n}, "chestplate")
        set {_leggings} to mascots_design({_n}, "leggings")
        set {_boots} to mascots_design({_n}, "boots")
        
        set {_skull} to skull of {_p} with nbt "%{_skull}%"
        set {_chestplate} to leather chestplate with nbt "%{_chestplate}%"
        set {_leggings} to leather leggings with nbt "%{_leggings}%"
        set {_boots} to leather boots with nbt "%{_boots}%"

        set spawned entity's helmet to {_skull}
        set spawned entity's chestplate to {_chestplate}
        set spawned entity's leggings to {_leggings}
        set spawned entity's boots to {_boots}
        
        while true:
            {_p} is online:
                set {_y} to {_p}'s yaw
                set {_l} to {_p}'s location
                add 1 to y-coordinate of {_l}

                add sin (({_y}*3.14159265358979323846264338327950288419716939937510582/180)+5*3.14159265358979323846264338327950288419716939937510582/4) to z-coordinate of {_l}
                add cos (({_y}*3.14159265358979323846264338327950288419716939937510582/180)+5*3.14159265358979323846264338327950288419716939937510582/4) to x-coordinate of {_l}
                
                teleport {_m} to {_l}
                wait tick
            else:
                kill {_m}
                stop
```

- **_Pour faire spawn une mascotte, utiliser cette fonction :_** ⚠️**REMPLACER "default" PAR LE NOM DE VOTRE MASCOTTE**
```
mascots(player, "default", player's location)
```
- **_Pour faire despawn une mascotte, utiliser cette fonction :_**
```
mascots(player, "none", player's location)
```

## 👩🏻‍💻 mascots_design
- **_Cette fonction si dessous, va vous permettre de créer vos propres mascottes :_**
```
function mascots_design(n: text, v: text) :: text:
    {_n} = "default":
        {_v} = "skull":
            set {_v} to "{SkullOwner:{Id:""e4a0d1c5-d868-4b6e-aee6-290644743e1e"",Properties:{textures:[{Value:""eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvYjdmY2NhNThkNjk5NjYyMWIyYTY2YmVjZDFiOGQ0ZDFiMTkwMWVkMWY5NmIwOWFkYTI0MDZkZTk3NjVmYjA5NyJ9fX0=""}]}}}"
        {_v} = "chestplate":
            set {_v} to "{display:{color:16776975}}"
        {_v} = "leggings":
            set {_v} to "{display:{color:16776975}}"
        {_v} = "boots":
            set {_v} to "{display:{color:16776975}}"
        return {_v}
```
## 👁️ Design des mascottes - ⚠️**SUIVER LE MODÈLE DE : 👩🏻‍💻 mascots_design**
- **_Pour trouver des têtes aller [ici](https://minecraft-heads.com/custom-heads)_**

- **_Pour trouver les couleurs pour les armures aller [ici](https://htmlcolorcodes.com/fr/) puis [ici](https://minecraftcommand.science/armor-color)_**
