# Étape 1 : build Angular
FROM node:24-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
ARG API_URL=http://localhost:8083
RUN sed -i "s#http://localhost:8080#${API_URL}#g" src/app/services/*.ts
RUN npm run build

# Étape 2 : Nginx sert le site
FROM nginx:alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist/frontend/browser /usr/share/nginx/html
EXPOSE 80
