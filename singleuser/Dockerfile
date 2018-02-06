# Build as giffordlab/jupyter-singleuser
# Run with the DockerSpawner in JupyterHub

FROM giffordlab/datascience-notebook

MAINTAINER Matt Edwards <matted@mit.edu>

USER root
# fetch juptyerhub-singleuser entrypoint

# Get the script at the commit for Jupyterhub release 0.6.1..
RUN wget -q https://raw.githubusercontent.com/jupyterhub/jupyterhub/94978ea9e0e9f16415e34606fcf30d77c04018e4/scripts/jupyterhub-singleuser \
        -O /usr/local/bin/jupyterhub-singleuser && \
    chmod 755 /usr/local/bin/jupyterhub-singleuser

ADD singleuser.sh /srv/singleuser/singleuser.sh
USER jovyan
# smoke test that it's importable at least
# RUN sh /srv/singleuser/singleuser.sh -h
CMD ["sh", "/srv/singleuser/singleuser.sh"]
