## What is a loss function? ##

Loss functions are one of the most important components when training deep learning algorithms, by providing feedback to the network on how accurately the model is predicting. The "loss function" itself is a mathematical function that typically takes all the values output by the network for one batch of training along with the ground truth and maps this to a single value representing the loss (or inaccuracy of prediction). A higher value from the loss function normally implies that the prediction of the algorithm's outputs are deviating significantly from the ground truth. Cross entropy loss and its variants are the most commonly used loss functions. 

In common deep learning networks like CNN, RNN or LSTM, we would expect the loss value to decrease as training progresses, implying that the model is learning correctly.

## How are loss functions different in GANs? ##

The loss functions used in GANs are no different from loss functions in regular networks discussed above. However, the most important property of a GAN is that it has 2 networks that needs to be trained separately (or alternatively) - a generator and critic (discriminator). As a result, each of these networks would have their own loss functions during their respective training. 

*__Critic loss function:__* Training the critic is very similar to training any standard algorithm such as a cat-dog detector, except the cat and dog classes get replaced by "real" and "fake" classes. The critic loss function evaluates how accurately real data is classified as "real" and fake data from generator as "fake" for one batch of critic training. The critic loss function would emit a large value if the "fake" data from generator is often categorized as "real" by the critic, thereby implying that the critic is weak and is easily fooled by the generator. 

*__Generator loss function:__* During the generator training, the output from the generator for one batch is fed to the critic to see if the generated data is realistic enough to fool it. A generator that is learning well would typically fool the critic into believing that the generated data is real, thereby leading to a smaller loss function score.

## What are the challenges with GAN loss functions? ##

Many challenges are seen with GAN loss functions, especially when using standard loss functions such as cross entropy:

*__(1) Oscillating losses:__* As mentioned in the previous section that we would expect the loss function value to decrease as training progresses in standard cat-dog detector type problems. However, this is not true for GANs. Since we train generator and critic alternatively, they get "stronger" and "weaker" (with respect to each other) alternatively, thereby leading to oscillating loss values. This makes it really hard to understand if training has progressed well overall and if the data generated by the generator is good enough.

*__(2) Mode collapse:__* Often, the generator learns to fool the discriminator by latching onto 1 specific output for which the discriminator always get fooled. Once this happens, the generator has no incentive to learn and create new variety of data, thereby leading to a sub optimal generator. 

## What is a Wasserstein loss function and WGAN? ##

The problems discussed above are overcome with a new type of loss function called Wasserstein loss and a GAN using this is called WGAN. Without going into the math details, it may be sufficient to know that Wasserstein loss primarily addresses the issues discussed above by:

(1) Defining the loss function such that there is correlation between the loss values and the model learning.

(2) Ensuring GANs become stable over time. 

## How do the loss functions work with DeepComposer? ##

Everything we spoke about GAN loss functions also applies to DeepComposer, so we use Wasserstein loss function in our architecture as well. But it is important to not over index on Wasserstein loss function as it is just another special type of loss function that works well for GANs. Keeping that in mind, here is how loss functions are related to the DeepComposer architecture:

*__(1) Critic loss function:__* During critic training, we provide 2 types of data: the real data (Multi-track songs with melody + accompaniments from training dataset) and the fake data (songs generated by the generator, in which the generator added the instrument accompaniments). The loss function outputs a value that indicates how well the critic can identify real songs as "real" and fake songs as "fake". A high value indicates that the critic is weak and easily is fooled by the generator, so this value is used to provide feedback to improve the critic training. 

*__(2) Generator loss function:__* During the generator training, the generator takes in a single melody track as input and outputs a multi-track song. This output is now fed to the critic (whose weights are frozen) to see if how close the critic thinks the generated song follows the distribution of the real songs. The loss function outputting a high value implies that the critic was not fooled correctly, thereby penalizing the generator to learn better.
