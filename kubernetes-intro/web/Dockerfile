FROM python:3.9-alpine

WORKDIR /app

ENV UID=1001
ENV GID=1001
RUN addgroup -S server -g ${GID} \
    && adduser -G server -u ${UID} -S -H -D server

USER server

EXPOSE 8000

CMD [ "python", "-m", "http.server", "8000" ]