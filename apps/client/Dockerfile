FROM node:alpine as build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build client

# Production image

FROM nginx:alpine  
COPY --from=build /app/dist/apps/client /usr/share/nginx/html

# FROM nginx:alpine  
# COPY --from=build /app/dist/apps/client /usr/share/nginx/html