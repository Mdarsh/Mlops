# MLOPS:ML integrating with Devops 
## Hello everyone, TODAY I am going to integrate Machine Learning with DevOps .So, I am going to train my model  and predict accuracy by using Devops.

## Project Overview :

1. Create docker container image that has python, Keras or numpy installed using dockerfile.
2. When we launch this image , it should automatically starts train the model in the container.
3. Now we create a job chain of job1,job2,job3,job4 and job5 using build pipeline plugin in jenkins.

## Project Summary :

We are going to build a chain of jobs here in order to get desired accuracy for given dataset but before going ahead we have to create a Dockerfile which will create image with required configurations.

* Job 1 : Job 1 will pull the Github repo automatically when some developers push repo to Github.

* Job 2 : By looking at the code or program file, jenkins should automatically start the respective machine learning software installed interpreter install image container to deploy code and start training.

* Job 3 : Train our model and predict accuracy or metrics.

* Job 4 : If metrics accuracy is less than 80% , then tweak the machine learning model architecture.

* Job 5 : Retrain the model or notify that the best model is being created.

* Job 6 : job6 will for monitoring: if container where app is running. fails due to any reason then this job should automatically start the container again from where the last trained model left.
#
