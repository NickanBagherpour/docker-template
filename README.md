## Note

This repo is based on `@alexeagleson` tutorial

* [Original Repo](https://github.com/alexeagleson/docker-node-postgres-template)
* [Youtube Video](https://www.youtube.com/watch?v=Te41e4urFO0&t=9s)
* [Article](https://dev.to/alexeagleson/docker-for-javascript-developers-41me#introduction)


-----------------------------
## Building the App Container

Now that your Dockerfile is created we have just one last thing we need to do before we build.

Similar to `.gitignore` that you're probably familiar with (used to prevent committing auto-generated files and private secrets to public repositories), Docker has a similar concept to keep you from unnecessarily copying files that your container doesn't need.

Let's create a `.dockerignore` file now:

`.dockerignore`
```
node_modules
npm-debug.log
```

Both of those will be generated inside the container, so we don't want to copy our local versions of them over.

At this point we are ready to build.  Run the following command:

```bash
docker build . -t my-node-app
```

That will build the _image_ describe by the Dockerfile in the current directory `.` and give it a name called `my-node-app`.  When it's done you can see the image and all its details with:

```bash
docker image ls
```

With the image created, we are now ready to build a _container_ off our image which will run our app:

```bash
docker run -p 3000:8080 --name my-node-app-container my-node-app
```

This command tells Docker to use our image to build a running container.  The `--name` flag lets us name the container (to make it easier to identify and stop/start later, otherwise the name will be randomly generated).

I used the name `my-node-app-container` to differentiate it from the last argument which is the name of the _image_ we are building from (`my-node-app`).  

We use the `-p` flag to bind ports from our host machine (our computer) environment to the container environment.  

If you recall we wrote `EXPOSE 8080` in our Dockerfile which is the port our app runs on.  The above command maps port 3000 on our machine to port 8080 in the container.

_(Note you can map the same port like 8080:8080 if you like, we just mixed it up in this example to show that it's possible)_

Double check that your container started successfully with:

```bash
docker container ls
```

My output looks like:

```
CONTAINER ID   IMAGE         COMMAND                  CREATED         STATUS         PORTS                    NAMES
b6523b2602e1   my-node-app   "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:3000->8080/tcp   my-node-app-container
```

_(Sorry if the words wrap and it make it difficult to line things up)_

We can see that the container is up for X minutes.  That means that our app is running on port 8080, we can access that port on our machine using port 3000 so open up your browser to [http://localhost:3000/]() to see the result.

Great!  You've created your first custom Docker image and container with your own app running in it!
