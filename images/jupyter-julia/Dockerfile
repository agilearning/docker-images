FROM julia

MAINTAINER Adrian Liaw (Wei-Han Liaw) <adrianliaw2000@gmail.com>

RUN apt-get update && \
    apt-get install -y python-dev git curl libzmq-dev gcc g++ && \
    curl https://bootstrap.pypa.io/get-pip.py | python && \
    pip install ipython[notebook] && \
    julia -e 'Pkg.add("IJulia")'

RUN (echo "require(['base/js/namespace'], function (IPython) {" && \
     echo "  IPython._target = '_self';" && \
     echo "});") \
    > /root/.ipython/profile_julia/static/custom/custom.js

RUN (echo "c = get_config()" && \
     echo "headers = {'Content-Security-Policy': 'frame-ancestors *'}" && \
     echo "c.NotebookApp.allow_origin = '*'" && \
     echo "c.NotebookApp.allow_credentials = True" && \
     echo "c.NotebookApp.tornado_settings = {'headers': headers}" && \
     echo "c.NotebookApp.ip = '0.0.0.0'" && \
     echo "c.NotebookApp.port = 8888" && \
     echo "c.NotebookApp.open_browser = False" && \
     echo "from IPython.lib import passwd" && \
     echo "import os" && \
     echo "c.NotebookApp.password = passwd(os.environ.get('PASSWORD', 'jupyter'))") \
    > /root/.ipython/profile_julia/ipython_notebook_config.py

RUN mkdir /root/workspace /root/workspace/notebooks /root/workspace/data

ENV DIR /root/workspace
ENV PASSWORD jupyter

WORKDIR $DIR

EXPOSE 8888

CMD echo "ipython notebook --profile julia" | bash
