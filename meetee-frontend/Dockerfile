FROM node:10.15.1-alpine
WORKDIR /webapp
COPY ./package.json /webapp
RUN npm install
COPY . /webapp
RUN npm run build
EXPOSE 8080
CMD ["http-server", "dist"]