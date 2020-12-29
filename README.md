# deeplearning

This repository is aimed at recreating a virtual environment for deeplearning and whole-slide image analyses.

Code to recreate the `deeplearning` virtual environment is below.

To install this git execute the following code in your designated `git` folder.

```
git clone git@github.com:swvanderlaan/deeplearning.git
cd deeplearning
```


--------------

# Setting up

## Step 1: get brew
Make sure you have `brew` installed.

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Also install these packages through `brew`.

```
brew install cmake pkg-config wget
brew install jpeg libpng libtiff openexr
brew install eigen tbb hdf5
```


## Step 2: get anaconda
Now install `anaconda` and make sure you install the `python 3.7` version, as anything higher causes issues with `TensorFlow` among others.

Make sure to download the right package from the offical `anaconda` [website](https://repo.anaconda.com/archive/Anaconda3-2019.03-MacOSX-x86_64.sh), this should be `Anaconda3-2019.03-MacOSX-x86_64` for `python 3.7`.

Next execute the following code.

```
bash ~/Downloads/Anaconda3-2019.03-MacOSX-x86_64.sh
```


## Step 3: install a virtual environment package
Next install the `virtual environment` package. 

```
pip install virtualenv virtualenvwrapper
```

Open up a `BBEdit` window to add in the following code to `.bashrc`.

```
open -a BBEdit $HOME/.bashrc
```

```
# virtualenv and virtualenvwrapper
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3
source /usr/local/bin/virtualenvwrapper.sh
```


## Step 4: create a virtual environment
Now we are ready to create a virtual environment.

```
mkvirtualenv deeplearning -p python3
```

We should also make sure to install all the required packages.

Make sure you `workon` the `deeplearning` environment.

```
workon deeplearning
```

Then install the required packages.

```
pip install -r /path/to/git-deeplearning/deeplearning.requirements.txt
```


## Step 5: test the environment
And now you should not have any problem running the following script.

```
python test.py
```

To `deactivate` the `deeplearning` virtual environment simply type `deactivate deeplearning`.

## Step 6: get an image viewer up and running

We can use the build-in deep zoomer from `openslide` to view some images locally.

First install the openslide `git` in the designated `git` folder.

```
git clone https://github.com/openslide/openslide-python.git
```

Now you are ready to view some images locallly.

The basic command is like below, where `-Q` stands for the quality (`100`%) in this case, and `WSI_DIRECTORY` is the directory containing slides to show.

```
python deepzoom_multiserver.py -Q 100 WSI_DIRECTORY
```

So for example in our case it would be like this.

```
python ~/git/swvanderlaan/openslide-python/examples/deepzoom/deepzoom_multiserver.py -Q 100 ~/PLINK/analyses/expressscan/Projects/slideToolKit_Development/images/
  
```


--------------

_Inspired by _

https://www.pyimagesearch.com/2019/01/30/macos-mojave-install-tensorflow-and-keras-for-deep-learning/

https://medium.com/swlh/how-to-setup-your-python-projects-1eb5108086b1

https://towardsdatascience.com/how-to-successfully-install-anaconda-on-a-mac-and-actually-get-it-to-work-53ce18025f97


--------------

#### The MIT License (MIT)
##### Copyright (c) 1979-2020 Sander W. van der Laan | s.w.vanderlaan [at] gmail [dot] com.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:   

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Reference: http://opensource.org.
