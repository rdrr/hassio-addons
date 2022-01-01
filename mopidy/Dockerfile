ARG BUILD_FROM

FROM $BUILD_FROM AS build-base

RUN apk add --no-cache --repository=https://dl-cdn.alpinelinux.org/alpine/edge/community --repository=https://dl-cdn.alpinelinux.org/alpine/edge/main \
      python3=3.10.1-r0 \
      py3-gst=1.18.5-r1 \
      py3-pip=21.3.1-r0 \
      mopidy=3.2.0-r4 \
      && ln -s /usr/lib/libpython3.10.so.1.0 /usr/lib/libpython3.10.so


FROM build-base AS mopidy-iris

RUN pip3 install --no-cache-dir --user Mopidy-Iris==3.60.1 &&\
      ls -al /root/.local/lib/python3.10/site-packages


FROM build-base AS mopidy-mpd

RUN pip3 install --no-cache-dir --user Mopidy-MPD==3.2.0 &&\
      ls -al /root/.local/lib/python3.10/site-packages


FROM build-base AS mopidy-TuneIn

RUN pip3 install --no-cache-dir --user Mopidy-TuneIn==1.1.0 &&\
      ls -al /root/.local/lib/python3.10/site-packages


FROM build-base AS mopidy-dLeyna

RUN pip3 install --no-cache-dir --user Mopidy-dLeyna==2.0.1 &&\
      ls -al /root/.local/lib/python3.10/site-packages


FROM build-base AS mopidy-SomaFM

RUN pip3 install --no-cache-dir --user Mopidy-SomaFM==2.0.2 &&\
      ls -al /root/.local/lib/python3.10/site-packages


FROM build-base AS mopidy-Muse

RUN pip3 install --no-cache-dir --user Mopidy-Muse==0.0.27 &&\
      ls -al /root/.local/lib/python3.10/site-packages


FROM $BUILD_FROM AS run

RUN apk add --no-cache --repository=https://dl-cdn.alpinelinux.org/alpine/edge/community --repository=https://dl-cdn.alpinelinux.org/alpine/edge/main \
      python3=3.10.1-r0 \
      py3-gst=1.18.5-r1 \
      mopidy=3.2.0-r4 \
      ca-certificates=20191127-r7 \
      sudo=1.9.8_p2-r1 \
      && ln -s /usr/lib/libpython3.10.so.1.0 /usr/lib/libpython3.10.so

COPY --from=mopidy-iris /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/
COPY --from=mopidy-mpd /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/
COPY --from=mopidy-dleyna /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/
COPY --from=mopidy-somafm /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/
COPY --from=mopidy-muse /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/

RUN echo "root ALL=NOPASSWD: /usr/lib/python3.10/site-packages/mopidy_iris/system.sh" >> /etc/sudoers

ENTRYPOINT [ "/init" ]
CMD []
COPY root /
