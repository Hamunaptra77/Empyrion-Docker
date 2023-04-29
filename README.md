# Ubuntu 22.04.2 LTS Empyrion - Galactic Survival 

**Docker-Image für den dedizierten Server [Empyrion](https://empyriongame.com/) mit WINE** 
Das Image selbst enthält WINE und steamcmd sowie ein entrypoint.sh-Skript, das die Installation des dedizierten Empyrion-Servers 
über steamcmd startet. Wenn Sie das Image ausführen, mounten Sie das Volume /home/user/Steam, 
um die Empyrion-Installation beizubehalten und zu vermeiden, dass sie bei jedem Containerstart heruntergeladen wird. 
Beispiel für einen Aufruf:
```
mkdir -p gamedir
docker run -di -p 30000:30000/udp -p 30001:30001/udp -p 30002:30002/udp -p 30003:30003/udp --restart unless-stopped -v $PWD/gamedir:/home/user/Steam bitr/empyrion-server

# Für die experimentelle Version:
mkdir -p gamedir_beta
docker run -di -p 30000:30000/udp -p 30001:30001/udp -p 30002:30002/udp -p 30003:30003/udp --restart unless-stopped -v $PWD/gamedir_beta:/home/user/Steam -e BETA=1 bitr/empyrion-server
```

Nach dem Start des Servers können Sie die Datei dedicated.yaml unter 'gamedir/steamapps/common/Empyrion - Dedicated Server/dedicated.yaml' bearbeiten. Sie müssen den Docker-Container nach der Bearbeitung neu starten.

Der Ordner DedicatedServer wurde symbolisch mit /server verknüpft, so dass Sie mit z:/server/Saves auf Speicherungen verweisen können 
(z. B. den Speicherplatz The\_Game):
```
# cp -r /..../Saves/Games/The_Game 'gamedir/steamapps/common/Empyrion - Dedicated Server/Saves/Games/'
# you might want a symlink for games: ln -s 'gamedir/steamapps/common/Empyrion - Dedicated Server/Saves/Games'
docker run -di -p 30000:30000/udp --restart unless-stopped -v $PWD/gamedir:/home/user/Steam bitr/empyrion-server -dedicated 'z:/server/Saves/Games/The_Game/dedicated.yaml'
```

Um Argumente an den Befehl steamcmd anzuhängen, verwenden Sie '-e "STEAMCMD=..."'. Beispiel: '-e "STEAMCMD=+runscript /home/user/Steam/addmods.txt"'.

Weitere Informationen über den dedizierten Server selbst finden Sie im [wiki](https://empyrion.gamepedia.com/Dedicated_Server_Setup).

TestServer IP 82.165.116.241
