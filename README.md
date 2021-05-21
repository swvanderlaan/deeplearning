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

## Step 0: change to `bash`

Since macOS Catalina the standard `shell` of Terminal is `zsh`. I prefer `bash`. You can easily [change this](https://www.cyberciti.biz/faq/change-default-shell-to-bash-on-macos-catalina/).

List the available `shell` languages.

```
cat /etc/shells
```

Change the `shell` language to `bash`.

```
chsh -s /bin/bash
```

Make sure you close your Terminal to have the command take effect.


## Step 1: make some directories

Before you start, open a Terminal (or iTerm2) on your macOS. If you have it open make sure to go to the home directory. The following command will take you there (`cd` means _change directory_). 

```
cd $HOME
```


Make `.ssh` directory this is necessary to store some keys needed with `ssh` for the HPC, and Git to work.

```
mkdir .ssh
```

Also make sure you have the rights set correctly.

```
chmod -vR 0700 ~/.ssh
```

Copy the necessary files to this directory.

Make a `bin` directory, we will use this to make some softlinks to programs installed.

```
mkdir bin
```

Also make sure you have the rights set correctly.

```
chmod -vR 0777 ~/bin
```


## Step 2: get brew
Make sure you have [`brew`](https://brew.sh) installed.

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Also install these packages through `brew`; you will need them somewhere down the road.

```
brew install coreutils gnu-sed rename git gd libxml2 gdal libgit2 openblas curl geos findutils zlib boost gsl bison
```

Some packages for _deep learning_ with `python` and `r` on macOS.

```
brew install cmake pkg-config wget
brew install jpeg libpng libtiff openexr
brew install eigen tbb hdf5
```

Some package we use for genetics, and other 'big data' science.

```
brew install llvm samtools bcftools vcftools 
```

Some packages to use with [`CellProfiler`](https://cellprofiler.org) and [`slideToolKit`](https://github.com/swvanderlaan/slideToolKit).

```
brew install libharu imagemagick lzo opencv libsvg
```

## Step 3: get anaconda
Now install `anaconda` and make sure you install the `python 3.7` version, as anything higher causes issues with `TensorFlow` among others.

Make sure to download the right package from the offical `anaconda` [website](https://repo.anaconda.com/archive/Anaconda3-2019.03-MacOSX-x86_64.sh), this should be `Anaconda3-2019.03-MacOSX-x86_64` for `python 3.7`.

Next execute the following code.

```
bash ~/Downloads/Anaconda3-2019.03-MacOSX-x86_64.sh
```

### Keep anaconda from constricting your `brew` installs

Now that you have `anaconda` in combination with `brew` you will get the below message if you run `brew doctor`. 

```
brew doctor
Please note that these warnings are just used to help the Homebrew maintainers
with debugging if you file an issue. If everything you use Homebrew for is
working fine: please don't worry or file an issue; just ignore this. Thanks!

Warning: "config" scripts exist outside your system or Homebrew directories.
`./configure` scripts often look for *-config scripts to determine if
software packages are installed, and which additional flags to use when
compiling and linking.

Having additional scripts in your path can confuse software installed via
Homebrew if the config script overrides a system or Homebrew-provided
script of the same name. We found the following "config" scripts:
  /Users/username/anaconda3/bin/icu-config
  /Users/username/anaconda3/bin/krb5-config
  /Users/username/anaconda3/bin/freetype-config
  /Users/username/anaconda3/bin/xslt-config
  /Users/username/anaconda3/bin/libpng16-config
  /Users/username/anaconda3/bin/python3.7-config
  /Users/username/anaconda3/bin/libpng-config
  /Users/username/anaconda3/bin/xml2-config
  /Users/username/anaconda3/bin/python3.7m-config
  /Users/username/anaconda3/bin/python3-config
  /Users/username/anaconda3/bin/curl-config
  /Users/username/anaconda3/bin/ncursesw6-config
  /Users/username/anaconda3/bin/pcre-config
```

This could be a problem, or just a nuissance. You can [change this](https://hashrocket.com/blog/posts/keep-anaconda-from-constricting-your-homebrew-installs). 

You can add the lines below to your `.bashrc` file.

```
open -a BBEdit ~/.bashrc
```

(Or use another text-editor).

```
export SANS_ANACONDA="/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"

# added by Anaconda3 4.4.0 installer
export PATH="/Users/username/anaconda3/bin:$SANS_ANACONDA"

alias perseus="export PATH="\$SANS_ANACONDA" && echo Medusa decapitated."
alias medusa="export PATH="/Users/username/anaconda3/bin:\$SANS_ANACONDA" && echo Perseus defeated."

brew () {
  perseus
  command brew "$@"
  medusa
}
```

## Step 4: install a virtual environment package
Next install the `virtual environment` package. 

```
pip3 install virtualenv virtualenvwrapper
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


## Step 5: create a virtual environment
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
pip install -r /path/to/git-deeplearning/requirements.txt
```


## Step 6: test the environment
And now you should not have any problem running the following script.

```
python DLTest.py
```

To `deactivate` the `deeplearning` virtual environment simply type `deactivate deeplearning`.

## Step 7: get an image viewer up and running

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

_Inspired by_

https://www.pyimagesearch.com/2019/01/30/macos-mojave-install-tensorflow-and-keras-for-deep-learning/

https://medium.com/swlh/how-to-setup-your-python-projects-1eb5108086b1

https://towardsdatascience.com/how-to-successfully-install-anaconda-on-a-mac-and-actually-get-it-to-work-53ce18025f97


--------------

#### The MIT License (MIT)
##### Copyright (c) 1979-2021 Sander W. van der Laan | s.w.vanderlaan [at] gmail [dot] com.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:   

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Reference: http://opensource.org.
