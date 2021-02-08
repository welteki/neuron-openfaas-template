FROM openfaas/of-watchdog:0.8.2 as watchdog
FROM sridca/neuron:latest as build

WORKDIR /home/app
COPY ./function .

RUN ["neuron", "gen"]

FROM alpine:3.13

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

WORKDIR /home/app

COPY --from=build /home/app/.neuron/output public

ENV mode="static"
ENV static_path="/home/app/public"

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]
