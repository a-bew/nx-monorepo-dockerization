# Builder stage
FROM node:alpine as build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .  
RUN npm run build server

# Production stage 
FROM node:alpine as production
WORKDIR /app
COPY --from=build /app/dist/apps/server ./
COPY --from=build /app/node_modules ./node_modules
CMD ["node", "main"]