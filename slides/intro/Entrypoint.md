
# Using `ENTRYPOINT`

We want to be able to specify a different message on the command line,
while retaining `figlet` and some default parameters.

In other words, we would like to be able to do this:

```bash
$ docker run figlet salut
           _            
          | |           
 ,   __,  | |       _|_ 
/ \_/  |  |/  |   |  |  
 \/ \_/|_/|__/ \_/|_/|_/
```


We will use the `ENTRYPOINT` verb in Dockerfile.

---

## Adding `ENTRYPOINT` to our Dockerfile

Our new Dockerfile will look like this:

```dockerfile
FROM ubuntu
RUN apt-get update
RUN ["apt-get", "install", "figlet"]
ENTRYPOINT ["figlet", "-f", "script"]
```

* `ENTRYPOINT` defines a base command (and its parameters) for the container.

* The command line arguments are appended to those parameters.

* Like `CMD`, `ENTRYPOINT` can appear anywhere, and replaces the previous value.

Why did we use JSON syntax for our `ENTRYPOINT`?

---

## Implications of JSON vs string syntax

* When CMD or ENTRYPOINT use string syntax, they get wrapped in `sh -c`.

* To avoid this wrapping, we can use JSON syntax.

What if we used `ENTRYPOINT` with string syntax?

```bash
$ docker run figlet salut
```

This would run the following command in the `figlet` image:

```bash
sh -c "figlet -f script" salut
```

---

## Build and test our image

Let's build it:

```bash
$ docker build -t figlet .
...
Successfully built 36f588918d73
Successfully tagged figlet:latest
```

And run it:

```bash
$ docker run figlet salut
           _            
          | |           
 ,   __,  | |       _|_ 
/ \_/  |  |/  |   |  |  
 \/ \_/|_/|__/ \_/|_/|_/
```

---

## Using `CMD` and `ENTRYPOINT` together

What if we want to define a default message for our container?

Then we will use `ENTRYPOINT` and `CMD` together.

* `ENTRYPOINT` will define the base command for our container.

* `CMD` will define the default parameter(s) for this command.

* They *both* have to use JSON syntax.

---

## `CMD` and `ENTRYPOINT` together

Our new Dockerfile will look like this:

```dockerfile
FROM ubuntu
RUN apt-get update
RUN ["apt-get", "install", "figlet"]
ENTRYPOINT ["figlet", "-f", "script"]
CMD ["hello world"]
```

* `ENTRYPOINT` defines a base command (and its parameters) for the container.

* If we don't specify extra command-line arguments when starting the container,
  the value of `CMD` is appended.

* Otherwise, our extra command-line arguments are used instead of `CMD`.

---

## Build and test our image

Let's build it:

```bash
$ docker build -t figlet .
...
Successfully built 6e0b6a048a07
Successfully tagged figlet:latest
```

Run it without parameters:

```bash
$ docker run figlet
 _          _   _                             _        
| |        | | | |                           | |    |  
| |     _  | | | |  __             __   ,_   | |  __|  
|/ \   |/  |/  |/  /  \_  |  |  |_/  \_/  |  |/  /  |  
|   |_/|__/|__/|__/\__/    \/ \/  \__/    |_/|__/\_/|_/
```

