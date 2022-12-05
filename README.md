# hesse-producer

The code stems from https://github.com/Spycsh/flink-statefun-playground/tree/main/playground-internal/statefun-playground-producer

## Edit

```
# push the recent changes
git commit -a -m "edit"
git push
# pull the original image and edit
docker pull spycsh/graph-producer
docker exec -it <container id> /bin/sh
rm -f main.py
wget https://raw.githubusercontent.com/Spycsh/hesse-producer/main/main.py
exit
docker commit -m="specify partition number" -a="spycsh" <container id> spycsh/hesse-producer
docker login
docker push spycsh/hesse-producer
```