##Static file serving using nginx web server:   
An Nginx container refers to an instance of the Nginx web server software packaged and deployed using containerization technology. Containers are lightweight, portable, and consistent environments that encapsulate an application and its dependencies, allowing it to run consistently across different environments.

Nginx is a popular open-source web server, reverse proxy server, and load balancer known for its high performance, scalability, and efficiency. When deployed within a container, Nginx can be easily managed, deployed, and scaled using container orchestration platforms like Docker .....

Frist pull container from docker hub

```
docker pull nginx
```

and to see the images 

```
docker images
```

![[Screenshot 2024-02-24 005256 1.png]]

to run the container 

```
docker run nginx
```

use -d to run in detach mode

![[Screenshot 2024-02-24 005714.png]]

Port mapping:

This enables external access to the containerized service through the specified host port, while the service inside the container continues to use its designated port.

for port mapping 

```
#docker run -p hostPort:containerPort image
docker run -p 8000:80 nginx
```

out put can seen in

`http://localhost:8000`

outcomes:
![[Screenshot 2024-02-24 005750.png]]



# STATIC FILE SERVING

CREATE A FOLDER
```
mkdir nginx-portfolio
```
and change directory to nginx-portfolio

Create a sample html files:
```
touch portfolio.html
```

and give 
```
code .
```
to go to vs code editor and 

And create a sample html file  in `portfolio.html`
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VIJAY KRISHNAA - Portfolio</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }

        header {
            background-color: #333;
            color: white;
            text-align: center;
            padding: 1em;
        }

        section {
            max-width: 800px;
            margin: 2em auto;
            padding: 1em;
            background-color: white;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        h2 {
            color: #333;
        }

        p {
            line-height: 1.6;
            color: #666;
        }

        .project {
            margin-bottom: 2em;
        }

        footer {
            text-align: center;
            padding: 1em;
            background-color: #333;
            color: white;
        }
    </style>
</head>

<body>

    <header>
        <h1>VIJAY KRISHNAA P</h1>
        <p>MLOPS</p>
    </header>

    <section id="about">
        <h2>About Me</h2>
        <p>
            Welcome to my portfolio! I am a passionate AI model developer with experience in Genrative ai, NLP Projects and ML model integrate with docker.
            and Expert in docker , server management and Linux Filesystem.
        </p>
    </section>

    <section id="projects">
        <h2>Projects</h2>

        <div class="project">
            <h3>lawyer GPT</h3>
            <p>Description of Project 1.</p>
        </div>

        <div class="project">
            <h3>HR as AI</h3>
            <p>Description of Project 2.</p>
        </div>



    </section>

    <section id="contact">
        <h2>Contact</h2>
        <p>Email: vijaypvk001@gmail.com</p>
        <p>LinkedIn: linkedin.com/vijaypvk</p>
        <p>GitHub: github.com/vijaypvk</p>
    </section>

    <footer>
        &copy; 2024 VIJAY KRISHNAA
    </footer>

</body>

</html>
```

and to host it in required container we need to write a docker file to run on nginx container

`dockerfile:`
```
FROM nginx:latest
COPY . /usr/share/nginx/html

EXPOSE 8000:80

CMD ["nginx", "-g", "daemon off;"]
```

- `FROM nginx:latest`: This line specifies the base image for your Docker image. In this case, it uses the latest version of the official Nginx image from the Docker Hub.
- `COPY . /usr/share/nginx/html`: This line copies the contents of the current directory (where the Dockerfile is located) into the `/usr/share/nginx/html` directory within the Docker image. This is typically the directory where Nginx serves its web content.
- `EXPOSE 8000:80` will host it in `http://localhost:8000`  
- `CMD ["nginx", "-g", "daemon off;"]`: This line specifies the default command to run when the container starts. It starts Nginx with the "daemon off;" option, which is often used when running Nginx in a Docker container to keep the process in the foreground.
and save it 

And built a base image using
```
docker build -t my_portfolio 
```
the `-t` flag is used to tag the Docker image with a specific name .

To verify
`docker images`
![[Pasted image 20240224142521.png]]

And run the image using 
```
 docker run -d -p 8000:80 --name my_portfolio my_portfolio
```
- `-d`: This flag stands for "detached" mode. It means the container will run in the background, and you'll get the command prompt back in your terminal.
    
- `-p 8000:80`: This flag is used to map ports between the container and the host. It maps port 80 on the container to port 8000 on the host. So, if you have a web server running inside the container on port 80, you can access it from your host machine at `http://localhost:8000`.
- `--name my_portfolio`: Is to giving a name to the container.
- `my_portfolio`: This is the name of the Docker image you want to run as a container. 

The container will be created based on the specified image.

now the docker container is running...
![[Pasted image 20240224143403.png]]
To check that :
```
docker ps
```

To see the outcomes:

now you can access the file in  `http://localhost:8000/portfolio.html`
![[Pasted image 20240224143659.png]]

a sample staic file serving using existing docker containers...
