FROM node:12-buster-slim
LABEL maintainer="yss2002@gmail.com"

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV LC_ALL=en_US.UTF-8

ENV NODE_ENV development
ENV GHOST_INSTALL /var/lib/ghost
ENV GHOST_CONTENT /var/lib/ghost/content

RUN echo "" > /etc/apt/sources.list && \
    echo "deb http://mirrors.ustc.edu.cn/debian stable  main non-free contrib"   >> /etc/apt/sources.list && \
    echo "deb http://mirrors.ustc.edu.cn/debian stable-updates main non-free contrib"  >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian/ buster main non-free contrib"   >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian/ buster main non-free contrib"  >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian-security buster/updates main"  >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian-security buster/updates main"  >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib"  >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian/ buster-updates main non-free contrib"  >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib"  >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.aliyun.com/debian/ buster-backports main non-free contrib"  >> /etc/apt/sources.list && \
    echo "Asia/Shanghai" > /etc/timezone

RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates apt-transport-https git && \
    yarn config set registry https://registry.npm.taobao.org && \
    yarn global add knex-migrator grunt-cli ember-cli bower

RUN git config --global http.postBuffer 524288000 && \
    git clone --depth=1 --verbose https://github.com/7convert/Ghost.git  "$GHOST_INSTALL" && \
    cd "$GHOST_INSTALL" && \
    yarn install

RUN git config --global http.postBuffer 524288000 && \
    git clone --depth=1 --verbose https://github.com/TryGhost/Casper.git  "$GHOST_CONTENT/themes/casper" && \
    cd "$GHOST_CONTENT/themes/casper" && \
    yarn install

RUN apt-get install -y --no-install-recommends ssh net-tools vim


RUN mkdir /root/.ssh && ssh-keyscan github.com >> /root/.ssh/known_hosts && \
    cat /data/liebling/src/id_rsa > /root/.ssh/id_rsa

RUN git clone --depth=1 --verbose git@github.com:7convert/Ghost-Admin.git  "$GHOST_INSTALL/core/client" && \
    cd "$GHOST_INSTALL/core/client" && \
    yarn install

WORKDIR $GHOST_INSTALL

EXPOSE 2368

CMD ["node", "index.js"]