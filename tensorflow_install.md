설치 환경: Ubuntu 14.04.3 64bit 

참고: http://www.tensorflow.org/get_started/os_setup.md#installing_from_sources/

작성자: Younggun Cho(yg.b.cho@gmail.com) / YoungSik Shin(bluevow@gmail.com)

# Installation from source

## Install CUDA & CUDNN
1. Download and install Cuda Toolkit 7.0 (이미 Cuda를 설치했으면 이 step은 pass)
	In order to build or run TensorFlow with GPU support, both Cuda Toolkit 7.0 and CUDNN 6.5 V2 from NVIDIA need to be installed.
	Follow instruction of

	https://developer.nvidia.com/cuda-toolkit-70

	Install the toolkit into e.g. /usr/local/cuda
    
2. Download and install CUDNN Toolkit 6.5
	우선 Cuda와 달리 Cudnn은 nvidia-developer에 가입 후 승인을 받아야 다운로드가 가능하다.
	Uncompress and copy the cudnn files into the toolkit directory. 
    Assuming the toolkit is installed in /usr/local/cuda
> $ tar xvzf cudnn-6.5-linux-x64-v2.tgz

	> $ sudo cp cudnn-6.5-linux-x64-v2/cudnn.h /usr/local/cuda/include

	> $ sudo cp cudnn-6.5-linux-x64-v2/libcudnn* /usr/local/cuda/lib6


-----
## Bazel 설치
Tensorflow는 cmake가 아닌 Bazel로 컴파일하기 때문에 우선 Bazel을 설치해야 한다. 

1. Install JDK8
> $ sudo add-apt-repository ppa:webupd8team/java
> $ sudo apt-get update
> $ sudo apt-get install oracle-java8-installer

2. Install Bazel
> $ git clone https://github.com/bazelbuild/bazel.git
> $ cd bazel
> $ git checkout tags/0.1.0
> $ ./compile.sh

3. Install other dependencies
> $ sudo apt-get install python-numpy swig python-dev

-----
    
## Download and Install Tensorflow
새로운 Command창에서 아래의 명령들을 수행한다. (Java 8 및 Bazel을 설치하였기 때문에 설치 후 새로운 Command에서 수행해야함)
    
1. Set environment
새로운 Command창에서 아래의 명령들을 수행한다.
> $ export PATH="$PATH:/home/yg/devel/bazel/output"

	/home/yg/devel/bazel/output 대신 본인의 경로 입력
    
2. Tensorflow Download
> $ git clone --recurse-submodules https://github.com/tensorflow/tensorflow

3. Configure TensorFlow's canonical view of Cuda libraries (TensorFlow root directory에서)
> $ ./configure 
	
	>Setting up Cuda include
	Setting up Cuda lib64
	Setting up Cuda bin
	Setting up Cuda nvvm
	Configuration finished
    

4. (optional) GPU campatibility 3.0 이하버전에 대해서
	참고: https://gist.github.com/infojunkie/cb6d1a4e8bf674c6e38e
    하지만 위 링크의 방법은 tensorflow 코드가 변경되면서 수정이 필요. TensorflowRoot에 본인 git clone한 경로를 입력
    
	1) TensorflowRoot/tensorflow/core/common_runtime/gpu/gpu_device.cc 의 line 625
		CudaVersion("3.5"), CudaVersion("5.2")
        ->
        CudaVersion("3.0"), CudaVersion("3.5"), CudaVersion("5.2")
    2) TensorflowRoot/third_party/gpus/crosstool/clang/bin/crosstool_wrapper_driver_is_not_gcc 의 line 237
    	supported_cuda_compute_capabilities = [ "3.5", "5.2" ]
        ->
		supported_cuda_compute_capabilities = [ "3.0", "3.5", "5.2" ]

5. Build your target with GPU support.
Bazel을 Download한 Root directory에서 아래의 명령을 수행
> $ bazel build -c opt --config=cuda //tensorflow/cc:tutorials_example_trainer
> $ bazel-bin/tensorflow/cc/tutorials_example_trainer --use_gpu

6. Create pip package and install
> $ bazel build -c opt //tensorflow/tools/pip_package:build_pip_package
> $ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
> $ sudo pip install /tmp/tensorflow_pkg/tensorflow-0.5.0-cp27-none-linux_x86_64.whl

-----
## Test Tensorflow
### Train your first TensorFlow neural net model
> $ python tensorflow/models/image/mnist/convolutional.py
> Succesfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Succesfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Succesfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Succesfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
Initialized!
Epoch 0.00
Minibatch loss: 12.054, learning rate: 0.010000
Minibatch error: 90.6%
Validation error: 84.6%
Epoch 0.12
Minibatch loss: 3.285, learning rate: 0.010000
Minibatch error: 6.2%
Validation error: 7.0%
...
...

위와 같이 training을 시작하면 Tensorflow가 올바르게 설치된 것


