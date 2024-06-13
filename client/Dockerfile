# Stage 1: Build the React app
FROM node:16-alpine AS build

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install app dependencies
RUN npm install

# Copy the rest of your application code to the working directory
COPY . .

# Build the React app
RUN npm run build

# Stage 2: Serve the React app with a lightweight server
FROM nginx:alpine

# Copy built files from the build stage
COPY --from=build /app/dist /usr/share/nginx/html

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose a port to communicate with the React app
EXPOSE 80

# No CMD required as nginx default command will serve the content
