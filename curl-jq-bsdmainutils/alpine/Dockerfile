FROM curlimages/curl:latest AS builder

###############################################################
# pinning image to Alpine 3.15.4
###############################################################
FROM alpine:latest

RUN apk update
RUN apk upgrade
RUN apk add --no-cache brotli brotli-dev libssh2 nghttp2-dev jq util-linux && \
    rm -fr /var/cache/apk/*

###############################################################
# install curl built from builder
###############################################################
COPY --from=builder "/usr/lib/libcurl.so.4.8.0" "/usr/lib/"
COPY --from=builder "/usr/bin/curl" "/usr/bin/curl"
COPY --from=builder "/usr/include/curl" "/usr/include/curl"

# explicitly set symlinks
RUN ln -s /usr/lib/libcurl.so.4.8.0 /usr/lib/libcurl.so.4
RUN ln -s /usr/lib/libcurl.so.4 /usr/lib/libcurl.so

###############################################################
# set entrypoint
###############################################################
COPY "entrypoint.sh" "/entrypoint.sh"
ENTRYPOINT ["sh", "/entrypoint.sh"]

#jq util-linux