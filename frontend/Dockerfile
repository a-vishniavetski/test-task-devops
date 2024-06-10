FROM node:20-alpine

WORKDIR /app

COPY . .

RUN npm update
RUN npm install

CMD ["npm", "run", "start"]
