FROM jupyter/base-notebook:04ec7b772984

LABEL authors="Mark S. Bartlett"
LABEL maintainer="bartlett <mark.bartlettjr@Stantec.com>"

# system environment
COPY stantec-ca.crt /usr/local/share/ca-certificates/
COPY stantec-ca.crt /etc/ssl/certs/ca-certificates/

#Add User
USER root

RUN chmod -R a+rwx $HOME
RUN chown -R $NB_USER .

RUN chmod 644 /usr/local/share/ca-certificates/stantec-ca.crt \
    && chmod 644 /etc/ssl/certs/ca-certificates/stantec-ca.crt \
    && update-ca-certificates


RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    # ---- nbconvert dependencies ----
    texlive-xetex \
    texlive-fonts-recommended \
    texlive-plain-generic && \
    # -----additional tex packages ----
    #texlive-science && \ 
    #texlive-latex-extra && \
    apt-get autoremove && \
    apt-get clean

# switch the user
USER $NB_USER

COPY --chown=${NB_UID}:${NB_GID} requirements_conda.txt /tmp/
COPY --chown=${NB_UID}:${NB_GID} requirements_pip.txt /tmp/

ENV REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt

#Install new pacakges
RUN conda config --set ssl_verify /usr/local/share/ca-certificates/stantec-ca.crt && \
    conda install --yes --file /tmp/requirements_conda.txt && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

RUN pip install --no-cache-dir --upgrade --upgrade-strategy only-if-needed --cert /usr/local/share/ca-certificates/stantec-ca.crt --requirement /tmp/requirements_pip.txt && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

EXPOSE 8050

#RUN npm config set cafile /usr/local/share/ca-certificates/stantec-ca.crt --global && \
RUN npm config set "strict-ssl" false -g && \
    jupyter labextension install @jupyter-widgets/jupyterlab-sidecar@^0.6.0 && \
    jupyter labextension install @jupyterlab/latex && \
    jupyter lab build && \
    jupyter lab clean

COPY jupyter_notebook_config.py /etc/jupyter/

# Fix permissions on /etc/jupyter as root
USER root

# Prepare upgrade to JupyterLab V3.0 #1205
RUN sed -re "s/c.NotebookApp/c.ServerApp/g" \
    /etc/jupyter/jupyter_notebook_config.py > /etc/jupyter/jupyter_server_config.py && \
    fix-permissions /etc/jupyter/

# Switch back to jovyan to avoid accidental container runs as root
USER ${NB_UID}



