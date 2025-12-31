# PCB-Classification details
This projects shows the code required to classify the PCB faults into 6 categories from a pretrained ResNet-34 model in pytorch. A pretrained model is used as the dataset
contains only 600 images, thus training a model from scratch isn't possible. The dataset is imported from Kaggle. The dataset has annotations which give coordinates of 
the faults. I proceed to crop the images, and normalise them, then feed it into the model. The model proceeds to train for about 8
epoch cycles. I intially tried 10 but by 7th epoch it reached 99.7%.
The second model is a non pretrained one. It doesnt use ResNet34. The model has 4 convolution blocks and uses maxpooling, batch normalisation
and a non linear activation in each convolution layer. By the 4rth convolution layer, I put 512 parameters in the model, This allows it to extract more details
The training procedure is the same as the ResNet one. By 16 epochs, I was able to achieve a 98.75% accuracy. 1 or to more epochs can push this to 100%.
Thus I was able to make 2 models, 1 pretrained and 1 from scratch. The pretrained ResNet was able to get to a high accuracy by 7 epochs and takes less time to trin. 
The model I made, requires atleast 16 epoch cycles to get to a good accuracy, thus takes more time to achieve the same level of succes
