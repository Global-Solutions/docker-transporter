FROM gsol/transporter
MAINTAINER Global Solutions co., ltd.
LABEL version="0.1.0"

ENV PKG_PATH=github.com/dshearer/jobber \
    JOBBER_USER=jobber_client \
    JOBBER_UID=1000 \
    JOBBER_BIN=${GOPATH}/bin/jobber \
    JOBBERD_BIN=${GOPATH}/bin/jobberd \
    TINI_VERSION=0.9.0
ENV JOBBER_HOME=/home/${JOBBER_USER} \
    JOBBER_GROUP=${JOBBER_USER} \
    JOBBER_GID=${JOBBER_UID} \
    TINI_DL=https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini-static \
    TINI_PATH=/usr/local/bin/tini

WORKDIR ${GOPATH}
RUN apk add --no-cache --virtual .deps git go=${GOVERSION} make curl && \
    go get ${PKG_PATH} && ls && \
    cd src/${PKG_PATH} && \
    make GO_WKSPC=${GOPATH} && \
    curl -fsSL ${TINI_DL} -o ${TINI_PATH} && \
    chmod +x ${TINI_PATH} && \
    addgroup -g ${JOBBER_GID} ${JOBBER_GROUP} && \
    adduser -u ${JOBBER_UID} -G ${JOBBER_GROUP} -S ${JOBBER_USER} && \
    chmod 04755 ${JOBBER_BIN} && \
    chown ${JOBBER_USER}:root ${JOBBER_BIN} && \
    chmod 0755 ${JOBBERD_BIN} && \
    cd ${GOPATH} && rm src pkg -rf && \
    apk del .deps

ENTRYPOINT ["tini", "jobberd"]
