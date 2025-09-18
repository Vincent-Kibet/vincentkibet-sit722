# Base image
FROM nginx:1.21-alpine

# Copy static files
COPY . /usr/share/nginx/html

# Expose port
EXPOSE 80

# Start server
CMD ["nginx", "-g", "daemon off;"]