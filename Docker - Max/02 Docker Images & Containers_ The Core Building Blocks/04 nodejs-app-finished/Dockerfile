FROM node:12

WORKDIR /app

COPY package.json /app     // this id done to use layer advantage since there is no change in package.json file so Docker will use cached layer untill it finds change in any layer. 
						   //	if it finds any change in layer it will not user cached layer from there onwards step. Since more often then not changes are made in code not in package.json

RUN npm install

COPY . /app

EXPOSE 80

CMD ["node", "server.js"]