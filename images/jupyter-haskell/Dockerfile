FROM haskell:7.10

MAINTAINER Adrian Liaw (Wei-Han Liaw) <adrianliaw2000@gmail.com>

RUN apt-get update

RUN apt-get install -y git python-dev curl libzmq3-dev pkg-config

RUN curl https://bootstrap.pypa.io/get-pip.py | python

RUN pip install ipython[notebook]

RUN cabal update && cabal install happy cpphs classy-prelude

RUN git clone https://github.com/twittner/zeromq-haskell

RUN cd zeromq-haskell && cabal install && cd ..

RUN git clone http://github.com/gibiansky/IHaskell

RUN cd IHaskell && ./build.sh ihaskell

RUN ihaskell install

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

RUN mkdir /root/workspace /root/workspace/notebooks /root/workspace/data

ENV DIR /root/workspace
ENV PASSWORD jupyter

WORKDIR $DIR

EXPOSE 8888

CMD ["ipython", "notebook"]
