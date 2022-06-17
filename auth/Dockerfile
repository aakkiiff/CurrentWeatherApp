FROM golang:alpine as builder

WORKDIR /app

COPY . .

WORKDIR /app/src/main

RUN go build -o /go/bin/app ./...


FROM alpine:latest

COPY --from=builder /go/bin/app /app
ENTRYPOINT [ "/app" ] 

EXPOSE 8080