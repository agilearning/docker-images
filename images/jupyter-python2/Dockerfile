FROM python:2.7

MAINTAINER Adrian Liaw (Wei-Han Liaw) <adrianliaw2000@gmail.com>

RUN pip install ipython[notebook] && \
    ipython profile create && \
    mkdir /root/workspace /root/workspace/notebooks /root/workspace/data

RUN (echo "require(['base/js/namespace'], function (IPython) {" && \
     echo "  IPython._target = '_self';" && \
     echo "});") \
    > /root/.ipython/profile_default/static/custom/custom.js

RUN (echo "c = get_config()" && \
     echo "headers = {'Content-Security-Policy': 'frame-ancestors *'}" && \
     echo "c.NotebookApp.allow_origin = '*'" && \
     echo "c.NotebookApp.allow_credentials = True" && \
     echo "c.NotebookApp.tornado_settings = {'headers': headers}" && \
     echo "c.NotebookApp.ip = '0.0.0.0'" && \
     echo "c.NotebookApp.open_browser = False" && \
     echo "from IPython.lib import passwd" && \
     echo "import os" && \
     echo "c.NotebookApp.password = passwd(os.environ.get('PASSWORD', 'jupyter'))") \
    > /root/.ipython/profile_default/ipython_notebook_config.py

ENV PASSWORD jupyter
ENV DIR /root/workspace

WORKDIR $DIR

EXPOSE 8888

CMD echo "ipython notebook" | bash
