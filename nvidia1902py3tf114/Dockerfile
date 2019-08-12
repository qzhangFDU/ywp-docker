FROM nvcr.io/nvidia/tensorrt:19.02-py3

MAINTAINER Paul Kwon <paulkwon@phantom.ai>

RUN pip install tensorflow-gpu

# Set up notebook config
#COPY jupyter_notebook_config.py /root/.jupyter/

# Jupyter has issues with being run directly: https://github.com/ipython/ipython/issues/7062
#COPY run_jupyter.sh /root/

# Expose Ports for TensorBoard (6006), Ipython (8888)
#EXPOSE 6006 8888

#WORKDIR "/root"
CMD ["/bin/bash"]
