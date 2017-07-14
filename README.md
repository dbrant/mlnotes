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

### Walkthrough

* https://codelabs.developers.google.com/codelabs/tensorflow-for-poets
* https://petewarden.com/2016/09/27/tensorflow-for-mobile-poets/

To retrain (on Inception V3 by default):
```
bazel build tensorflow/examples/image_retraining:retrain
bazel-bin/tensorflow/examples/image_retraining/retrain --image_dir [directory]
```
The output graph is written to `/tmp` by default, or use `--output_graph` and `--output_labels` to
specify custom output locations. It creates a .pb file, and a .txt file that contains the auto-generated labels.

To verify:
```
bazel-bin/tensorflow/examples/label_image/label_image \
--graph=/tmp/output_graph.pb --labels=/tmp/output_labels.txt \
--input_layer=Mul --output_layer=final_result \
--image=[image.jpg]
```
To strip unused Ops (i.e. the DecodeJpeg op) for use on mobile device:
```
bazel build tensorflow/python/tools:strip_unused
bazel-bin/tensorflow/python/tools/strip_unused --input_graph output_graph.pb --output_graph foo.pb --input_node_names "Mul" --output_node_names "final_result" --input_binary true
```

To retrain with a MobileNet model (faster than Inception (with a slight decrease in accuracy),
and therefore better for mobile):
```
bazel-bin/tensorflow/examples/image_retraining/retrain --image_dir [directory] \
--architecture mobilenet_1.0_224
```
For a list of MobileNet variations, see https://research.googleblog.com/2017/06/mobilenets-open-source-models-for.html

#### Model parameters

For Inception V3:
```
input_layer = Mul
input_size = 299
input_mean = 128
input_std = 128
output_layer = final_result
```

For MobileNet:
```
input_layer = input
input_size = [last number of model architecture]
input_mean = 127.5
input_std = 127.5
output_layer = final_result
```

### Android samples

* https://github.com/tensorflow/tensorflow/tree/master/tensorflow/examples/android
* https://github.com/tensorflow/tensorflow/blob/master/tensorflow/contrib/android/README.md

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
