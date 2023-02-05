# hesse-producer

The code serves as a docker image and it enables producing messages into in one Kafka topic from a JSON file. For multi-partition
implementation, please refer to branch `multi-partition`.

## Build

```
# push the recent changes
git commit -a -m "edit"
git push
# pull the original image and edit
docker pull spycsh/graph-producer
docker run -d spycsh/graph-producer
docker exec -it <container id> /bin/sh
rm -f main.py
wget https://raw.githubusercontent.com/Spycsh/hesse-producer/main/main.py
exit
docker commit -m="specify partition number" -a="spycsh" <container id> spycsh/graph-producer
docker login
docker push spycsh/graph-producer
```