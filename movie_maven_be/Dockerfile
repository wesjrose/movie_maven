FROM golang:1.26-alpine AS build
WORKDIR /src

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go build -o /out/movie_maven_be .

FROM alpine:3.22
WORKDIR /app

# wget (from busybox) is used by the compose healthcheck.
COPY --from=build /out/movie_maven_be ./movie_maven_be
COPY openapi.yaml ./openapi.yaml

ENV LISTEN_ADDR=0.0.0.0:8001
EXPOSE 8001

CMD ["./movie_maven_be"]
