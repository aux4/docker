ARG base=ubuntu
FROM ${base}

ARG systemPackageManager=apt
ARG systemPackageManagerUpdate=update
ARG systemPackageManagerInstall=install
ARG systemPackages=""

RUN ${systemPackageManager} ${systemPackageManagerUpdate} && \
    ${systemPackageManager} ${systemPackageManagerInstall} -y curl zip unzip wget which

SHELL ["/bin/bash", "-c"]
ENV SHELL=/bin/bash

RUN if [[ -n "${systemPackages}" ]]; then \
      for pkg in ${systemPackages}; do \
        ${systemPackageManager} ${systemPackageManagerInstall} -y "$pkg"; \
      done; \
    fi

WORKDIR /home/aux4
ENV HOME=/home/aux4

ARG version=latest

RUN curl "https://aux4.sh?version=${version}" | sh \
  && source /home/aux4/.bashrc

ENV PATH="/home/aux4/.aux4.config/packages/aux4/aux4:${PATH}"

RUN curl https://dev.pkger.aux4.sh | sh
RUN aux4 aux4 pkger install aux4/system-installer-${systemPackageManager}

ARG packages=""
RUN if [[ -n "${packages}" ]]; then \
      for pkg in ${packages}; do \
        aux4 aux4 pkger install "$pkg"; \
      done \
    ; fi

ARG aux4=-none
COPY ${aux4}* /home/aux4/.aux4

ENTRYPOINT ["aux4"]
