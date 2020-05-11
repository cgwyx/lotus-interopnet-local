# lotus-interopnet-local
 local devnet from lotus interopnet  

docker run -d --restart=always --name lotuslocal --gpus all -e BELLMAN_CUSTOM_GPU="GeForce GTX 1660:1408" \  
-v /media/p300/lotusv25/:/root/ -v /media/p300/lotusv25/:/var/tmp/ -p "1234:1234"  -p "1347:1347" -p "2345:2345" \  
cgwyx/lotus-interopnet-local:latest  

docker exec -it  lotuslocal lotus-seed pre-seal --sector-size 2048 --num-sectors 10  
docker exec -it  lotuslocal lotus-seed genesis new lotusgen.json  
docker exec -it  lotuslocal lotus-seed genesis add-miner lotusgen.json ~/.genesis-sectors/pre-seal-t01000.json  
docker exec -it  lotuslocal lotus daemon --lotus-make-genesis=dev.gen --genesis-template=lotusgen.json --bootstrap=false  
(in a separate tab)  
docker exec -it  lotuslocal lotus wallet import ~/.genesis-sectors/pre-seal-t01000.key  
docker exec -it  lotuslocal lotus-storage-miner init --genesis-miner --actor=t01000 --sector-size=2048 --pre-sealed-sectors=~/.genesis-sectors --pre-sealed-metadata=~/.genesis-sectors/pre-seal-t01000.json --nosync  
docker exec -it  lotuslocal lotus-storage-miner run --nosync  
