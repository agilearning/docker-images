FROM python:3.4

MAINTAINER Adrian Liaw (Wei-Han Liaw) <adrianliaw2000@gmail.com>

RUN pip install ipython[notebook] && \
    ipython profile create && \
    mkdir workspace workspace/notebooks workspace/data

RUN (echo "require(['base/js/namespace'], function (IPython) {" && \
     echo "  IPython._target = '_self';" && \
     echo "});") \
    > /root/.ipython/profile_default/static/custom/custom.js

RUN (echo "c = get_config()" && \
     echo "headers = {'X-Frame-Options': 'ALLOWALL'}" && \
     echo "c.NotebookApp.allow_origin = '*'" && \
     echo "c.NotebookApp.allow_credentials = True" && \
     echo "c.NotebookApp.webapp_settings = {'headers': headers}" && \
     echo "c.NotebookApp.ip = '0.0.0.0'" && \
     echo "c.NotebookApp.open_browser = False" && \
     echo "from IPython.lib import passwd" && \
     echo "import os" && \
     echo "c.NotebookApp.password = passwd(os.environ.get('PASSWORD', 'jupyter'))") \
    > /root/.ipython/profile_default/ipython_notebook_config.py

ENV PASSWORD jupyter
ENV DIR /workspace

WORKDIR $DIR

EXPOSE 8888

CMD ipython notebook
