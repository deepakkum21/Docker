FROM node

WORKDIR /app

COPY . /app    // or COPY . .  2nd dot points to the working directory set we can explicitly write /app otherwise it will point to root dir or working dir if set using WORKDIR

RUN npm install

EXPOSE 80

CMD ["node", "server.js"]