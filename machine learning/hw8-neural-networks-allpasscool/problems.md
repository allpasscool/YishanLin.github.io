# Coding (0 points...but free response answers without supporting code may get a 0)

There is only one autograded test for this assignment and that is the test_netid.py test. You will not have to write any other code to pass the tests. You will, however, still have coding to do for the experiments.

You must hand in whatever code you did for data loading, your visualizations, and experiments by pushing to github (as you did for all previous assignments). Your code should be in the code/ directory.

**NOTE: if we have any doubts about your experiments we reserve the right to check this code to see if your results could have been generated using this code. If we don't believe it, or if there is no code at all, then you may receive a 0 for any free-response answer that would have depended on running code.**

You should make a conda environment for this homework just like you did for previous homeworks. We have included a requirements.txt.

# Free-response questions (10 points)


To answer some of the free-response questions, you will have to write extra code (that is not covered by the test cases). You may include your experiments in new files in the `experiments` directory. See `experiments/example.py` for an example. You can run any experiments you create within this directory with `python -m experiments.<experiment_name>`. For example, `python -m experiments.example` runs the example experiment.


## PyTorch Introduction

For this assigment, you will be learning about a popular library for implementing neural networks called PyTorch. It is very popular to train neural networks using a GPU (because they speed up training by a large factor), but for this assignment **you do not need a GPU** as **all of the training will happen on your CPU**. There are other popular libraries for neural networks, such as TensorFlow, but for this assignment you will be using PyTorch.

Here is the official website for PyTorch: [https://pytorch.org/](https://pytorch.org/)

Here is the API documentation: [https://pytorch.org/docs/stable/index.html](https://pytorch.org/docs/stable/index.html)

Here is a cheat sheet of commonly used methods: [https://pytorch.org/tutorials/beginner/ptcheat.html](https://pytorch.org/tutorials/beginner/ptcheat.html)

Here is a comparison of PyTorch and Numpy methods: [https://github.com/wkentaro/pytorch-for-numpy-users](https://github.com/wkentaro/pytorch-for-numpy-users)

**IMPORTANT: PyTorch is not included in `requirements.txt`!** To install PyTorch, find the correct install command for your operating system and version of python [here](https://pytorch.org/get-started/locally/). For "PyTorch Build" select the `Stable (1.1)` build, select your operating system, for "Package" select `pip` , for "Language" select your python version (either python 3.5, 3.6, or 3.7), and finally for "CUDA" select `None`. **Make sure to run the command with your conda environment activated.**

_Note: To determine which python you are using, type `python` into your command line to get an interactive shell, you should see your python version in the first line._


## Understanding PyTorch (1 point)

Read the first four tutorials on [this page](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html) ("*What is PyTorch?*", "*Autograd: Automatic Differentiation*", "*Neural Networks*", and "*Training a Classifier*") and then answer the following questions.

1. (0.25 points) What is a tensor? How is it different than a matrix?

2. (0.5 points) What is automatic differentiation? How does it relate to gradient descent and back-propagation? Why is it important for PyTorch?

3. (0.25 points) Why does PyTorch have its own tensor class (`torch.Tensor`) when it is extremely similar to numpy's `np.ndarray`?

## Training on MNIST (2 points)

Let us train neural networks to classify handwritten digits from the MNIST dataset and analyze the accuracy and training time of these neural networks. 

**Build the model architecture:** Create a neural network with two fully connected (aka, dense) hidden layers of size 128, and 64, respectively. Your network should have a total of four layers: an input layer that takes in examples, two hidden layers, and an output layer that outputs a predicted class (10 possible classes, one for each digit class in MNIST). Your hidden layers should have a ReLU activation function.  Your last (output) layer should be a linear layer with one node per class (in this case 10), and the predicted label is the node that has the max value. *Hint: In PyTorch the fully connected layers are called `torch.nn.Linear()`.

**Use these training parameters:** When you train a model, train for 100 epochs with batch size of 10 and use cross entropy loss. In PyTorch's `CrossEntropyLoss` class, the [softmax operation is built in](https://pytorch.org/docs/stable/nn.html?highlight=crossentropyloss#torch.nn.CrossEntropyLoss). Use the SGD optimizer with a learning rate of 0.01. 

**Making training sets:** Create one training set of each of these sizes {500, 1000, 1500, 2000} from MNIST. Note that you should be selecting examples in such a way that you minimize bias, i.e., make sure all ten digits are equally represented in each of your training sets. To do this, you can use `load_mnist_data` function in `load_data.py` where you can adjust the number of examples per digit and the amount of training / testing data. 

*Hint: To read your MNIST dataset for training, you may want to use a PyTorch `DataLoader`, but this is not required. If you do want to use it with your numpy NMIST dataset, you should use a custom PyTorch `DataLoader` class. We included the class definition for you in the HW (`MyDataset` in `my_dataset.py`) You can see more details about using custom dataset in this [blog](https://stanford.edu/~shervine/blog/pytorch-how-to-generate-data-parallel) or [github repo](https://github.com/utkuozbulak/pytorch-custom-dataset-examples))

**Train one model per training set:** Train a new model for each MNIST training set you created and test it on the MNIST testing subset. Use the same architecture for every model. For each model you train, record the loss function value every epoch. Record the time required to train for 100 epochs. From python's built in `time` module, use `time.time()`.

4. (0.5 points) Given the data from your 4 trained models, create a graph that shows the amount of training time along the y-axis and number of training examples along the x-axis. 

5. (0.5 points) What happens to your training time as the number of training examples increases? Roughly how many hours would you expect it to take to train on the full MNIST training set using the same architecture on the same hardware you used to create the graph in question 4?

6. (0.5 points) Create a graph that shows classification accuracy on your testing set on the y-axis and number of training 
examples on the x-axis. 

7. (0.5 points) What happens to the accuracy as the number of training examples increases?

## Exploring DogSet (1 point)

DogSet is a subset from a popular machine learning dataset called ImageNet (more info [here](http://www.image-net.org/) and [here](https://en.wikipedia.org/wiki/ImageNet)) which is used for image classification. The DogSet dataset is available [here](https://drive.google.com/open?id=1sKqMO7FwT_DyxQJh1YaMKAI6-lrlMXs0). (Note: you need to be signed into your `@u.northwestern.edu` google account to view this link). As it name implies, the entire dataset is comprised of images of dogs and labels indicating what dog breed is in the image. The metadata, which correlates any particular image with its label and partition, is provided in a file called `dogs.csv`. We have provided a general data loader for you (in `data/dogs.py`), but you may need to adopt it to your needs when using PyTorch. **Note: You may need to use the dataset class we provided in MNIST questions if you want to use a PyTorch `DataLoader`**

**Validation sets:** Thus far, you have only used "train" and "test" sets. But it is common to use a third partition called a "validation" set. The validation set is used during training to determine how well a model generalizes to unseen data. The model does *not* train on examples in the validation set, but periodically predicts values in the validation set while training on the training set. Diminishing performance on the validation set is used as an early stopping criterion for the training stage. Only after training has stopped is the testing set used. Here's what this looks like in the context of neural networks: for each epoch a model trains on every example in the training partition, when the epoch is finished the model makes predictions for all of the examples in the validation set and a loss is computed. If the difference between the calculated loss for this iteration and the previous is below some _epsilon_ for _N_ number of epochs in a row, then training stops and we move onto the testing phase.  

8. (0.5 points) In Dogset, how many are in the `train` partition, the `valid` partition and the `test` partition? What is the color palette of the images (greyscale, black & white, RBG)? How many dog breeds are there? 

9. (0.5 points) Select one type of breed. Look through variants of images of this dog breed. Show 3 different images of the same breed that you think are particularly challenging for a classifier to get correct. Explain why you think these three images might be challenging for a classifier. 

## Training A Model on DogSet (2 points)

**Build the model architecture:** Create a neural network with two fully connected (aka, dense) hidden layers of size 128, and 64, respectively. Note that you should be flattening your _NxNxC_ image to 1D for your input layer (where _N_ is the height/width, and _C_ is the number of color channels). Your network should have a total of four layers: an input layer that takes in examples, two hidden layers, and an output layer that outputs a predicted class (one node for each dog class in DogSet). Your hidden layers should have a ReLU activation function. Your last (output) layer should be a linear layer with one node per class, and the predicted label is the node that has the max value.

**Use these training parameters:** Use a batch size of 32 and using cross entropy loss. Use the SGD optimizer with a learning rate of 0.001. 

**When to stop training:** Stop training after 100 epochs or when your validation loss doesn't decrease by more than 1e-4 for 3 epochs in a row., whichever happens first.

**Training, Testing, Validation sets:** You should use training examples from `train` partition of DogSet. Validation should come from the `valid` partition and testing examples should come from the `test` partition.

10. (0.5 points) How many connections (weights) does this network have?

11. (1.0 point) Train a model on DogSet. After every epoch, record three things: the loss of your model on the training set, the loss of your model on the validation set, and the accuracy of your model on the validation set. 

    * Report the number of epochs your model trained, before terminating.
  
    * Make a graph that has both training and validation loss on the y-axis and epoch on the x-axis.
  
    * Make a graph that has the validation accuracy on the y-axis and epoch on the x-axis. 

    * Report the accuracy of your model on the testing set.
  
12. (0.5 points) Describe the interaction between training loss, validation loss and validation accuracy. When do you think your network stopped learning something meaningful to the problem? Why do you think that? Back up your answer by referring to your graphs.
  

## Convolutional Layers (2 points)

Convolutional layers are layers that sweep over and subsample their input in order to represent complex structures in the input layers. For more information about how they work, [see this blog post](https://ujjwalkarn.me/2016/08/11/intuitive-explanation-convnets/). Don't forget to read the PyTorch documentation about Convolutional Layers (linked above).

13. (0.5 points) Convolutional layers produce outputs that are of different size than their input by representing more than one input pixel with each node. If a 2D convolutional layer has `3` channels, batch size `16`, input size `(32, 32)`, padding `(4, 8)`, dilation `(1, 1)`, kernel size `(8, 4)`, and stride `(2, 2)`, what is the output size of the layer?

14. (0.5 point) Combining convolutional layers with fully connected layers can provide a boon in scenarios involving learning from images. Using a similar architecture to the one used in question 10, replace each of your first two hidden layers with a convolutional layers, and add a fully connected layer to output predictions as before. When you call the PyTorch convolutional layer function, leave all of the arguments to their default settings except for kernel size and stride. Determine reasonable values of kernel size and stride for each layer and report what you chose. Tell us how many connections (weights) this network has.


15. (1 point) Train your convolutional model on DogSet. After every epoch, record three things: the loss of your model on the training set, the loss of your model on the validation set, and the accuracy of your model on the validation set. 

    * Report the number of epochs your model trained, before terminating.
  
    * Make a graph that has both training and validation loss on the y-axis and epoch on the x-axis.
  
    * Make a graph that has the validation accuracy on the y-axis and epoch on the x-axis. 

    * Report the accuracy of your model on the testing set.

## Thinking about deep models (2.0 points)

16. (0.5 points) For any binary function of binary inputs, is it possible to construct some deep network built using only perceptron activation functions that can calculate this function correctly? If so, how would you do it? If not, why not?

17. (0.5 points) Is it possible to learn any arbitrary binary function from data using a network build only using linear activation functions? If so, how would you do it? If not, why not? 

18. (1 point) An adversarial example is an example that is designed to cause your machine learning model to fail. Gradient descent ML methods (like deep networks) update their weights by descending the gradient on the loss function L(X,Y,W) with respect to W. Here, X is a training example, Y is the true label and W are the weights. Explain how you could create an adversarial example by using the gradient with respect to X instead of W.





