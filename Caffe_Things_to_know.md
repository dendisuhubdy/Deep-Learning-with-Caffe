
# Caffe: Things to know to train your network

1. **Data Injestion and Preprocessing**
 * Data Injestion formats
    * Level DB or LMDB database.
    * Datat in memory (C++ or Python).
    * HDF5 formated data.
    * Image files.
 * **Preprocessing Tools**
     `~ Can be found at $CAFFE_ROOT/build/tools`
    * LevelDB or LMDB creation from raw images.
    * Training and validation set creation with shuffling algorithms.
    * Mean image generation.
 * **Data Transformation Tools**
    * Image Cropping, Scaling, Resizing and mirroring.
    * Mean Subtration.
    
2. **Model Defenition**
 * Defined in a prototxt format.
 * Prototxt is a human readable format developed by Google.
 * It autogenerates and checks Caffe code.
 * Used to define Caffe's network architecture and training parameters.
 * Can be written by hand without any coding.
 * Can be written as a python function also. It autogenerates the prototxt file for you.
 
    2.1 **Different fucntions and layers**
     * **Loss Functions**
         * Classification
             * Softmax
             * Hinge loss
         * Linear Regression
             * Euclidean Loss
         * Attributes/Multiclassification
             * Simoid cross entropy loss
     * **Layers**
         * Convolution
         * Pooling
         * Normalization
     * **Activation Functions**
         * ReLU
         * Sigmoid
         * Tanh
     
3. **Network Training - Solver Files**
 * Prototxt file to list the parameters of the neural net's training algorithm
 * Example Solver File:
     
     ```
    # The train/test net protocol buffer definition
    train_net: "mnist/lenet_auto_train.prototxt"
    test_net: "mnist/lenet_auto_test.prototxt"
    # test_iter specifies how many forward passes the test should carry out.
    # In the case of MNIST, we have test batch size 100 and 100 test iterations,
    # covering the full 10,000 testing images.
    test_iter: 100
    # Carry out testing every 500 training iterations.
    test_interval: 500
    # The base learning rate, momentum and the weight decay of the network.
    base_lr: 0.01
    momentum: 0.9
    weight_decay: 0.0005
    # The learning rate policy
    lr_policy: "inv"
    gamma: 0.0001
    power: 0.75
    # Display every 100 iterations
    display: 100
    # The maximum number of iterations
    max_iter: 10000
    # snapshot intermediate results
    snapshot: 5000
    snapshot_prefix: "mnist/lenet"
    ```
4. Optimization Algorithms
 * SGD + momentum
 * ADAGRAD
 * NAG

5. After training
 * Caffe training produces a binary file with extension *\.caffemodel*.
     * This is a machine readable file generally a few hundered mega bytes.
     * This model can be reused for further training and can be shared as well.
     * Caffemodel file can be used for implementation of the neural net.
 * Integrate the model into the data pipeline using Caffe command line or Matlab/Python.
 * Deploy model across hardware or OS environments with caffe installed.
 
 6. Share to ModelZoo
  * ModelZoo is a project/model sharing community.
  * Normally, when sharing the caffemodel, the following should be present:
      * Solver
      * Model Prototxt Files
      * readme.md file which describes the:
          * Caffe version.
          * URL and SHA1 of *\.caffemodel* file.
      * License
      * Description of the Training Data.
 
### Caffe: Extensible Code
Caffe's inbuilt data types, layer types or loss functions may not be directly relevant in our neural network architecture. In those cases, we can write our own datatype or layertype or loss function by writing specific a Python or C++ classes for the same. This way we can extend caffe's capabilities to meet our needs. The new layer or loss function class can now be properly used in the required prototxt file.
