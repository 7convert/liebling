FROM node:12-alpine
LABEL maintainer="yss2002@gmail.com"

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV LC_ALL=en_US.UTF-8

RUN echo '' > /etc/apk/repositories && \
    echo "https://mirror.tuna.tsinghua.edu.cn/alpine/v3.10/main"         >> /etc/apk/repositories && \
    echo "https://mirror.tuna.tsinghua.edu.cn/alpine/v3.10/community"    >> /etc/apk/repositories && \
    echo "Asia/Shanghai" > /etc/timezone

RUN apk update && apk add git && \
    yarn global add knex-migrator grunt-cli ember-cli bower

RUN git config --global http.postBuffer 524288000 && \
    git clone --depth 1 --recurse-submodules https://github.com/7convert/Ghost.git  /var/lib/ghost && \
    cd /var/lib/ghost && \
    yarn setup

WORKDIR /var/lib/ghost

RUN grunt build

EXPOSE 2368

CMD ["npm", "start"]