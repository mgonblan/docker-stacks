FROM --platform=$BUILDPLATFORM buildpack-deps:testing AS miniconda
ARG TARGETPLATFORM
ARG BUILDPLATFORM
ARG NB_USER="jovyan"
ARG NB_GROUP="conda"
ENV CONDA_DIR=/usr/conda \
    SHELL=/bin/bash \
    NB_USER=$NB_USER \
    NB_GROUP=$NB_GROUP
ENV PATH=$CONDA_DIR/bin:$PATH \
    HOME=/home/$NB_USER
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && \
    apt-get install -yq --no-install-recommends locales && \
    locale-gen ${LANG} ${LC_ALL} && \
    dpkg-reconfigure locales && \
    apt-get clean && rm -rf  /var/cache/apt/archives && \
    sed -i 's/^#force_color_prompt=yes/force_color_prompt=yes/' /etc/skel/.bashrc && \
    echo "auth requisite pam_deny.so" >> /etc/pam.d/su && \
    chmod g+w /etc/passwd && \
    wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh  -O /tmp/miniconda.sh && \
    chmod u+x /tmp/miniconda.sh && \
    /tmp/miniconda.sh -f -b -p ${CONDA_DIR} && \
    rm  /tmp/miniconda.sh && \
    conda config --system --prepend channels conda-forge && \
    conda config --system --set auto_update_conda true && \
    conda config --system --set show_channel_urls true && \
    conda install --yes -c conda-forge pypy3.6 pip \
    dask lz4 nomkl && \
    conda update --all --yes && \
    ${CONDA_DIR}/bin/conda clean -afy \
    && conda clean -tipsy \
    && find ${CONDA_DIR} -type f,l -name '*.a' -delete \
    && find ${CONDA_DIR} -type f,l -name '*.pyc' -delete \
    && find ${CONDA_DIR} -type f,l -name '*.js.map' -delete \
    && find ${CONDA_DIR}/lib/python*/site-packages/bokeh/server/static -type f,l -name '*.js' -not -name '*.min.js' -delete \
    && rm -rf ${CONDA_DIR}/pkgs && \
    groupadd ${NB_GROUP} && \
    chgrp -R ${NB_GROUP} ${CONDA_DIR} && \
    chmod 770 -R ${CONDA_DIR} && \
    useradd -s ${SHELL} -G ${NB_GROUP} ${NB_USER} && \
    echo '\n' >>  ${HOME}/.config && \
    chown -R ${NB_USER}:$(id -gn ${NB_USER}) ${HOME}/.config 
