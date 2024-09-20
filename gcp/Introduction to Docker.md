Docker helps you ship code faster, test faster, deploy faster, and shorten the cycle between writing code and running code.

Docker does this by combining kernel containerization features with workflows and tooling that helps you manage and deploy your applications.

# Docker 이미지 만들고 확인하기

```
docker run hello-world
docker ps
docker ps -a # 종료된 컨테이너도 볼 수 있음
```

# 도커 빌드

간단한 node app 만들고 빌드하기
```
mkdir test && cd test
```

노드 app
```
cat > Dockerfile <<EOF
# Use an official Node runtime as the parent image
FROM node:lts # The initial line specifies the base parent image, which in this case is the official Docker image for node version long term support (lts).

# Set the working directory in the container to /app
WORKDIR /app # In the second, you set the working (current) directory of the container.

# Copy the current directory contents into the container at /app
ADD . /app # In the third, you add the current directory's contents (indicated by the "." ) into the container.

# Make the container's port 80 available to the outside world
EXPOSE 80 # Then expose the container's port so it can accept connections on that port and finally run the node command to start the application.

# Run app.js using node when the container launches
CMD ["node", "app.js"]
EOF
```

노드 앱 실행
```
cat > app.js << EOF;
const http = require("http");

const hostname = "0.0.0.0";
const port = 80;

const server = http.createServer((req, res) => {
	res.statusCode = 200;
	res.setHeader("Content-Type", "text/plain");
	res.end("Hello World\n");
});

server.listen(port, hostname, () => {
	console.log("Server running at http://%s:%s/", hostname, port);
});

process.on("SIGINT", function () {
	console.log("Caught interrupt signal and will exit");
	process.exit();
});
EOF
```

이미지 빌드 

```
docker build -t node-app:0.1 .
```
The -t is to name and tag an image with the name:tag syntax. The name of the image is node-app and the tag is 0.1. The tag is highly recommended when building Docker images. If you don't specify a tag, the tag will default to latest and it becomes more difficult to distinguish newer images from older ones. Also notice how each line in the Dockerfile above results in intermediate container layers as the image is built.

빌드된 이미지 확인
```
docker images
```

# 빌드된 이미지 실행하기
```
docker run -p 4000:80 --name my-app node-app:0.1
```
The --name flag allows you to name the container if you like. The -p instructs Docker to map the host's port 4000 to the container's port 80. Now you can reach the server at http://localhost:4000. Without port mapping, you would not be able to reach the container at localhost.

백그라운드에서 실행하려면 -d 붙이기

빌드된 앱 들어가보기
```
curl http://localhost:4000
```

# 컨테이너 종료 및 삭제
```
docker stop my-app && docker rm my-app
```

# 배포
Now you're going to push your image to the Google Artifact Registry. After that you'll remove all containers and images to simulate a fresh environment, and then pull and run your containers. This will demonstrate the portability of Docker containers.

To push images to your private registry hosted by Artifact Registry, you need to tag the images with a registry name. The format is <regional-repository>-docker.pkg.dev/my-project/my-repo/my-image.

## Create the target Docker repository (Using Cloud Console)
You must create a repository before you can push any images to it. Pushing an image can't trigger creation of a repository and the Cloud Build service account does not have permissions to create repositories.

From the Navigation Menu, under CI/CD navigate to Artifact Registry > Repositories.

Click the +CREATE REPOSITORY icon next to repositories.

Specify my-repository as the repository name.

Choose Docker as the format.

Under Location Type, select Region and then choose the location : us-east1.

Click Create.

## **Before you can push or pull images, configure Docker to use the Google Cloud CLI to authenticate requests to Artifact Registry. **

```
gcloud auth configure-docker us-east1-docker.pkg.dev
```

```
gcloud artifacts repositories create my-repository --repository-format=docker --location=us-east1 --description="Docker repository" # 저장소 생성
```

```
docker build -t us-east1-docker.pkg.dev/qwiklabs-gcp-02-19c4e38275ff/my-repository/node-app:0.2 . # gcloud 내 repo에 빌드하기
```

```
docker push us-east1-docker.pkg.dev/qwiklabs-gcp-02-19c4e38275ff/my-repository/node-app:0.2
```
![image](https://github.com/user-attachments/assets/b03e7633-58af-4c86-93eb-c6d0560b38ce)




