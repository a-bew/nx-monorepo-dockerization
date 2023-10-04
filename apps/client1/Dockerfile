FROM node:alpine as build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build client1

# Production image

FROM nginx:alpine  
COPY --from=build /app/dist/apps/client1 /usr/share/nginx/html
