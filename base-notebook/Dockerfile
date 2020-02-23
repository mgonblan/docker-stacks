# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM --platform=$BUILDPLATFORM bnxtipro/miniconda:latest AS base
LABEL MAINTAINER="Manuel Gonzalez Blanco <manuel.gonzalez.freelancer@gmail.com>"
ARG TARGETPLATFORM
ARG BUILDPLATFORM
ARG NB_USER="jovyan"
ARG NB_GROUP="conda"
ENV PATH=$CONDA_DIR/bin:$PATH \
    HOME=/home/$NB_USER
USER root
RUN chown -R ${NB_USER}:$(id -gn ${NB_USER}) ${HOME}/.config && \
    chown -R ${NB_USER}:$(id -gn ${NB_USER}) ${HOME} && \
    chmod -R u+rw ${HOME}
USER $NB_USER
RUN conda install -c conda-forge jupyterlab jupyterhub && mkdir -p ${HOME}/work && \
    cd ${HOME}/work && \
    jupyter notebook --generate-config && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    npm cache clean --force && \
    conda clean -afy && \
    rm -rf /home/$NB_USER/.cache/yarn 
EXPOSE 8888
CMD [ "start-notebook.sh" ]
# Copy local files as late as possible to avoid cache busting
COPY start.sh /usr/local/bin/
COPY start-notebook.sh /usr/local/bin/
COPY start-singleuser.sh /usr/local/bin/
COPY jupyter_notebook_config.py /etc/jupyter/
