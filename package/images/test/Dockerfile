ARG base=aux4/aux4
FROM ${base}

# Switch back to root for package installation  
USER 0

# Debug: Check what user we're running as and fix apt permissions
RUN whoami && id && \
    chmod 755 /var/lib/apt /var/lib/apt/lists /var/lib/dpkg /var/cache/apt /var/cache/apt/archives && \
    ls -la /var/lib/apt/lists/

ARG systemPackageManager=apt
ARG systemPackageManagerUpdate=update
ARG systemPackageManagerInstall=install

RUN rm -rf /var/lib/apt/lists/* /var/cache/apt/archives/lock /var/lib/dpkg/lock* && \
    ${systemPackageManager} ${systemPackageManagerUpdate} && \
    ${systemPackageManager} ${systemPackageManagerInstall} -y jq

RUN if [[ -n "${systemPackages}" ]]; then \
      for pkg in ${systemPackages}; do \
        ${systemPackageManager} ${systemPackageManagerInstall} -y "$pkg"; \
      done; \
    fi

WORKDIR /home/aux4/workspace

ARG scope
ARG name
ARG package=-

COPY ${package}* /home/aux4/workspace/

ARG packages=""
RUN if [[ -n "${packages}" ]]; then \
      for pkg in ${packages}; do \
        aux4 aux4 pkger install "$pkg"; \
      done \
    ; fi

RUN aux4 aux4 pkger install aux4/system-installer-npm
RUN aux4 aux4 pkger install aux4/config
RUN aux4 aux4 pkger install aux4/test

RUN if [[ "${package}" != "-" ]]; then \
      aux4 aux4 pkger install --fromFile $(ls /home/aux4/workspace/*.zip); \
      cp -r /home/aux4/.aux4.config/packages/${scope}/${name}/test/. /home/aux4/workspace/test/ 2> /dev/null || true \
    ; fi

RUN mkdir -p /home/aux4/workspace/test

ARG aux4=-none
COPY ${aux4}* /home/aux4/workspace/.aux4

ARG test=-test
COPY ${test}* /home/aux4/workspace/test

# Create aux4 user and group (if they don't already exist)
RUN getent group aux4 || groupadd -r aux4 && \
    getent passwd aux4 || useradd -r -g aux4 -d /home/aux4 -s /bin/bash aux4

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

WORKDIR /home/aux4/workspace/test

ENTRYPOINT ["aux4", "test", "run"]
