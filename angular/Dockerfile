# Using node image to build the Angular Project
FROM node:latest as node

# Setting up the working directory
WORKDIR /app

# This will copy all files relative to Dockerfile to the WORKDIR ie: /app
COPY . .

# install all dependency
RUN npm install

# building Angular Project for Production
RUN npm run build --prod

# Using NGINX image to deploy Angular Project
FROM nginx:alpine

# Copy all build files to Nginx
COPY --from=node /app/dist/angular /usr/share/nginx/html
