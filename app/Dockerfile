# build stage
FROM node:14 as build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# production stage
FROM nginx:1.21.4 as production-stage
# RUN apt update -y \
#     && apt install nginx -y \
#     && apt-get install software-properties-common -y \
#     && add-apt-repository ppa:certbot/certbot -y \
#     && apt-get update -y \
#     && apt-get install python-certbot-nginx -y \
#     && apt-get clean

RUN apt update -y
RUN apt install certbot python3-certbot-nginx -y

COPY ./nginx.conf /etc/nginx/nginx.conf
COPY --from=build-stage /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
