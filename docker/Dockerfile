## PLEASE DON'T CHANGE ANYTHING IN THIS FILE UNLESS YOU'RE developing.
## REFER TO THE DOCUMENTATION FOR CONFIGURATION OF YOUR HAMCLOCK!
FROM alpine:3.23
ARG HC_SIZE

USER root
WORKDIR /root/hamclock
# Install prerequisites
RUN apk update && \
    apk add curl make g++ libx11-dev openssl unzip perl && \
    rm -rf /var/cache/apk/*

# Install Hamclock
COPY ESPHamClock/ ESPHamClock/
RUN cd ESPHamClock && \
    if [ -n "$HC_SIZE" ]; then \
        make -j 4 hamclock-web-${HC_SIZE} && \
        make install && \
        make clean; \
    else \
		for size in 800x480 1600x960 2400x1440 3200x1920; do \
            make -j 4 hamclock-web-$size && \
            cp hamclock-web-$size /usr/local/bin/ && \
            make clean; \
        done; \
        chown root /usr/local/bin/hamclock-web-* && \
        chmod u+s /usr/local/bin/hamclock-web-*; \
    fi

COPY hamclock-contrib/ hamclock-contrib/
# Install Hamclock Contrib and move hceeprom to hamclock directory
RUN cp hamclock-contrib/hceeprom.pl /root/hamclock/ESPHamClock && \
    chmod +x /root/hamclock/ESPHamClock/hceeprom.pl

# Copy runfile
COPY docker/run.sh ./
COPY docker/backend_host ./
RUN chmod +x run.sh

WORKDIR /root/hamclock/ESPHamClock
CMD ["/root/hamclock/run.sh"]
