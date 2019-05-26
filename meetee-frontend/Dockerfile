FROM nginx:1.15.2-alpine
WORKDIR /webapp
COPY ./package.json /webapp
RUN npm install
COPY . /webapp
EXPOSE 9001
CMD ["npm", "start"]