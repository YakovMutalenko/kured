FROM golang as builder
RUN go get github.com/golang/dep/cmd/dep
WORKDIR /go/src/github.com/brantb/kured
COPY . .
RUN dep ensure -v && make

FROM ubuntu:16.04
RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/cache/apt
# NB: you may need to update RBAC permissions when upgrading kubectl - see kured-rbac.yaml for details
ADD https://storage.googleapis.com/kubernetes-release/release/v1.10.3/bin/linux/amd64/kubectl /usr/bin/kubectl
RUN chmod 0755 /usr/bin/kubectl
COPY --from=builder /go/src/github.com/brantb/kured /usr/bin/kured
ENTRYPOINT ["/usr/bin/kured"]
