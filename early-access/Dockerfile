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

FROM ubuntu:12.04

MAINTAINER Arthur Barr <arthur.barr@uk.ibm.com>

ENV MQLIGHT_DATA_PATH /var/mqlight

COPY mqlight /usr/local/bin/

RUN chmod +x /usr/local/bin/mqlight && \
    export DEBIAN_FRONTEND=noninteractive && \
    apt-get update -y && apt-get install -y \
        bash \
        curl \
        net-tools \
        tar && \
    mkdir -p /opt/mqlight /var/mqlight && \
    MQLIGHT_URL=https://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/messaging/mqkoa && \
    MQLIGHT_PKG=IBM-MQ-Light-Linux-x64-early-access-L150205.1.tar.gz && \
    curl -sL $MQLIGHT_URL/$MQLIGHT_PKG | tar -zx --strip-components 1 -C /opt/mqlight && \
    /usr/local/bin/mqlight initialize && \
    apt-get --purge remove --auto-remove curl -y && \
    apt-get clean autoclean -y && \
    rm -rf /tmp/* \
        /var/cache/apt/* \
        /var/lib/apt/lists/* \
        /var/lib/cache/* \
        /var/lib/log/* \
        /var/tmp/*

VOLUME ${MQLIGHT_DATA_PATH}

EXPOSE 5672 5671 9180 9181

ENTRYPOINT ["mqlight"]
