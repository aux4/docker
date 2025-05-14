ARG base=aux4/aux4
FROM ${base}

ARG systemPackageManager=apt
ARG systemPackageManagerUpdate=update
ARG systemPackageManagerInstall=install

RUN ${systemPackageManager} ${systemPackageManagerUpdate} && \
    ${systemPackageManager} ${systemPackageManagerInstall} -y jq

RUN if [[ -n "${systemPackages}" ]]; then \
      for pkg in ${systemPackages}; do \
        ${systemPackageManager} ${systemPackageManagerInstall} -y "$pkg"; \
      done; \
    fi

WORKDIR /home/aux4

ARG scope
ARG name
ARG package=-

COPY ${package}* /home/aux4/

ARG packages=""
RUN if [[ -n "${packages}" ]]; then \
      for pkg in ${packages}; do \
        aux4 aux4 pkger install "$pkg"; \
      done \
    ; fi

RUN aux4 aux4 pkger install aux4/system-installer-npm
RUN aux4 aux4 pkger install aux4/test

RUN if [[ "${package}" != "-" ]]; then \
      aux4 aux4 pkger install --fromFile $(ls /home/aux4/*.zip); \
      cp -r /home/aux4/.aux4.config/packages/${scope}/${name}/test/. /home/aux4/test/ 2> /dev/null || true \
    ; fi

RUN mkdir -p /home/aux4/test

ARG aux4=-none
COPY ${aux4}* /home/aux4/.aux4

ARG test=-test
COPY ${test}* /home/aux4/test

WORKDIR /home/aux4/test

ENTRYPOINT ["aux4", "test", "run"]
