# Introduction

This chapter briefly introduces the outline of the whole book, targeted audience, how to use the book, then the installation of Owl.
There are different ways to interact with Owl, including `utop`, notebook, and the Owl-Jupyter. Feel free to choose one as you are exploring the Owl world with us.

## What Is Scientific Computing

Scientific Computing is a rapidly evolving multidisciplinary field which uses advanced computing capabilities to understand and solve complex problems.
The algorithms used in scientific computing can be generally divided into two types: numerical analysis, and computer algebra (or symbolic computation).
The former uses numerical approximation to solve mathematical problems, while the latter requires a exact close-form representation of computation and manipulates symbols that are not assigned specific values.

Both approaches are widely used in various applications fields, such as engineering, physics, biology, finance, etc.
Even though these advanced applications are sophisticated, they are all built atop of basic numerical operations in a scientific library, most of which Owl has already provided.
For example, you can write a deep neural network with Owl in a few lines of code:

```ocaml
open Owl
open Neural.S
open Neural.S.Graph
open Neural.S.Algodiff

let make_network input_shape =
  input input_shape
  |> lambda (fun x -> Maths.(x / F 256.))
  |> conv2d [|5;5;1;32|] [|1;1|] ~act_typ:Activation.Relu
  |> max_pool2d [|2;2|] [|2;2|]
  |> dropout 0.1
  |> fully_connected 1024 ~act_typ:Activation.Relu
  |> linear 10 ~act_typ:Activation.(Softmax 1)
  |> get_network
```

It actually consists of basic operations such as `add`, `div`, `convolution`, `dot`, etc.
It's totally OK if you have no idea what this piece of code is doing. We'll cover that later in this book.
The point is that how to dissect a complex application into basic building blocks in a numerical library, and that's what we are trying to convey throughout this book.

The book is divided into three parts, each contains multiple chapters.
The first part covers the numerical techniques such as the math function, optimisation methods, neural networks, etc. This part aims to introduce how to *use* Owl as a tool.
The second part introduces the system architecture of Owl. It introduces the design and implementation of Owl itself for those users who are interested in how things work. In this part we include things such as how we optimise the performance of operations and graphs in computation. 
The third part contains interesting use cases of Owl. They are not toy examples, but real applications used in particular fields. For example, we have computer vision applications such as image classification, segmentation, and neural style transfer, each is covered by a whole chapter.

There is a long way to go from simple math calculation to those large use cases. Now let's start from the very first step: installing Owl.


## What is Functional Programming

**[jianxin]** Two pages. Have to include at least: imperative (procedure, OO); different computational model, one follows Church, the other Turing (who is the student of Church); basic lambda calculus.

TBD: Function programming is ... but do we want to use a functional programming language to do scientific computing?

Explain the fundamental difference between imperative languages (e.g. C) and functional languages is the underlying model of computation. The imperative one is based Alan Turing model, whereas the functional one is based on Alonzo Church. Mention that Church was Turing's teacher. 

Spend some words on lambda calculus ...


## Who Is This Book For

We really hope this book can cover as broad an audience as possible. Both scientific computing and functional programming are big areas, therefore it is quite a challenge to write a book that satisfies everyone. If you are reading this book now, we assume you are already interested in analytical tasks and enthusiastic about gaining some hands-on experience with functional programming languages. We also assume you know how to program in OCaml and are familiar with the core concepts of functional programming.

We want this book to be relatively general so we have covered many topics in scientific computing. However, this means we cannot dive very deeply into each topic, and each of them per se is probably worth a book. When designing the chapters, we select those topics which are either classical (e.g. statistics, linear algebra) or popular and proven-to-be-effective in industry (e.g. deep neural network, probabilistic programming, and etc.). We strive to reach a good balance between breadth and depth. For each topic, we will try our best to list sufficient references to guide our readers to study further.

Unlike from other data science books, this book can serve as a reference for other software architects building modern numerical software systems. A big part of this book is dedicated to explaining the underlying details of Owl. Not only we will give you a bird's-eye-view of the overall Owl system, but also teach you how to build the system up and optimise each component step-by-step. If you use Owl to build applications, this book can serve as a useful reference manual as well.


## Structure of the Book

The book is divided into three parts, and each part focuses on different areas.

Part I first introduces the basics of the Owl system and important conventions to help you in studying how to program with Owl. It then explores various topics in scientific computing, from the classic mathematics, statistics, linear algebra, algorithmic differentiation, optimisation, regression to the popular deep neural network, natural language processing, probabilistic programming, and etc. The chapters are loosely organised based on their dependency, e.g. you need to know optimisation before studying regression and deep neural networks.

Part II is dedicated to presenting the architecture of the Owl system. We will dive into each core component and show how we build and optimise the software. By so doing, you will gain a thorough understanding on how a modern numerical system can be structured and developed, and what are the key components needed in such a complex system. Note that even though Owl is developed in OCaml, the knowledge you learnt in this part can be extrapolated to another language.

Part III is a collection of case studies. This part might be the most interesting one for data scientists and practitioners. We will demonstrate how you can build a complete numerical application quickly from scratch using Owl. The cases include computer vision, recommender systems, financial technology, and etc.

The book does not enforce any strict order in reading, you can simply jump to the topic that interests you most. If you are completely new to the Owl system, we still strongly recommend you to start with the first two chapters of the book so that you know how to set up a working environment and start programming. All the code snippets included in this book can be compiled with the most recent master branch of Owl, our tooling guarantees the book material stay up-to-date with the software.


## Installation

Owl requires OCaml version `>=4.10.0`. Please make sure you have a working OCaml environment before you start installing Owl. You can read the guide on how to [Install OCaml](https://ocaml.org/docs/install.html).

Owl's installation is rather trivial. There are four possible ways as shown below, from the most straightforward one to the least one.


### Option 1: Install from OPAM

Thanks to the folks in [OCaml Labs](http://ocamllabs.io/), OPAM makes package management in OCaml much easier than before. You can simply type the following the command lines to install.

```shell

  opam install owl

```

There is a known issue when installing Owl on `ubuntu`-based distribution. The reason is that the binary distribution of BLAS and LAPACK are outdated and failed to provide all the interfaces Owl requires. You will need to compile `openblas` manually, and use the appropriate environment variables to point at your newly compiled library. You can use `[owl's docker file](https://github.com/owlbarn/owl/blob/master/docker/Dockerfile.ubuntu) as a reference for this issue.

This way of installation pulls in the most recent Owl released on OPAM. Owl does not have a fixed release schedule. We usually make a new release whenever there are enough changes accumulated or a significant feature implemented. If you want to try the newest development features, we recommend the other ways to install Owl, as below.


### Option 2: Pull from Docker Hub

[Owl's docker images](https://hub.docker.com/r/owlbarn/owl/) are synchronised with the master branch. The image is always automatically built whenever there are new commits. You can check the building history on [Docker Hub](https://hub.docker.com/r/owlbarn/owl/builds).

You only need to pull the image then start a container.

```shell

  docker pull owlbarn/owl
  docker run -t -i owlbarn/owl

```

Besides the complete Owl system, the docker image also contains an enhanced OCaml toplevel - `utop`. You can start `utop` in the container and try out some examples. The source code of Owl is stored in `/root/owl` directory. You can modify the source code and rebuild the system directly in the started container.

There are Owl docker images on various Linux distributions, this can be further specified using tags, e.g. `docker pull owlbarn/owl:alpine`.


### Option 3: Pin the Dev-Repo

`opam pin` allows you to pin the local code to Owl's development repository on Github. The first command `opam depext` installs all the dependencies Owl needs.

```shell

  opam depext owl
  opam pin add owl --dev-repo

```


### Option 4: Compile from Source

This is an old-schooled but my favourite option. First, you need to clone the repository.

```shell

  git clone git@github.com:owlbarn/owl.git

```

Second, you need to figure out the missing dependencies and install them.

```shell

  dune external-lib-deps --missing @install @runtest

```

Last, this is perhaps the most classic step.

```shell

  make && make install

```

If your OPAM is older than `V2 beta4`, you need one extra step. This is due to a bug in OPAM which copies the compiled library into `/.opam/4.06.0/lib/stubslibs` rather than `/.opam/4.06.0/lib/stublibs`. If you don't want to upgrade OPAM, then you need to manually move `dllowl_stubs.so` file from `stubslib` to `stublib` folder, then everything should work. However, if you have the most recent OPAM installed, this will not be your concern.


### CBLAS/LAPACKE Dependency

The most important dependency is [OpenBLAS](https://github.com/xianyi/OpenBLAS). Linking to the correct OpenBLAS is the key to achieve the best performance. Depending on the specific platform, you can use `yum`, `apt-get`, `brew` to install the binary format. For example on my Mac OSX, the installation looks like this:

```shell

  brew install homebrew/science/openblas

```

However, installing from OpenBLAS source code give us extra benefits. First, it implements the most recent interfaces comparing to the outdated binary distribution offered by the native package management tool. Second, it leads to way better performance because OpenBLAS tunes many parameters based on your system configuration and architecture to generate the most optimised binary code.

OpenBLAS already contains an implementation of LAPACKE, as long as you have a Fortran complier installed on your computer, the LAPACKE will be compiled and included in the installation automatically.


## Interacting with Owl

There are several ways to interact with Owl system. The classic one is to write an OCaml application, compile the code, link to Owl system, then run it natively on a computer. You can also skip the compilation and linking step, and use Zoo system to run the code as a script.

However, the easiest way for a beginner to try out Owl is using REPL (Read–Eval–Print Loop), namely an interactive toplevel. The toplevel offers a convenient way to play with small code snippets. The code run in the toplevel is compiled into bytecode rather than native code. Bytecode often runs much slower than native code. However, this has very little impact on Owl's performance because all its performance-critical functions are implemented in C language.

OCaml code can be compiled in either bytecode or native code. The bytecode is executed on OCaml virtual machine which is less performant then platform-optimised native code. Toplevel runs the user code in bytecode mode, but this has little impact on Owl's performance because its core functions are implemented in C language. It is hard to notice any performance degradation if you run Owl in a script.

In the following, I will introduce two options to set up an interactive environment for Owl.


### Using Toplevel

OCaml language has bundled with a simple toplevel, but I recommend *utop* as a more advance replacement. Installing *utop* is straightforward using OPAM, simply run the following command in the system shell.


```shell

  opam install utop

```

After installation, you can load Owl in *utop* with the following commands. `owl-top` is Owl's toplevel library which will automatically load several related libraries (including `owl-zoo`, `owl-base`, and `owl` core library) to set up a complete numerical environment.

```ocaml
#  #require "owl-top"
#  open Owl
```

If you do not want to type these commands every time you start *toplevel*, you can add them to `.ocamlinit` file. The toplevel reads `.ocamlinit` file to initialise the environment during the startup. This file is often stored in the home directory on your computer.



### Using Notebook

Jupyter Notebook is a popular way to mix presentation with interactive code execution. It originates from Python world and is widely supported by various languages. One attractive feature of notebook is that it uses client/server architecture and runs in a browser.

If you want to know how to use a notebook and its technical details, please read [Jupyter Documentation](http://jupyter.org/documentation). Here let me show you how to set up a notebook to run Owl step by step.

Run the following commands in the shell will install all the dependency for you. This includes Jupyter Notebook and its [OCaml language extension](https://github.com/akabe/ocaml-jupyter).


```shell

  pip install jupyter
  opam install jupyter
  jupyter kernelspec install --name ocaml-jupyter "$(opam config var share)/jupyter"

```

To start a Jupyter notebook, you can run this command. The command starts a local server running on [http://127.0.0.1:8888/](http://127.0.0.1:8888/), then opens a tab in your browser as the client.


```shell

  jupyter notebook

```

If you wish to run a notebook server remotely, please refer to ["Running a notebook server"](http://jupyter-notebook.readthedocs.io/en/stable/public_server.html) for more information. To set up a server for multiple users, which is especially useful for educational purpose, please consult to [JupyterHub](https://jupyterhub.readthedocs.io/en/latest/) system.

When everything is up and running, you can start a new notebook in the web interface. In the new notebook, you must run the following OCaml code in the first input field to load Owl environment.


```ocaml
# #use "topfind"
# #require "owl-top, jupyter.notebook"
```

At this point, a complete Owl environment is set up in the Jupyter Notebook, and you are free to go with any experiments you like.

For example, you can simply copy & paste the whole [lazy_mnist.ml](https://github.com/owlbarn/owl/blob/master/examples/lazy_mnist.ml) to train a convolutional neural network in the notebook. But here, let us just use the following code.


```ocaml env=intro_00
# #use "topfind"
# #require "owl-top, jupyter.notebook"

# open Owl
# open Neural.S
# open Neural.S.Graph
# open Neural.S.Algodiff

# let make_network input_shape =
    input input_shape
    |> lambda (fun x -> Maths.(x / F 256.))
    |> conv2d [|5;5;1;32|] [|1;1|] ~act_typ:Activation.Relu
    |> max_pool2d [|2;2|] [|2;2|]
    |> dropout 0.1
    |> fully_connected 1024 ~act_typ:Activation.Relu
    |> linear 10 ~act_typ:Activation.(Softmax 1)
    |> get_network
val make_network : int array -> network = <fun>
```

`make_network` defines the structure of a convolution neural network. By passing the shape of input data, Owl automatically infers the shape of whole network, and prints out the summary of network structure nicely on the screen.

```ocaml env=intro_00
# make_network [|28;28;1|]
- : network =
18839

[ Node input_0 ]:
    Input : in/out:[*,28,28,1]
    prev:[] next:[lambda_1]

[ Node lambda_1 ]:
    Lambda       : in:[*,28,28,1] out:[*,28,28,1]
    customised f : t -> t
    prev:[input_0] next:[conv2d_2]

[ Node conv2d_2 ]:
    Conv2D : tensor in:[*;28,28,1] out:[*,28,28,32]
    init   : tanh
    params : 832
    kernel : 5 x 5 x 1 x 32
    b      : 32
    stride : [1; 1]
    prev:[lambda_1] next:[activation_3]

[ Node activation_3 ]:
    Activation : relu in/out:[*,28,28,32]
    prev:[conv2d_2] next:[maxpool2d_4]

[ Node maxpool2d_4 ]:
    MaxPool2D : tensor in:[*,28,28,32] out:[*,14,14,32]
    padding   : SAME
    kernel    : [2; 2]
    stride    : [2; 2]
    prev:[activation_3] next:[dropout_5]

[ Node dropout_5 ]:
    Dropout : in:[*,14,14,32] out:[*,14,14,32]
    rate    : 0.1
    prev:[maxpool2d_4] next:[fullyconnected_6]

[ Node fullyconnected_6 ]:
    FullyConnected : tensor in:[*,14,14,32] matrix out:(*,1024)
    init           : standard
    params         : 6423552
    w              : 6272 x 1024
    b              : 1 x 1024
    prev:[dropout_5] next:[activation_7]

[ Node activation_7 ]:
    Activation : relu in/out:[*,1024]
    prev:[fullyconnected_6] next:[linear_8]

[ Node linear_8 ]:
    Linear : matrix in:(*,1024) out:(*,10)
    init   : standard
    params : 10250
    w      : 1024 x 10
    b      : 1 x 10
    prev:[activation_7] next:[activation_9]

[ Node activation_9 ]:
    Activation : softmax 1 in/out:[*,10]
    prev:[linear_8] next:[]


```

The Second example demonstrates how to plot figures in notebook. Because Owl's Plot module does not support in-memory plotting, the figure needs to be written into a file first before passing to `Jupyter_notebook.display_file` to render.


```ocaml env=intro_01

# #use "topfind"
# #require "owl-top, owl-plplot, jupyter.notebook"
# open Owl
# open Owl_plplot

# let f x = Maths.sin x /. x in
  let h = Plot.create "plot_00.png" in
  Plot.set_title h "Function: f(x) = sine x / x";
  Plot.set_xlabel h "x-axis";
  Plot.set_ylabel h "y-axis";
  Plot.set_font_size h 8.;
  Plot.set_pen_size h 3.;
  Plot.plot_fun ~h f 1. 15.;
  Plot.output h
- : unit = ()
```

To load the image into browser, we need to call `Jupyter_notebook.display_file` function. Then we can see the plot [@fig:introduction:example00] is correctly rendered in the notebook running in your browser. Plotting capability greatly enriches the content of an interactive presentation.

```text
Jupyter_notebook.display_file ~base64:true "image/png" "plot_00.png"
```

![Plot example using Owl Notebook](images/introduction/plot_00.png "plot_00"){ width=90% #fig:introduction:example00}

Even though the extra call to `display_file` is not ideal, it is obvious that the tooling in OCaml ecosystem has been moving forward quickly. I believe we will soon have even better and more convenient tools for interactive data analytical applications.


### Using Owl-Jupyter

For the time being, if you want to save that extra line to display a image in Jupyter. There is a convenient module called `owl-jupyter`. Owl-jupyter module overloads the original `Plot.output` function so that a plotted figure can be directly shown on the page.

```txt

# #use "topfind"
# #require "owl-jupyter"
# open Owl_jupyter

# let f x = Maths.sin x /. x in
  let h = Plot.create "plot_01.png" in
  Plot.set_title h "Function: f(x) = sine x / x";
  Plot.set_xlabel h "x-axis";
  Plot.set_ylabel h "y-axis";
  Plot.set_font_size h 8.;
  Plot.set_pen_size h 3.;
  Plot.plot_fun ~h f 1. 15.;
  Plot.output h
- : unit = ()

```

![Plot example using Owl-Jupyter](images/introduction/plot_01.png "plot_01"){ width=90% #fig:introduction:plot01 }

From the example above, you can see Owl users' experience can be significantly improved.


## Start The Adventure

At this point you have installed a working environment of Owl on your computer, you should feel really proud of yourself. Often, this can be the most challengening part for a new user, even though Owl team has spent tons of time in improving its compilation and intallation. Anyway, it is time to move foward, so roll up your sleeves and let's march into the wonderful scientific computing world
