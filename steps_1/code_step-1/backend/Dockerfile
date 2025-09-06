# Stage 1 - install deps
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install --production
COPY . .

# Stage 2 - run
FROM node:18-alpine
WORKDIR /app
COPY --from=build /app .
EXPOSE 5000
ENTRYPOINT node server.js
