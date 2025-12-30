# PCB-Classification details
This projects shows the code required to classify the PCB faults into 6 categories from a pretrained ResNet-34 model in pytorch. A pretrained model is used as the dataset
contains only 600 images, thus training a model from scratch isn't possible. The dataset is imported from Kaggle. The dataset has annotations which give coordinates of 
the faults. I proceed to crop the images, and normalise them, then feed it into the model. The model proceeds to train for about 8
epoch cycles. I intially tried 10 but by 7th epoch it reached 100%. Thus 8 epochs is enough to get 100% accuracy in the model.
