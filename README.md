# P4.-Network-e-Compose_Marcos-Alonso-Cabral
## 1. Crear unha rede en docker
Para comezar unha rede en docker podemos poñer o comando `docker network create <nome da red>`.  
>[!NOTE]
>No meu caso a miña rede se chama "red_marcos"  

## 2. Crear dous contenedores unidos a esa rede
Para crear estes dous contenedores debemos facer o comando `docker run -dit --name <nome do contenedor> --network <nome da red anterior> ubuntu`
Unha vez posto o comando, no meu caso voy poñer dous contenedores chamados *contenedor1* e *contenedor2*.  
Comando : `docker run -dit --name contenedor1 --network red_marcos ubuntu` e `docker run -dit --name contenedor2 --network red_marcos ubuntu`.

## 3. Comprobar que os contenedores están na rede
Para comprobar que ambos contenedores estan na rede debemos facer o seguinte comando : `docker network inspect red_marcos`  
Ademais con este comando tamen podemos ver a configuracion da propia rede.
## 4. Comprobar que os contenedores poden verse entre eles

Primeiro de todo debemos ter iniciados ambos contenedores e facer na terminal de un.
>[!NOTE]
>Esto o podemos facer mediante un *attach shell* ou metendonos na consola e poñer o comando `docker exec -it contenedor1 bash`.  

Para que podan verse primero que todo, xa que me pasou a min, se non tes o docker co comando ping instalado non vai deixarte facer o ping.  
Para isto, debemos poñer na terminal do contenedor que acabamos de abrir o seguinte comando : `apt update`  e posteriormente un `apt install -y iputils-ping`  
Unha vez na terminal dun contenedor facemos o comando `ping contenedor2` e se vai todo correcto, deberian conectarse.

## 5. Listar os contenedores conectados á rede
Para listar os contenedores debemos poñer o comando : `docker network inspect red_marcos`    
Na saída, na sección de "Containers", verás os contedores conectados a esa rede.
>[!NOTE]
>Este é o comando que usei no punto catro xa que mostra ambas cousas
## 6. Listar as propiedades da rede
As propiedades da rede (como o nome, driver, subrede, etc.) pódense ver usando o mesmo comando *inspect*: `docker network inspect red_marcos`
## 7. Crea outra rede
Para crear outra rede facemos o mesmo que no primeiro paso, debemos poñer o comando `docker network create <nome da red>`.  
>[!NOTE]
>No meu caso a miña segunda rede chamarase "red_marcos2"  
## 8. Lanza dous contenedores novos conectados a esa nova rede
Para facer este apartado, simplemente debemos copiar o apartado 2 xa que e o mesmo e só temos que cambiarlle o nome aos contenedores como podemos ver nos seguintes comandos: `docker run -dit --name contenedor3 --network red_marcos2 ubuntu` e `docker run -dit --name contenedor4 --network red_marcos2 ubuntu`.
## 9. Comproba as posibles conexións entre os 4 contenedores
Como fixen no punto 4, debemos facer ping entre todos os contenedores.Primeiro debemos entrar por exemplo na terminal do *contenedor1*.  
>[!NOTE]
>Esto o podemos facer mediante un *attach shell* ou metendonos na consola e poñer o comando `docker exec -it contenedor1 bash`.  
Unha vez na terminal deste primeiro contenedor facemos o comando `ping contenedor2`, e como podemos ver, o contenedor1 se conecta a este segundo contenedor.  
Logo facemos o ping pero co contenedor3 e comprobamos se conecta co comando `ping contenedor3`. E como podemos observar, da fallo e non se encontran xa que o container 3 xunto co container4 estan na red_marcos2 que é esta segunda red que creamos.  
  
  Logo pasamos á terminal do contenedor3 (xa que o contenedor2 vai facer o mesmo que o un xa que estan na mesma rede).
>[!NOTE]
>Unha vez dentro de la terminal recomiendo facer un `apt update` e un `apt install -y iputils-ping` para instalar a funcion de ping que seguramente non este instalado (polo menos no meu caso)  
Logo procedemos a executar o comando `ping contenedor1`, e observamos que tanto o ping do contenedor1 como o do contenedor2 da error xa que esta noutra rede. Pero a o facer o `ping contenedor4` podemos ver que entre eles si se conectan.

>[!NOTE]
>En conclusion só se conectan o contenedor1 co 2, e o contenedor3 co 4.  

# Docker compose:

## 1. Segue os pasos da guía de iniciación de docker-compose, e explica coas túas palabras os pasos que segues e qué fan
>[!NOTE]
>Primero de todo, el proceso que segui esta en el siguiente link : https://docs.docker.com/compose/gettingstarted/  

O primeiro que hay que facer é crear una carpeta de proyecto, en mi caso la llame composetest ya que era la que aparecia en el tutorial. Luego creamos con `touch app.py` este fichero en el que dentro pondremos el siguiente codigo que realmente es lo que nos va a hacer que luego cuando entremos a la pagina web del contenerod nos aparezca que entramos varias veces e todo o demais.   

Logo de configurar ese fichero volvemos facer un `touch requirements.txt` para crear este fichero de requerimientos en el que debemos poner el codigo que nos manda en el propio tutorial.    

O seguinte que faremos é crear a dockerfile que simplemente podemos crear ubn fichero sin extension e modificalo poñendolle o codigo que manda no tutorial, que resumidamente instala y contruye esta imaxes con os requerimentos e o codigo que ye puximos anteriorente nos outros ficheiros asi como o porto.    

Logo creamos un arquivo .yaml co comando `touch compose.yaml` e dentro poñemos o que nos indica a paxina que basicamente é o tipo de imaxe e os portos que vai utilizar.   

E finalmente para lanzar o docker compose facemos o comando `docker compose up` na carpeta do proyecto. E se copiamos todo tal cal da web, apareceranos no navegador na ubicacion de `http://localhost:8000/`    

E se accedemos e vamos refrescando a paxina podemos ver que o numero de accesos cambia.


## 2. Agora que sabes algo máis de docker-compose, crea un arquivo (ou varios arquivos) de configuración que ó ser lanzados cun docker-compose up, resulten nunha rede docker á que estean conectados 3 contenedores, explica os parámetros do .yaml usado

Para crear un arquivo .yaml que inicie os tres contenedores a vez debemos hacer un `touch compose.yaml`en el directorio en el que queramos que este el archivo y pegar por ejemplo el siguiente codigo que ten estos 3 servizos:  


        version: '3.8'
      
      services:  

      app:  
        image: myapp:latest  
        networks:  

          - mynetwork  

        depends_on:  
          - db  
          - cache  
            

      db:  
        image: postgres:latest  
        environment:  
          POSTGRES_USER: user  
          POSTGRES_PASSWORD: password  

        networks:  
          - mynetwork  
            
      cache:  
        image: redis:latest  
        networks:  
          - mynetwork     

      networks:  
      mynetwork:  
        driver: bridge 




### Explicación de los parámetros:

1. versión: especifica a versión da sintaxe de Docker Compose que se está a utilizar. Neste caso, estase a utilizar a versión
3.8


2. services: Define os contedores a executar. Cada contedor defínese como un servizo.

- app: este é o servizo principal que representa a túa aplicación.
- image: especifica a imaxe de Docker que se utilizará para crear o contedor. Neste caso, úsase myapp:latest

- network: indica a que rede se conectará o contedor. Aquí te conectas á mynetwork

- depends_on: define as dependencias entre servizos. Isto garante que a base de datos e o contenedor de caché se inicien antes da aplicación

db: este servizo representa unha base de datos PostgreSQL.
- image: especifica a imaxe de Docker para PostgreSQL.
- enviroment: Aquí defínense as variables de ambiente necesarias para configurar a base de datos, como o nome de usuario e o contrasinal.
- network: tamén se conecta á mynetwork


- caché: este servizo representa un contedor Redis.
- image: especifica a imaxe de Docker para Redis.
- network: conéctase á miña rede


3. network: Define as redes que se utilizarán nos servizos.
- mynetwork: é o nome da rede que se crea.
- driver: especifica o tipo de rede. En




## 3. Busca e proba 4 parámetros e configuracións diferentes que podes incluir no arquivo compose, explica qué fan. (por exemplo diferentes cousas que facer coa opción RUN)
Parametros: 

build: Docker Compose creará a imaxe da aplicación a partir do ficheiro Dockerfile especificado no camiño indicado.  

volume: este parámetro utilízase para montar volumes no contedor, permitindo que os datos se conserven ou que se compartan ficheiros entre o host e o contenedor.

ports: este parámetro utilízase para asignar portos de contedores a portos de host. Isto permite que as aplicacións do contedor sexan accesibles desde o exterior.