FROM node:10.15.1-alpine
WORKDIR /webapp
COPY . /webapp
RUN npm install && npm run build

FROM nginx
RUN mkdir /webapp
COPY --from=0 /webapp/dist /webapp
COPY nginx.conf /etc/nginx/nginx.conf

# RUN npm run build
# EXPOSE 8080
# CMD ["http-server", "dist"]