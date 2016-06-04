## Synthesizing preferred inputs via deep generator networks

This repository contains source code necessary to reproduce some of the main results in the paper:

[Nguyen A](http://anhnguyen.me), [Dosovitskiy A](http://lmb.informatik.uni-freiburg.de/people/dosovits/), [Yosinski J](http://yosinski.com/), [Brox T](http://lmb.informatik.uni-freiburg.de/people/brox/index.en.html), [Clune J](http://jeffclune.com). (2016). ["Synthesizing the preferred inputs for neurons in neural networks via deep generator networks."](http://arxiv.org/abs/1605.09304). arXiv:1605.09304.

**If you use this software in an academic article, please cite:**

    @article{nguyen2016synthesizing,
      title={Synthesizing the preferred inputs for neurons in neural networks via deep generator networks},
      author={Nguyen, Anh and Dosovitskiy, Alexey and Yosinski, Jason and Brox, Thomas and Clune, Jeff},
      journal={arXiv preprint arXiv:1605.09304},
      year={2016}
    }

For more information regarding the paper, please visit www.evolvingai.org/synthesizing

## Setup
This code is built on top of Caffe. You'll need to install the following:
* Install Caffe; follow the official [installation instructions](http://caffe.berkeleyvision.org/installation.html).
* Build the Python bindings for Caffe
* If you have an NVIDIA GPU, you can optionally build Caffe with the GPU option to make it run faster
* Install [ImageMagick](http://www.imagemagick.org/script/binary-releases.php) CLI on your system

## Usage
The main Python file is [act_max.py](act_max.py), which is a standalone Python script; you can pass various command-line arguments to run different experiments. Basically, to synthesize a preferred input for a target neuron *h* (e.g. the “candle” class output neuron), we optimize the hidden code input (red) of a [deep image generator network](https://arxiv.org/abs/1602.02644) to produce an image that highly activates *h*.

<p align="center">
    <img src="http://www.cs.uwyo.edu/~anguyen8/share/160531__arxiv_main_concept.jpg" width=600px>
</p>

We provide here four different examples:

[1_activate_output.sh](1_activate_output.sh): Optimizing a code to activate an *output* neuron of the [CaffeNet DNN](https://github.com/BVLC/caffe/tree/master/models/bvlc_reference_caffenet) trained on ImageNet dataset. This script synthesizes images for 5 example neurons and produces this result:

<p align="center">
    <img src="examples/example1.jpg" width=600px>
</p>

[2_start_from_real_image.sh](3_start_from_real_image.sh): Instead of starting from a random code, this example starts from a code of a real image (here, an image of a red bell pepper) and optimizes it to increase the activation of the "bell pepper" neuron. 
* Depending on the hyperparameter settings, one could produce images near or far the initialization code (e.g. ending up with a *green* pepper when starting with a red pepper).
* The `debug` option is enabled, so one can see the activations of intermediate images. The script produces:

<p align="center">
    <img src="examples/example2.jpg" width=700px>
</p>
<p align="center"><i>Optimization adds more green leaves and a surface below the initial pepper</i></p>


[3_activate_output_placesCNN.sh](4_activate_output_placesCNN.sh): Optimizing a code to activate an *output* neuron of a different network, here [AlexNet DNN](http://places.csail.mit.edu/) trained on [MIT Places205](http://places.csail.mit.edu/) dataset. The prior used here produces the best images for visualizing AlexNet models, it also works on other models but the image quality degrades (see Sec. 3.3 in our paper).

<p align="center">
    <img src="examples/example3.jpg" width=600px>
</p>

[4_activate_hidden.sh](4_activate_hidden.sh): Optimizing a code to activate a *hidden* neuron of the [DeepScene DNN](https://github.com/BVLC/caffe/tree/master/models/bvlc_reference_caffenet) trained on MIT Places dataset. This script synthesizes images for 5 example neurons and produces this result:

<p align="center">
    <img src="examples/example4.jpg" width=500px>
</p>
<p align="center"><i>Visualizations of hidden neurons at layer 5 of DeepScene DNN. From left to right are units that are semantically labeled by humans as: lighthouse, building, bookcase, food, and painting </i></p>

* This result matches the conclusion that object detectors automatically emerge in a DNN trained to classify images of places [2]. See Fig. 6 in [our paper](http://arxiv.org/abs/1605.09304) for more comparison between these images and visualizations produced by [2].

## Licenses
Note that the code in this repository is licensed under MIT License, but, the pre-trained models used by the code have their own licenses. Please carefully check them before use.
* The [image generator networks](https://arxiv.org/abs/1602.02644) are for non-commercial use only. See their [page](http://lmb.informatik.uni-freiburg.de/resources/software.php) for more.
* See the licenses of the models that you visualize (e.g. [DeepScene CNN](https://people.csail.mit.edu/khosla/papers/iclr2015_zhou.pdf)) before use.

## References

[1] Yosinski J, Clune J, Nguyen A, Fuchs T, Lipson H. "Understanding Neural Networks Through Deep Visualization". ICML 2015 Deep Learning workshop.

[2] Zhou B, Khosla A, Lapedriza A, Oliva A, Torralba A. "Object detectors emerge in deep scene cnns". ICLR 2015.
