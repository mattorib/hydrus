FROM m1k1o/neko:base

HEALTHCHECK --interval=20s --timeout=10s --retries=3 --start-period=30s CMD ! supervisorctl status | grep -v RUNNING
ENTRYPOINT ["/bin/bash", "/opt/hydrus/static/build_files/docker/client/entrypoint.sh"]
LABEL git="https://github.com/hydrusnetwork/hydrus"

RUN set -eux; apt-get update; \
  apt-get install libmpv1 ffmpeg openbox python3 python3-pip libxkbcommon-x11-0 libxcb-cursor0 libxcb-icccm4 libxcb-keysyms1 libxcb-randr0 libxcb-cursor0 -y; \
  apt-get clean -y; \
  rm -rf /var/lib/apt/lists/* /var/cache/apt/* /tmp/*;
COPY ./static/build_files/linux/requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

RUN mkdir -p /opt/hydrus

COPY --chown=hydrus . /opt/hydrus
COPY --chown=hydrus --from=suika/swftools:2013-04-09-1007 /swftools/swfrender /opt/hydrus/bin/swfrender_linux

COPY ./static/build_files/docker/client/supervisord.conf /etc/neko/supervisord/hydrus.conf
COPY ./static/build_files/docker/client/openbox.xml /etc/neko/openbox.xml

RUN ln -fs /usr/bin/python3 /usr/bin/python && ln -fs /usr/bin/pip3 /usr/bin/pip

VOLUME /opt/hydrus/db

ENV UID=${UID:-1000} \
    GID=${GID:-1000} \
    DB_DIR=/opt/hydrus/db \
    HYDRUS_EXTRA=""
