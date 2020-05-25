From golang:1.13 AS base

ENV GO111MODULE=on
ENV GOPATH=/home/alessio/golang
WORKDIR /go/src/app
ADD main.go go.mod go.sum /go/src/app/


# Get dependancies - will also be cached if we won't change mod/sum
RUN go mod download
# COPY the source code as the last step
COPY . .

# CGO_ENABLED=0 disables cgo - no cross-compiled dependencies.
# -a forces a rebuild.
# -ldflags -w disables debug.
# -extldflags "-static" 
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 GOPATH=/home/alessio/golang go \
    build -a -ldflags='-w -extldflags "-static"' \
    -o /go/bin/app


FROM scratch
COPY --from=base /go/bin/app /go/bin/app
ENTRYPOINT ["/go/bin/app"]