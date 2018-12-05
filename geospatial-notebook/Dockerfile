# The image to use as a base image
FROM quay.io/uninett/jupyter-spark:20180901-0c7a087

# Adjusted from:
# https://github.com/lucadelu/docker-stacks/blob/geospatial/geospatial-notebook/Dockerfile
# https://github.com/wenzeslaus/geospatial-modeling-course-jupyter/blob/master/Dockerfile

# See also:
# https://github.com/neteler/docker-grass-gis-alpine/blob/master/Dockerfile
# https://github.com/mundialis/docker-grass-gis/blob/master/Dockerfile
# https://github.com/wenzeslaus/Notebook-for-processing-point-clouds-in-GRASS-GIS/blob/master/Dockerfile

# Install system packages
USER root
# libav-tools for matplotlib anim
RUN apt-get update && \
    apt-get install -y --no-install-recommends ffmpeg \
    apt-utils \
    xterm && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install all geospatial packages for python 2
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    python-pip \
    python-setuptools \
    python-gdal \
    python-rasterio \
    python-shapely \
    python-fiona \
    python-geopandas \
    python-matplotlib \
    python-mapnik \
    python-pdal \
    python-pyproj \
    python-pysal \
    python-osmapi \
    python-overpass \
    python-overpy \
    python-mapscript \
    python-owslib \
    python-numpy \
    python-pil \
    python-ply \
    geany \
    libboost-system-dev \
    libboost-thread-dev \
    libboost-filesystem-dev \
    libboost-program-options-dev \
    libboost-iostreams-dev \
    p7zip-full \
    mc \
    python-sphinx \
    python-http-parser && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install all geospatial packages python 3
RUN apt-get update && \
    apt-get install -y --no-install-recommends python3-fiona \
    python3-gdal \
    python3-rasterio \
    python3-shapely \
    # python3-pdal \
    python3-osmapi \
    python3-overpass \
    python3-overpy && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN python2 -m pip install ipykernel
RUN python2 -m ipykernel install

COPY grass-compile.sh /usr/local/bin/

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    python-numpy \
    python3-numpy \
    virtualenv \
    libgeotiff-dev \
    subversion \
    cython \
    build-essential \
    libblas-dev \
    libbz2-dev \
    libcairo2-dev \
    libfftw3-dev \
    libfreetype6-dev \
    libgdal-dev \
    libgeos-dev \
    libglu1-mesa-dev \
    libgsl0-dev \
    libjpeg-dev \
    liblapack-dev \
    libncurses5-dev \
    libnetcdf-dev \
    libopenjp2-7 \
    libopenjp2-7-dev \
    libpdal-dev pdal \
    libpdal-plugin-python \
    libpng-dev \
    libpq-dev \
    libproj-dev \
    libreadline-dev \
    libsqlite3-dev \
    libtiff-dev \
    libxmu-dev \
    libzstd-dev \
    bison \
    flex \
    g++ \
    gettext \
    gdal-bin \
    libfftw3-bin \
    make \
    ncurses-bin \
    netcdf-bin \
    proj-bin \
    proj-data \
    python \
    python-dev \
    python-numpy \
    python-pil \
    python-ply \
    sqlite3 \
    subversion \
    unixodbc-dev \
    zlib1g-dev

RUN grass-compile.sh

ENV TERM xterm

# Add shortcuts to distinguish pip for python2 and python3 envs
RUN ln -s $CONDA_DIR/envs/python2/bin/pip $CONDA_DIR/bin/pip2 && \
    ln -s $CONDA_DIR/bin/pip $CONDA_DIR/bin/pip3

RUN pip2 install \
    psutil \
    cartopy \
    folium \
    iris \
    pysal \
    scikit-learn \
    grass-session \
    pymodis \
    sentinelsat \
    pandas \
    gbif
    
RUN pip3 install \
    psutil \
    cartopy \
    folium \
    # iris \
    pysal

# Install other packages
USER notebook
# Configure ipython kernel to use matplotlib inline backend by default
RUN mkdir -p $HOME/.ipython/profile_default/startup
COPY mplimporthook.py $HOME/.ipython/profile_default/startup/

