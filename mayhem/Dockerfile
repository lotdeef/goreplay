# Build Geth in a stock Go builder container
FROM fuzzers/go-fuzz:1.2.0 as builder

ADD . goreplay
WORKDIR /go/goreplay

RUN cd proto && GO_ENABLED=0 go-fuzz-build -libfuzzer -o ../fuzz.a .
RUN clang -fsanitize=fuzzer fuzz.a -o fuzz.libfuzzer

FROM ubuntu:20.04
COPY --from=builder /go/goreplay/fuzz.libfuzzer /

# CMD ["./bitutil.libfuzzer"]