command /freeze [<offline player>] [<text>]:
    aliases: clubmodutilities:ss, clubmodutilities:freeze, ss, screenshare, clubmodutilities:screenshare
    trigger:
        if executor does not have permission ".command.freeze.use":
            message "&4you do not have permission to perform this command."
        else:
            if arg 1 is not set:
                message "&eUsage: &6/freeze &r<player>"
            else:
                if arg 1 is not online:
                    message "&cNo player matching &e%arg-1% &cis connected to this server."
                else:
                    if "%arg-1's world%" is not "%player's world%":
                        message "&cNo player matching &e%arg-1% &cis connected to this server."
                    else:
                        if {freeze.%arg-1's world%.%uuid of arg-1%} is not set:
                            set {freeze.%arg-1's world%.%uuid of arg-1%} to true
                            message "&cYou froze %{rank.%{rank-player.%uuid of arg-1%}%.color}%%arg-1%"
                            set {freeze-location.%arg-1's world%.%uuid of arg-1%} to location of arg 1
                            message "&b[S] &3[%player's world%] %{rank.%{rank-player.%uuid of player%}%.color}%%player% &3froze %{rank.%{rank-player.%uuid of arg-1%}%.color}%%arg-1%" to all players where [input has permission "clubmodutilities.staff"]
                            message "&r" to arg 1
                            message "&r         &4&l&nTEAM| Du wurdest durch ein Team Mitglied eingefroren." to arg 1
                            message "&r       &4&l&nTEAM| Bitte melde dich auf unserem Dc im Support: discordlink" to arg 1
                            message "&r         &4&l&nTEAM| Du hast 5 Minuten Zeit. Ausloggen führt bis aufs erste zu einem Community Ausschluss." to arg 1
                            loop 256 times:
                                if block 1 below {freeze-location.%arg-1's world%.%uuid of arg-1%} is air:
                                    remove 1 from y coordinate of {freeze-location.%arg-1's world%.%uuid of arg-1%}
                                else:
                                    stop loop
                            message "&r" to arg 1
                            while {freeze.%arg-1's world%.%uuid of arg-1%} is true:
                                message "&r" to arg 1
                                message "&r         &4&l&nTEAM| Du wurdest durch ein Team Mitglied eingefroren." to arg 1
                                message "&r       &4&l&nTEAM| Bitte melde dich auf unserem Dc dich im Support: discordlink" to arg 1
                                message "&r         &4&l&nTEAM| Du hast 5 Minuten Zeit. Ausloggen führt bis aufs erste zu einem Community Ausschluss." to arg 1
                                message "&r" to arg 1
                                wait 5 seconds
                        else:
                            delete {freeze.%arg-1's world%.%uuid of arg-1%}
                            message "&cYou unfroze %{rank.%{rank-player.%uuid of arg-1%}%.color}%%arg-1%"
                            message "&b[S] &3[%player's world%] %{rank.%{rank-player.%uuid of player%}%.color}%%player% &3unfroze %{rank.%{rank-player.%uuid of arg-1%}%.color}%%arg-1%" to all players where [input has permission "clubmodutilities.staff"]
                            message "&eDu bist nicht länger eingefroren." to arg 1
  
on chat:
    if "%player's world%" contains "hub":
        if player does not have permission "clubmodutilities.staff":
            cancel event 
            message "&cYou're not allowed to talk in the Hub."
    
on any movement:
    if {freeze.%player's world%.%uuid of player%} is set:
        if x coordinate of player is not x coordinate of {freeze-location.%player's world%.%uuid of player%}:
            teleport player to {freeze-location.%player's world%.%uuid of player%}
        else:
            if z coordinate of player is not z coordinate of {freeze-location.%player's world%.%uuid of player%}:
                teleport player to {freeze-location.%player's world%.%uuid of player%}
