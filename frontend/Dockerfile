# Build stage
FROM node:24 AS build

WORKDIR /app

# Copy package.json and package-lock.json to the container
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Override the committed .env value at build time if needed (CRA bakes REACT_APP_* vars in at build, not at runtime)
ARG REACT_APP_API_BASE_URL=http://application-lb.student.com:8080
ENV REACT_APP_API_BASE_URL=${REACT_APP_API_BASE_URL}

# Build the React app
RUN npm run build

# Serve stage
FROM nginx:1.31.5

# Copy the custom nginx.conf file to the container
COPY .docker/nginx.conf /etc/nginx/nginx.conf

# Copy the built React app from the build stage to the nginx container
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]