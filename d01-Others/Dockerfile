### BUILD STAGE
FROM node:16.0.0-alpine as build

# Set custom labels
LABEL maintainer="KMS Healthcare"
LABEL version="1.0"
LABEL description="A simple web application"

# Sets the working directory inside the container
WORKDIR /app

# Copy the dependencies JSON config files to WORKDIR
# This is done BEFORE npm install for optimal caching
# Use cache mounting for npm install
COPY package*.json ./
RUN --mount=type=cache,id=npm,target=/root/.npm npm install

# Copy necessary files/directories to WORKDIR for build step
COPY . .

# Build with potential cache invalidation
RUN --mount=type=cache,id=npm,target=/root/.npm npm run build

### RUN STAGE
FROM nginx:1.23.3-alpine as production

# Copy artifact to nginx directory
COPY --from=build /app/dist /usr/share/nginx/html

RUN rm -f /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/

# Expose necessary port(s)
EXPOSE 80

# Start nginx with ENTRYPOINT
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
