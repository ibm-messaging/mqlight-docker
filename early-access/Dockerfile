# © Copyright IBM Corporation 2015.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM ubuntu:14.04

MAINTAINER David McCann <mccannd@uk.ibm.com>

ENV MQLIGHT_DOCKER_USER user
ENV MQLIGHT_DOCKER_GROUP staff
ENV MQLIGHT_DOCKER_HOME /home/${MQLIGHT_DOCKER_USER}
ENV MQLIGHT_HOME /opt/mqlight
ENV MQLIGHT_DATA_PATH /var/mqlight

RUN useradd -u 1000 --create-home --home-dir ${MQLIGHT_DOCKER_HOME} -g ${MQLIGHT_DOCKER_GROUP} -G sudo ${MQLIGHT_DOCKER_USER}
WORKDIR ${MQLIGHT_DOCKER_HOME}

RUN set -x \
        && export DEBIAN_FRONTEND=noninteractive \
        && apt-get update -qqy \
        && apt-get install -qqy --no-install-recommends \
                bash \
                bc \
                coreutils \
                curl \
                net-tools \
                openjdk-7-jre \
                sudo \
                tar \
                >/dev/null \
        && sed -e 's/^%sudo	.*/%sudo	ALL=NOPASSWD:ALL/g' -i /etc/sudoers \
        && mkdir -p ${MQLIGHT_HOME} ${MQLIGHT_DATA_PATH} \
        && chown ${MQLIGHT_DOCKER_USER}:${MQLIGHT_DOCKER_GROUP} ${MQLIGHT_HOME} ${MQLIGHT_DATA_PATH} \
        && apt-get clean autoclean -y \
        && rm -rf /tmp/* \
                  /var/cache/apt/* \
                  /var/lib/apt/lists/* \
                  /var/lib/cache/* \
                  /var/lib/log/* \
                  /var/tmp/*

ENV MQLIGHT_VERSION early-access-L150326.1
ENV MQLIGHT_URL http://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/messaging/mqkoa/IBM-MQ-Light-Linux-x64-${MQLIGHT_VERSION}.tar.gz

COPY mqlight /usr/local/bin/

RUN set -x \
        && chmod +x /usr/local/bin/mqlight \
        && su -c "curl -sL ${MQLIGHT_URL} | tar -zx --strip-components 1 -C ${MQLIGHT_HOME}" ${MQLIGHT_DOCKER_USER} \
        && rm -rf "${MQLIGHT_HOME}/runtime/java/jre64/jre" \
        && ln -s /usr/lib/jvm/java-7-openjdk-amd64/jre "${MQLIGHT_HOME}/runtime/java/jre64/" \
        && sed -e 's/-Xquickstart //' -i "${MQLIGHT_HOME}/runtime/bin/lap.sh" \
        && sed -e '145s/Darwin/Linux/' -i "${MQLIGHT_HOME}/runtime/mqxr/bin/controlMQXR_mqm.sh" \
        && su -c "/usr/local/bin/mqlight initialize" ${MQLIGHT_DOCKER_USER}

EXPOSE 5672 5671 9180 9181
VOLUME ${MQLIGHT_DATA_PATH}
USER ${MQLIGHT_DOCKER_USER}
ENTRYPOINT ["mqlight"]
