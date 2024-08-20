# Docker

### Docker este o platformă software care permite dezvoltatorilor să creeze, să desfășoare și să ruleze aplicații în containere.   
## Containers
Containerele sunt unități standardizate de software care împachetează codul aplicației împreună cu toate dependințele sale, asigurându-se că aplicația poate rula consistent pe orice mediu  
Containers are isolated processes for each of your app's components. Each component - the frontend React app, the Python API engine, and the database - runs in its own isolated environment, completely isolated from everything else on your machine  

## Images 
**Imaginile Docker** sunt șabloane read-only folosite pentru a crea containere. Ele conțin tot ceea ce este necesar pentru a rula o aplicație, inclusiv codul aplicației, runtime, librării și setări de configurare  
Images are immutable. Once an image is created, it can't be modified. You can only make a new image or add changes on top of it  

`docker image inspect id_img` ->to see more information about an image  
## ADD and COPY 
Instrucțiunea **COPY** este folosită pentru a copia fișiere și directoare din sistemul gazdă în imaginea Docker. Este o operațiune simplă de copiere și este recomandată pentru majoritatea cazurilor de utilizare atunci când doar trebuie să transferi fișiere locale în imagine.  
Instrucțiunea **ADD** este mai puternică și mai flexibilă decât COPY. În plus față de copierea fișierelor și directoarelor, ADD are funcționalități suplimentare:  

1. Decomprimare arhive:  
Dacă sursa este un fișier de arhivă (.tar, .gz, .bz2, etc.), ADD îl va decomprima automat în destinație.  
2. Copierea de la URL-uri:  
ADD poate copia fișiere direct de la URL-uri, descărcându-le în timpul construirii imaginii.  

## Managing Images & Containers

`docker build -t app:latest . `   
docker build -> pentru a construi o imagine docker  
-t -> pentru a atribui un tag imaginii  

`docker ps` -> afiseaza toate containerele care ruleaza  
`docker ps -a` ->afiseaza toate containerele inclusiv cele care au fost oprite   
`docker images` -> afiseaza toate imaginile  

`docker run --name app` -> to set a name for a container  

## Stopping & Restarting Containers
`docker run` ->a rula un nou container Docker (the container is running in the foreground)  
-d -> rulează containerul în fundal, permițându-ți să continui să folosești terminalul  
-it -> pentru a intra in modul interactiv si a putea introduce date   

`docker start name/id_container` ->to start a container(the container is running in the background)   
`docker stop name_container` -> to stop a container  
`docker attach nume/id_container` ->to attached the container(we see the output in terminal)  = `docker start -a name/id`

 `docker logs nume_container` ->we see the past logs that were printed by that container  

## Deleting Images & Containers
`docker rm -f name `->pentru a elimina un container Docker  
You can't remove a running container  

`docker container prune` -> to remove all stopped containers at once  

`docker rmi id/name_image` -> to remove a image  
You can only remove images if they're not getting used by any container anymore and that includes stopped container.The container needs to be remove first.    
`docker image prune` -> to remove all unused images  

`docker run -p 80:80 -d --rm id_image` -> this container is always removed whenever we stop it  

## Copying files 
`docker cp source destination` ->it allows you to copy files or folders into a running container or out of a running container  
## CMD and ENTRYPOINT
instrucțiunea **CMD** dintr-un Dockerfile este utilizată pentru a specifica comanda implicită care va fi rulată atunci când un container este pornit.  
Dockerfile poate conține mai multe instrucțiuni CMD, dar doar ultima instrucțiune CMD va fi luată în considerare. Instrucțiunile CMD anterioare sunt suprascrise de cea finală.  
**ENTRYPOINT** este folosit pentru a seta o comandă fixă ce este executată atunci când containerul rulează, făcând containerul să se comporte ca un executabil specific  

Diferența între CMD și ENTRYPOINT:  
CMD: Este utilizat pentru a specifica comanda implicită pentru un container. Poate fi ușor suprascrisă.  
ENTRYPOINT: Definește o comandă fixă care nu poate fi suprascrisă la fel de simplu, doar argumentele sale pot fi modificate.  

Container networking refers to the ability for containers to connect to and communicate with each other, or to non-Docker workloads  

`docker run -d -v /root/app-1:/usr/share/nginx/html -p 80:80 --name app-1 nginx:alpine`  


-v sau --mount-> mapează un volum din gazdă către container  
Acest lucru înseamnă că orice fișiere din /root/app-1 pe gazdă vor fi disponibile în /usr/share/nginx/html în container. /usr/share/nginx/html este directorul implicit din care NGINX servește fișierele  
-p -> mapează un port de pe gazdă la un port din container  
Portul 80 de pe gazdă este mapat la portul 80 din container  
--name specifică un nume pentru container  

`curl localhost:81` -> face o cerere HTTP către serverul care rulează pe gazda locală (localhost) la portul 81  

`docker exec` -> este folosită pentru a executa comenzi într-un container deja rulând  
sh -c -> Opțiunea -c permite trecerea unei comenzi complete ca argument pentru shell-ul sh  
--mount type=bind -> Opțiunea --mount este folosită pentru a atașa un volum la container. type=bind specifică că se folosește un volum de tip bind mount  


