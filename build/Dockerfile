FROM docker.io/zmkfirmware/zmk-dev-arm:3.2 as builder

RUN git clone https://github.com/zmkfirmware/zmk /zmk
WORKDIR /zmk

RUN \
  west init -l app && \
  west update && \
  west zephyr-export && \
  pip3 install --user -r /zmk/app/scripts/requirements.txt

COPY config /board/config

FROM builder as left
RUN west build \
  -d /build \
  -s /zmk/app \
  -b "nice_nano_v2" \
  -- \
  -DZMK_CONFIG=/board/config \
  -DSHIELD="corne_left"

FROM builder as right
RUN west build \
  -d /build \
  -s /zmk/app \
  -b "nice_nano_v2" \
  -- \
  -DZMK_CONFIG=/board/config \
  -DSHIELD="corne_right"

FROM scratch as artifacts
COPY --from=left /build/zephyr/zmk.uf2 /left.uf2
COPY --from=right /build/zephyr/zmk.uf2 /right.uf2
