FROM golang:1.25-alpine AS builder
WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /app/backend ./cmd/api

FROM alpine:3.22
RUN apk add --no-cache ca-certificates && update-ca-certificates
RUN adduser -D -g '' backenduser

WORKDIR /app
COPY --from=builder /app/backend /app/backend

USER backenduser
EXPOSE 8081
ENV GIN_MODE=release
ENTRYPOINT ["/app/backend"]
