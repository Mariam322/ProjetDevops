# Stage 1 : Build
FROM node:latest as builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Stage 2 : Run
FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
