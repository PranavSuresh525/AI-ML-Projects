# PCB-Classification details
This projects shows the code required to classify the PCB faults into 6 categories from a pretrained ResNet-34 model in pytorch. A pretrained model is used as the dataset
contains only 600 images, thus training a model from scratch isn't possible. The dataset is imported from Kaggle. The dataset has annotations which give coordinates of 
the faults. I proceed to crop the images, and normalise them, then feed it into the model.  
