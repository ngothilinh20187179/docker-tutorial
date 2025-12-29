Dockerfile: 
```bash
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

Build image
```bash
docker build -t my-web-image .
```

Run container
```bash
docker run -d -p 8080:80 --name my-web-container my-web-image
```

Check browser
```bash
localhost:8080
```