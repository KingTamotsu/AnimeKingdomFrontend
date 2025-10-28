# AnimeKingdomFrontend - Docker Setup

This Angular application is containerized using Docker for easy deployment and hosting.

## Docker Configuration

The project includes the following Docker-related files:

- **`Dockerfile`** - Multi-stage production build (Node.js build + Nginx serve)
- **`Dockerfile.dev`** - Development container with hot-reloading
- **`docker-compose.yml`** - Docker Compose configuration for both production and development
- **`.dockerignore`** - Excludes unnecessary files from Docker build context
- **`nginx.conf`** - Custom Nginx configuration for serving the Angular SPA

## Production Deployment

### Build and Run with Docker

```bash
# Build the production image
docker build -t anime-kingdom-frontend .

# Run the container on port 3000
docker run -d -p 3000:80 --name anime-kingdom anime-kingdom-frontend

# View the application
# Open http://localhost:3000 in your browser
```

### Using Docker Compose (Recommended)

```bash
# Build and start the production container
docker-compose up -d

# View logs
docker-compose logs -f

# Stop the container
docker-compose down
```

The application will be available at **http://localhost:3000**

## Development with Docker

For development with hot-reloading:

```bash
# Start development container
docker-compose --profile dev up -d anime-kingdom-dev

# The dev server will be available at http://localhost:4200
```

## Container Features

### Production Container
- **Multi-stage build** - Optimized for size and security
- **Nginx web server** - High-performance static file serving
- **Security headers** - X-Frame-Options, X-Content-Type-Options, X-XSS-Protection
- **Gzip compression** - Reduces bandwidth usage
- **Health check endpoint** - Available at `/health`
- **Proper Angular routing** - All routes serve `index.html` for SPA behavior

### Development Container
- **Hot-reloading** - Changes reflect immediately
- **Volume mounting** - Source code changes sync with container
- **All dev dependencies** - Full development environment

## Environment Variables

The containers support the following environment variables:

- `NODE_ENV` - Set to `production` or `development`

## Health Check

The production container includes a health check endpoint:

```bash
# Check container health
curl http://localhost:3000/health
```

## Deployment Options

### Docker Hub / Container Registry

```bash
# Tag for registry
docker tag anime-kingdom-frontend your-registry/anime-kingdom-frontend:latest

# Push to registry
docker push your-registry/anime-kingdom-frontend:latest
```

### Cloud Deployment

This containerized application can be deployed to:

- **AWS ECS/Fargate**
- **Google Cloud Run**
- **Azure Container Instances**
- **DigitalOcean App Platform**
- **Heroku Container Registry**
- **Kubernetes clusters**

### Example Cloud Run deployment:

```bash
# Build and tag for Google Cloud Run
docker build -t gcr.io/your-project/anime-kingdom-frontend .
docker push gcr.io/your-project/anime-kingdom-frontend

# Deploy to Cloud Run
gcloud run deploy anime-kingdom --image gcr.io/your-project/anime-kingdom-frontend --platform managed
```

## Performance

The production container:
- Serves compressed static files
- Uses efficient Nginx configuration
- Includes proper caching headers
- Has a small footprint (~50MB after build)

## Troubleshooting

### Common Issues

1. **Build fails with "ng: not found"**
   - Ensure the Dockerfile includes all dependencies with `npm ci` (not `--only=production`)

2. **Container starts but app doesn't load**
   - Check the build output path in the Dockerfile matches Angular's dist folder
   - Verify nginx.conf serves files from the correct directory

3. **Routes return 404**
   - Ensure nginx.conf includes the `try_files` directive for Angular routing

### Debug Commands

```bash
# View container logs
docker logs anime-kingdom

# Access container shell
docker exec -it anime-kingdom sh

# Check Nginx configuration
docker exec anime-kingdom nginx -t

# View container processes
docker exec anime-kingdom ps aux
```

## Development

### Local Development without Docker

```bash
npm install
npm start
```

### Building for Production Locally

```bash
npm run build
# Output will be in dist/AnimeKingdomFrontend/
```

---

## Docker Commands Quick Reference

```bash
# Build
docker build -t anime-kingdom-frontend .

# Run production
docker run -d -p 3000:80 anime-kingdom-frontend

# Run development
docker-compose --profile dev up

# View logs
docker logs <container-name>

# Stop all
docker-compose down

# Clean up
docker system prune
```