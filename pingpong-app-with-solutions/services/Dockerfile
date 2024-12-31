# Step 1: Build the Go binary
FROM golang:1.22-alpine AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy the Go modules manifests
COPY go.mod go.sum ./

# Download the dependencies (and cache them)
RUN go mod tidy

# Copy the source code into the container
COPY . .

# Build the Go application
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -o pingpong .

# Step 2: Create the final image
FROM alpine:latest

# Install required dependencies (e.g., for HTTP and Prometheus handling)
RUN apk --no-cache add ca-certificates

# Create a non-root user and group
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set the working directory for the application
WORKDIR /app

# Copy the binary from the builder stage to the final image
COPY --from=builder /app/pingpong .

# Copy the environment file (if necessary) or any additional resources
COPY .env ./

# Change ownership of the files to the non-root user
RUN chown -R appuser:appgroup /app

# Switch to the non-root user
USER appuser

# Expose the necessary ports (Service port and Metrics port)
EXPOSE 8080 9080

# Set the entry point to run the application
CMD ["./pingpong"]