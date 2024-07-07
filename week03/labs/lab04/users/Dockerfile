FROM golang:1.22.5-alpine3.20 AS build

RUN mkdir /app
ADD . /app
WORKDIR /app

RUN go mod tidy -compat=1.22
RUN go build -o main ./cmd/main.go

FROM alpine
WORKDIR /app
RUN mkdir /configs
COPY --from=build /app/configs/app.env /configs
RUN mkdir /keys
COPY --from=build /app/keys/. /keys
COPY --from=build /app/main /app/main
CMD ["/app/main"]