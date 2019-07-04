# Create Image using Dockerfile
docker build -t angular-app:1.0.0 .


# Run Docker Image
docker run --rm -d -p 8080:80 angular-app:1.0.0

# Push image to dockerhub
docker push thecrazzyrahul/angular-app:1.0.0
