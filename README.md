# NX-Monorepo-Dockerization

Steps to set up a Nx monorepo, create multi-stage Dockerfiles, and run the final containers:

# Setup Nx Workspace

Initialize Nx workspace:

`npx create-nx-workspace`

Choose Node, NestJS, Docker.

Install Nx plugins:

`npm install @nrwl/express @nrwl/react  @nrwl/next`

## Generate apps:

`
npx nx g @nrwl/express:app server
npx nx g @nrwl/react:app client
npx nx g @nrwl/react:app client1 
`

## Multi-Stage Dockerfiles

### server Dockerfile

apps/server/Dockerfile:

```
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
```

### Client Dockerfile

apps/client/Dockerfile:

```
FROM node:alpine as build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build client

# Production image

FROM nginx:alpine  
COPY --from=build /app/dist/apps/client /usr/share/nginx/html

```

### Client1 Dockerfile

apps/client1/Dockerfile:

```
FROM node:alpine as build
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build client1

# Production image

FROM nginx:alpine  
COPY --from=build /app/dist/apps/client1 /usr/share/nginx/html
```

## Build and Run
### Build images:
```
docker build -t server -f apps/server/Dockerfile .
docker build -t client -f apps/client/Dockerfile .
docker build -t client1 -f apps/client1/Dockerfile .

```

### Run images:

```
docker run -p 3000:3000 server: 
docker run -p 4200:80 client
docker run -p 4201:80 client1

```

Then you can access it at http://localhost:2222 for server, http://localhost:4200 and http://localhost:4201 for client on your host machine.

```
The End The End The End The End The End The End The End The End The End The End
```



# Additional Info

## Start the app locally

To start the development server run `nx serve test-mono-app`. Open your browser and navigate to http://localhost:4200/. Happy coding!
