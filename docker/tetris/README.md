# Tetris

Basado en la <https://mpirescarvalho.github.io/react-tetris/>

## demo 1 - como virtual machine

Abrimos un docker dentro a modo de maquina virtual

```bash
docker run -it --rm ubuntu:latest /bin/bash
```

Una vez dentro vamos a clonarnos el repo y hacer el build y run del proyecto

```bash
cd /tmp
git clone https://github.com/mpirescarvalho/react-tetris/
# falla por no tener git
apt install git
# falla por los sources
apt update
apt install git
git clone https://github.com/mpirescarvalho/react-tetris/
cd react-tetris

# sabemos de que es el repo, node + npm
apt install nodejs npm
# ahora ya no falla porque tenemos los sources

npm install
npm run build

npm start
# arrancado pero no vemos nada...
```

hacemos lo mismo pero compartiendo el puerto 3000

```bash
docker run -it --rm -p 3000:3000 ubuntu:latest /bin/bash
```

```bash
cd /tmp
apt update
apt install git nodejs npm

git clone https://github.com/mpirescarvalho/react-tetris/
cd react-tetris

npm install
npm run build

npm start
# abrimos el navegador en localhost:3000
```

Ahora ya lo vemos en el navegador.

Ahora paramos la ejecución, dumpeamos el history y salimos

```bash
history > /tmp/history.txt
exit
```

Vamos a intentar hacer esto reproducible con un Dockerfile

```Dockerfile
FROM ubuntu:latest

RUN apt update
RUN apt install -y git nodejs npm
WORKDIR /tmp
RUN git clone https://github.com/mpirescarvalho/react-tetris/
WORKDIR /tmp/react-tetris
RUN npm install
RUN npm run build
CMD [ "npm", "start" ]
```

Hacemos build de la imagen de docker:

```bash
docker build -t tetris .
```

Y la ejecutamos:

```bash
docker run -it --rm -p 3000:3000 tetris
```

