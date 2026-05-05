ARG base=aux4/aws-lambda
FROM ${base}

USER 0

ARG packages=""
RUN if [[ -n "${packages}" ]]; then \
      for pkg in ${packages}; do \
        aux4 aux4 pkger install "$pkg"; \
      done \
    ; fi

ARG aux4=-none
COPY ${aux4}* ${LAMBDA_TASK_ROOT}/.aux4

ARG mode=""
ENV AUX4_LAMBDA_MODE=${mode}

RUN chown -R aux4:aux4 /var/task
USER aux4

ARG command=""
CMD ${command}
