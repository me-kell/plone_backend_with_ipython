FROM plone/server-dev:6.1.1 AS builder

RUN gosu plone /app/bin/pip install ipython==9.2.0

COPY --chown=500:500 --chmod=755 ipythonconsole /app/bin/ipythonconsole

RUN sed -i '/^elif  \[\[ "$1" == "console" ]]; then/i elif  \[\[ "$1" == "ipythonconsole" ]]; then\n \ \ exec $sudo $VENVBIN/ipythonconsole etc/${CONF}' /app/docker-entrypoint.sh
