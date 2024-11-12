# Stage 1: Build Go application
FROM golang:1.19 AS builder

# Set working directory and copy files
WORKDIR /app
COPY . .

# Build the Go application
RUN go mod download
RUN go build -o main .

# Stage 2: Minimal runtime environment
FROM alpine:latest

# Copy the built binary from the builder stage
COPY --from=builder /app/main /app/main

# Expose port if necessary
EXPOSE 8080

# Run the application
CMD ["/app/main"]
