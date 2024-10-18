# P4.-Network-e-Compose_Marcos-Alonso-Cabral
## 1. Crear unha rede en docker

## 2. Crear dous contenedores unidos a esa rede

## 3. Comprobar que os contenedores están na rede

## 4. Comprobar que os contenedores poden verse entre eles

## 5. Listar os contenedores conectados á rede

## 6. Listar as propiedades da rede

## 7. Crea outra rede

## 8. Lanza dous contenedores novos conectados a esa nova rede

## 9. Comproba as posibles conexións entre os 4 contenedores

## 10. Docker compose:

Segue os pasos da guía de iniciación de docker-compose, e explica coas túas palabras os pasos que segues e qué fan

Agora que sabes algo máis de docker-compose, crea un arquivo (ou varios arquivos) de configuración que ó ser lanzados cun docker-compose up, resulten nunha rede docker á que estean conectados 3 contenedores, explica os parámetros do .yaml usado

Busca e proba 4 parámetros e configuracións diferentes que podes incluir no arquivo compose, explica qué fan. (por exemplo diferentes cousas que facer coa opción RUN)



Documentación:

 

https://docs.docker.com/compose/gettingstarted/

https://docs.docker.com/compose/intro/compose-application-model/

https://docs.docker.com/reference/cli/docker/network/create/

https://medium.com/@caysever/docker-compose-network-b86e424fad82

 

Mini-guía comandos network:

Para crear redes usamos comando

docker network create

 

Para configurar tódo-los parámetros da rede:

docker network create \

  --driver=bridge \

  --subnet=172.28.0.0/16 \

  --ip-range=172.28.5.0/24 \

  --gateway=172.28.5.254 \

  minharede

 

exemplo de contenedor conectado:

docker run -itd --network=mired ubuntu
