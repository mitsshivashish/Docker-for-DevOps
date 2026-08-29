# Docker Exercise 1.1 - Getting Started

## Question

Since we already did "Hello, World!" in the material, let's do something else.

Start **3 containers** from an image that does not automatically exit (such as `nginx`) in detached mode.

Stop **2 containers** and leave **1 container running**.

### Answer

#### i) Commands Used

    docker run -d --name nginx1 nginx
    docker run -d --name nginx2 nginx
    docker run -d --name nginx3 nginx

    docker stop nginx1
    docker stop nginx2

#### ii) Output

![Docker `ps -a` output](./images/docker-ps-a.png)
