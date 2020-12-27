FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

ENV MONGODB_USERNAME=deepak
ENV MONGODB_PASSWORD=deepak

CMD ["npm", "start"]