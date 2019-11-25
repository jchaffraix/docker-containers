##############################
# Step 1: Build the executable
##############################
FROM alpine:latest AS builder

# musl-dev required for some system library (e.g. thread)
RUN apk --no-cache --update add musl-dev go git

# Build the main.
WORKDIR build/
COPY . .

# Fetch the dependencies
# TODO: Use go mod?
# TODO: My structure doesn't work with go get.
#RUN go get -d -v

# Build the binary
RUN go build -o /go/bin/main src/main.go

####################################
# Step 2: Build the actual container
####################################
FROM alpine:latest

# TODO: Do I need musl-dev and go?
RUN apk --no-cache --update add ca-certificates

# Create a non-priviledged system user
RUN addgroup -S www
RUN adduser -S -G www www

# Copy the binary and change the permission
COPY --from=builder /go/bin/main /data/main
RUN chown -R www:www /data

USER www
WORKDIR /data

CMD /data/main
