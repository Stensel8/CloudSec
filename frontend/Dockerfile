# Build stage
FROM node:24@sha256:be23f54a88d34e8824c741b19b91064094f92c1c97b194144bfc8b50d67258e2 AS build

WORKDIR /app

# Copy package.json and package-lock.json to the container
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Override the committed .env value at build time if needed (Vite bakes VITE_* vars in at build, not at runtime)
ARG VITE_API_BASE_URL=http://application-lb.student.com:8080
ENV VITE_API_BASE_URL=${VITE_API_BASE_URL}

# Build the React app
RUN npm run build

# Serve stage
FROM nginx:1.31.5@sha256:db3099a8d62d2bd8f5f7875e6e3d2dfbd92c999cd35967fb05d41374386535e1

# Copy the custom nginx.conf file to the container
COPY .docker/nginx.conf /etc/nginx/nginx.conf

# Copy the built React app from the build stage to the nginx container
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]