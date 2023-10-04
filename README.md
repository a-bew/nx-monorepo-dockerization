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






# Addtional Set up generated by NX mono-repo

## Start the app locally

To start the development server run `nx serve test-mono-app`. Open your browser and navigate to http://localhost:4200/. Happy coding!


## Generate code

If you happen to use Nx plugins, you can leverage code generators that might come with it.

Run `nx list` to get a list of available plugins and whether they have generators. Then run `nx list <plugin-name>` to see what generators are available.

Learn more about [Nx generators on the docs](https://nx.dev/plugin-features/use-code-generators).

## Running tasks

To execute tasks with Nx use the following syntax:

```
nx <target> <project> <...options>
```

You can also run multiple targets:

```
nx run-many -t <target1> <target2>
```

..or add `-p` to filter specific projects

```
nx run-many -t <target1> <target2> -p <proj1> <proj2>
```

Targets can be defined in the `package.json` or `projects.json`. Learn more [in the docs](https://nx.dev/core-features/run-tasks).

## Want better Editor Integration?

Have a look at the [Nx Console extensions](https://nx.dev/nx-console). It provides autocomplete support, a UI for exploring and running tasks & generators, and more! Available for VSCode, IntelliJ and comes with a LSP for Vim users.

## Ready to deploy?

Just run `nx build demoapp` to build the application. The build artifacts will be stored in the `dist/` directory, ready to be deployed.

## Set up CI!

Nx comes with local caching already built-in (check your `nx.json`). On CI you might want to go a step further.

- [Set up remote caching](https://nx.dev/core-features/share-your-cache)
- [Set up task distribution across multiple machines](https://nx.dev/core-features/distribute-task-execution)
- [Learn more how to setup CI](https://nx.dev/recipes/ci)

## Connect with us!

- [Join the community](https://nx.dev/community)
- [Subscribe to the Nx Youtube Channel](https://www.youtube.com/@nxdevtools)
- [Follow us on Twitter](https://twitter.com/nxdevtools)
# nx-monorepo-dockerization