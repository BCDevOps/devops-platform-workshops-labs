FROM registry.access.redhat.com/rhscl/nodejs-8-rhel7

ENV RC_VERSION 0.69.2

MAINTAINER buildmaster@rocket.chat

LABEL name="Rocket.Chat" \
      vendor="Rocket.Chat" \
      version="${RC_VERSION}" \
      release="1" \
      url="https://rocket.chat" \
      summary="The Ultimate Open Source Web Chat Platform" \
      description="The Ultimate Open Source Web Chat Platform" \
      run="docker run -d --name ${NAME} ${IMAGE}"




# RUN set -x \
#  && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys 0E163286C20D07B9787EBE9FD7F9D0414FD08104 \
#  && curl -SLf "https://releases.rocket.chat/${RC_VERSION}/download" -o rocket.chat.tgz \
#  && curl -SLf "https://releases.rocket.chat/${RC_VERSION}/asc" -o rocket.chat.tgz.asc \
#  && gpg --verify rocket.chat.tgz.asc \
#  && tar -zxf rocket.chat.tgz -C /opt/app-root/src/ \
#  && cd /opt/app-root/src/bundle/programs/server \
#  && npm install

RUN : \
 && source scl_source enable rh-nodejs8 \
 && set -x \
 && curl -SLf "https://releases.rocket.chat/${RC_VERSION}/download" -o rocket.chat.tgz \
 && tar -zxf rocket.chat.tgz -C /opt/app-root/src/ \
 && cd /opt/app-root/src/bundle/programs/server \
 && npm ci \
 && rm -rf /opt/app-root/src/rocket.chat.tgz
 

COPY licenses /licenses

VOLUME /opt/app-root/src/uploads

WORKDIR /opt/app-root/src/bundle

# Hack needed to force use of bundled library instead of system level outdated library
# https://github.com/lovell/sharp/issues/892
ENV LD_PRELOAD=/opt/app-root/src/bundle/programs/server/npm/node_modules/sharp/vendor/lib/libz.so

ENV DEPLOY_METHOD=docker-redhat \
    NODE_ENV=production \
    MONGO_URL=mongodb://dbuser:dbpassword@mongo:27017/rocketchat \
    HOME=/tmp \
    PORT=3000 \
    ROOT_URL=http://localhost:3000

EXPOSE 3000

CMD ["bash", "-c" "exec node main.js"]
