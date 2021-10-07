Docker Setup
==============================


## Build and test with Docker

This project has been containerized using Docker. To build and test code in the Docker container, first install:

* Docker desktop - https://www.docker.com/products/docker-desktop (once installed, adjust Docker settings to increase max available CPU to at least 8 and max available memory to at least 8GB)
* Windows sub-Linux SYstem - https://docs.microsoft.com/en-us/windows/wsl/install-win10 (note that this only requires completely steps up to and including Step 5. No need to install a Linux distribution)

### Running the Container

Before running the container, open the docker-compose.yml file in the docker folder and examine if folder volumes on the container map to the correct local folders on one's computer. Note under the 'volumes' section of the docker-compose file, the folder location on the local computer is given before the colon while the location in the container is given after the colon. Just change thte location on the computer (text before the colon), as needed. Also note the port over which the Jupyter notebook is accessed (here it set as 8080). Change as necessary.

To run the container, open the command line prompt or git bash and
- Change directory to the docker folder, where the docker-compose.yml file is located 
- Run the command 'docker-compose up --build' to run the docker container

Once the container is running, one uses Chrome or another web browser to work with the code through one of three options:  
* Jupyter Lab -- http://localhost:8080/lab
* Jupyter Notebook -- http://localhost:8080
* Nteract Notebook -- http://localhost:8080/nteract

For more information on Nteract, please refer to https://nteract.io/. Nteract was developed by Netflix https://netflixtechblog.com/notebook-innovation-591ee3221233

### Altering the Docker Container
The Docker container allows for multiple users to consistently setup and run the notebook and setup of the container is outlined in the Dockerfile (in the Docker folder). The Dockerfile starts with a base image pulled from the Jupyter Docker Stacks: https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html

For this project, the container is built upon the jupyter/scipy-notebook image, which contains

* Everything in jupyter/minimal-notebook and its ancestor images
* dask, pandas, numexpr, matplotlib, scipy, seaborn, scikit-learn, scikit-image, sympy, cython, patsy, statsmodel, cloudpickle, dill, numba, bokeh, sqlalchemy, hdf5, vincent, beautifulsoup, protobuf, xlrd, bottleneck, and pytables packages
* ipywidgets and ipympl for interactive visualizations and plots in Python notebooks
* Facets for visualizing machine learning datasets.

Any additional packages required for the project should be listed in the requirements.txt file. Listed packages will be installed with conda. In addition, if pip install is required, add a line to the Dockerfile such as 

* RUN pip install package_name --trusted-host pypi.org --trusted-host files.pythonhosted.org

In addition, this container is loaded with the latest version of GRASS GIS. Additional GRASS GIS extensions are added by 

Before adding more, container installations (e.g., GRASSS GIS), packages with the requirements.txt file, or packages through pip install, first
* Stop running the docker container with control + c
* Execute the command 'docker-compose down' from the folder where the docker-compose file resides
* Edit the requirements in the requirements.txt file or Dockerfile
* Rebuild the container with docker-compose up

## If WSL 2 is using too much memory

Please refer to here:
https://superuser.com/questions/1559170/how-can-i-reduce-the-consumption-of-the-vmmem-process

https://medium.com/@lewwybogus/how-to-stop-wsl2-from-hogging-all-your-ram-with-docker-d7846b9c5b37

## Setup NGINX server

A reverse proxy nginx server can be used to handle the communications when trying to acess Jupyter Lab from an external computer.

Effectively, many firewalls block websocket connections used by Jupter to connect to the computation kernel. The webserver Nginx acts as a reverse proxy and makes a regular HTTP connection and then upgrades the connection (on the server side) to a Websocket. The browser on the client side initiates the conneciton. Accordingly, the existing firewall rules do not interfere unless the firewall is setup to do aggresive application level packet inspection. 


* https://geekflare.com/setup-nginx-with-lets-encrypt-cert/
* https://gist.github.com/cboettig/8643341bd3c93b62b5c2

For installing directly inside the container explor
* https://www.programmersought.com/article/17081579616/
* https://gist.github.com/soheilhy/8b94347ff8336d971ad0

For linkng domain name to IP address:

* https://pythonforundergradengineers.com/add-ssl-and-domain-name-to-jupyterhub.html

--------



<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>
