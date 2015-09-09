FROM node:0.12-onbuild

EXPOSE 3000

ENV APP_NAME runverter
ENV REDIS_PORT 6379
ENV REDIS_HOST runverter-redis
ENV REDIS_SECRET ""

CMD [ "node", "--harmony", "index.js" ]