# Build as giffordlab/jupyter-systemuser
# Run with the DockerSpawner in JupyterHub

FROM giffordlab/jupyter-singleuser

MAINTAINER Matt Edwards <matted@mit.edu>

USER root
ENV SHELL /bin/bash
ADD systemuser.sh /srv/singleuser/systemuser.sh

CMD ["sh", "/srv/singleuser/systemuser.sh"]
