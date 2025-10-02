# Use a fixed and minimal base image instead of "latest"
FROM alpine:3.19

# Create a non-root user
RUN addgroup -S app && adduser -S app -G app

# Install only necessary packages and clean up cache
RUN apk add --no-cache curl

# Copy app files
WORKDIR /app
COPY hello.sh /app/hello.sh
RUN chmod +x /app/hello.sh

# Switch to non-root user
USER app

# Define entrypoint
ENTRYPOINT ["/app/hello.sh"]
