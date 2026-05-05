# FROM node:20 AS build
# WORKDIR /app
# COPY package*.json ./
# RUN npm install
# COPY . .
# npm run build

FROM nginx:alpine
COPY dist /usr/share/nginx/html