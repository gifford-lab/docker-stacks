# Build as giffordlab/jupyter-singleuser
# Run with the DockerSpawner in JupyterHub

FROM giffordlab/datascience-notebook

MAINTAINER Matt Edwards <matted@mit.edu>

USER root

# Get the script at the commit for Jupyterhub release 0.7.0..
RUN wget -q https://raw.githubusercontent.com/jupyterhub/jupyterhub/c3faef8e2ae011f6650730804a2fe88ec5b6f572/scripts/jupyterhub-singleuser \
        -O /usr/local/bin/jupyterhub-singleuser && \
    chmod 755 /usr/local/bin/jupyterhub-singleuser

ADD install_jupyterhub /tmp/install_jupyterhub
ARG JUPYTERHUB_VERSION=master
# install pinned jupyterhub and ensure notebook is installed
RUN python3 /tmp/install_jupyterhub && \
    python3 -m pip install notebook

ADD singleuser.sh /srv/singleuser/singleuser.sh
USER jovyan
# smoke test that it's importable at least
# RUN sh /srv/singleuser/singleuser.sh -h
CMD ["sh", "/srv/singleuser/singleuser.sh"]
