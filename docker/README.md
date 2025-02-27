# Clean up existing containers
docker-compose down -v

# Build images
docker-compose build --no-cache

# Run services
docker-compose up -d