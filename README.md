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
wget <path to raw main.py on GitHub>
```