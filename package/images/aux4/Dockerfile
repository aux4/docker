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

WORKDIR /home/aux4/workspace
ENV HOME=/home/aux4

ARG version=latest

RUN curl "https://aux4.sh?version=${version}" | sh \
  && source /home/aux4/.bashrc

ENV PATH="/home/aux4/.aux4.config/packages/aux4/aux4:${PATH}"

RUN curl https://pkger.aux4.sh | sh
RUN aux4 aux4 pkger install aux4/system-installer-${systemPackageManager}
RUN aux4 aux4 pkger install aux4/config

ARG packages=""
RUN if [[ -n "${packages}" ]]; then \
      for pkg in ${packages}; do \
        aux4 aux4 pkger install "$pkg"; \
      done \
    ; fi

ARG aux4=-none
COPY ${aux4}* /home/aux4/workspace/.aux4

# Create aux4 user and group
RUN groupadd -r aux4 && \
    useradd -r -g aux4 -d /home/aux4 -s /bin/bash aux4

# Set proper ownership and permissions
RUN chown -R aux4:aux4 /home/aux4 && \
    # Make .aux4.config read-only for the aux4 user
    chmod -R 755 /home/aux4/.aux4.config && \
    # Remove write permissions for owner on .aux4.config
    chmod -R a-w /home/aux4/.aux4.config && \
    # Ensure aux4 user owns their home but can't modify .aux4.config
    chmod 755 /home/aux4

# Switch to aux4 user
USER aux4

ENTRYPOINT ["aux4"]
