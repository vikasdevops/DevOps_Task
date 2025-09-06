# Stage 1 - build React
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2 - serve with nginx
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
