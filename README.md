# MNIST characters

This is a copy of the MNIST character data and a class called
`sm::mnist` to load and use the data in a C++ program. `sm::mnist` uses code from [Seb's maths library](https://github.com/sebsjames/maths).

It also provides a program called `grid_mnist` to display the
characters. This program uses
[mathplot](https://github.com/sebsjames/mathplot) for the display.

![An image of MNIST numeral ID 0 which is a seven](https://github.com/sebsjames/mnist/blob/main/grid_mnist_0.png?raw=true)

## Dependencies

If you are using Debian or Ubuntu, the following `apt` command should
install the required dependencies.

```bash
sudo apt install build-essential cmake git wget  \
                 nlohmann-java3-dev librapidxml-dev \
                 freeglut3-dev libglu1-mesa-dev libxmu-dev libxi-dev \
                 libglfw3-dev libfreetype-dev
```

## Building

To build and run the example:

```bash
# Clone this example
git clone git@github.com:sebsjames/mnist

# Clone, copy or symlink mathplot INSIDE your example:
cd mnist
git clone --recurse-submodules git@github.com:sebsjames/mathplot

# Build prog1 in a 'build' directory
mkdir build
cd build
cmake ..
make
./grid_mnist 0 # You should see MNIST training/test example numeral ID 0
```
