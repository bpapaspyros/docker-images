FROM ghcr.io/aica-technology/control-libraries:v9.2.0 AS cl
FROM ghcr.io/aica-technology/ros2-ws:jazzy AS poco

RUN sudo apt-get update && sudo apt-get install -y \
    libpoco-dev \
    && sudo rm -rf /var/lib/apt/lists/*

FROM poco AS deps
COPY --from=cl / /

RUN git clone https://github.com/ros-industrial/abb_libegm && \
  cmake -B build/egm -S abb_libegm -DCMAKE_INSTALL_PREFIX=/tmp/deps -DCMAKE_BUILD_TYPE=Release && \
  sudo cmake --build build/egm --target all install

RUN git clone https://github.com/ros-industrial/abb_librws && \
  cmake -B build/rws -S abb_librws -DCMAKE_INSTALL_PREFIX=/tmp/deps -DCMAKE_BUILD_TYPE=Release && \
  sudo cmake --build build/rws --target all install

RUN git clone https://github.com/ros-industrial/abb_egm_rws_managers && \
  cmake -B build/managers -S abb_egm_rws_managers -DCMAKE_INSTALL_PREFIX=/tmp/deps -DCMAKE_BUILD_TYPE=Release && \
  sudo cmake --build build/managers --target all install

FROM poco
COPY --from=deps /tmp/deps /usr
