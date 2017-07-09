# mlnotes

These notes apply only to my setup (YMMV):
* Ubuntu 16.04 x86_64
* 48GB RAM
* GeForce GTX 1060 6GB

## CUDA

* Install per instructions, via runfile: http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html

## CuDNN

* Download versions 5 **and** 6 from NVidia: https://developer.nvidia.com/rdp/cudnn-download
* Copy the header(s) from v6 to `/usr/local/cuda/include`
* Copy all the v6 libraries to `/usr/local/cuda/lib64`
* Copy just the `*.so.5*` files to `/usr/local/cuda/lib64`

## TensorFlow

* Install with virtualenv, according to documentation: https://www.tensorflow.org/install/install_linux
* If you see errors related to `mock`, go outside of virtualenv, and install mock globally: `sudo pip install --upgrade mock`

## Image retraining demo

* https://www.tensorflow.org/tutorials/image_retraining
* After training complete, when running `label_image`, use the following parameters:

```
bazel-bin/tensorflow/examples/label_image/label_image --graph=/tmp/output_graph.pb --labels=/tmp/output_labels.txt --output_layer=final_result --input_layer=Mul --image=$HOME/<path to image>.jpg
```

## TensorBox

* Install based on instructions: https://github.com/TensorBox/TensorBox
* Optionally, edit `download_data.sh` and comment out the download of `brainwash`, since this is
3GB of sample data. If training on your own images, you don't need it.

## Caffe

* When building for use with Python, refer to the Makefile.config and Makefile in the `caffe` directory in this repo.

## ideepcolor

* Install per instructions: https://github.com/junyanz/interactive-deep-colorization
* If the output image appears gray, try re-downloading the models. (?)

### .bashrc

```
export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

export PYTHONPATH=$HOME/git/caffe/python:$PYTHONPATH
```

