# Write Dockerfile

To create an image of your app, first you need to write a dockerfile.


I will deploy one of my angular apps I had worked earlier \
- Repository: https://github.com/neutral-00/practice-angular
- Branch: 16-template-forms

## Reference
> https://github.com/docker/welcome-to-docker/blob/main/Dockerfile

## Steps
1. open cmd and navigate to the location where `practice-angular` project exist.
- checkout to the correct branch : `git switch 16-template-forms`
- This app needs pnpm + nodejs
- I search the docker hub, I think `https://hub.docker.com/r/guergeiro/pnpm` will be suitable
- While copying we want ignore the node_modules so create `.dockerignore` file with
```
node_modules
dist
```
2. Create a file named Dockerfile
- First let's define what we need to run our app
```
# starting the image with a node + pnpm base image
FROM guergeiro/pnpm:22-10
# The /app directory should act as the main application directory
WORKDIR /app
COPY . .
RUN pnpm install
EXPOSE 4200
CMD ["pnpm", "run", "dev"]
```
3. Let's build and run it
```
docker build -t angular-blogger-app .
docker run -d --rm --name blogger -p 4200:4200 angular-blogger-app

```
The t flag tags the image with a name. \
The --name in the 2nd command give a container name.
