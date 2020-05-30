# MLOPS: ML integrating with Devops 
 Hello everyone, TODAY I am going to integrate Machine Learning with DevOps. So, I am going to train my model  and predict accuracy by using Devops.

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
## Creating a dockerfile :
In RHEL8 fisrt make a directory that will store all the data or the program for our machine learning model.

    mkdir Code
Now the jenkins will automatically copy the files in this folder.
Download a centos:7 image in docker using:

    docker pull centos:7
    docker run -it --name os centos:7
Now install miniconda in this centos:7 :

    yum -y update 
    yum -y install curl bzip2 
    curl -sSL https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o /tmp/miniconda.sh 
    bash /tmp/miniconda.sh -bfp /usr/local/ 
    rm -rf /tmp/miniconda.sh 
    conda install -y python=3 
    conda update conda 
    conda clean --all --yes 
    rpm -e --nodeps curl bzip2 
    yum clean all
Now install all the requirements for the Machine learning model.
After the requirements are fulfilled use commit command to make your own image and we will use this image in our Dockerfile.

    docker commit os myimage:v2
Now type:

    vim Dockerfile
And write the following code:

    FROM: myimage:v2
    RUN mkdir /root/my_model
    VOLUME /root/my_model
    COPY ./Code/. ./root/my_model/
    WORKDIR /root/my_model
    CMD ["python3","code_file2.py"]
Save this docker file and now build two different images for different environments:

    docker build -t deep:v1 /root
    docker build -t neural_net:v1 /root
## Job 1 :
Pushing code to GitHub repository.

* Create a new job in Jenkins.
* Enter a Job Name, select “Freestyle project” and hit “OK” button.
* You will be redirected to the job configuration page where you can see the following settings :

  *General Settings: The section contents the general setting of the job like Discard old builds, support parameter, Disable the project, etc.

  *Source Code Management: The section contents the source code options such as GIT, SVN, etc.
 
  *Build Triggers: The section contents trigger settings that trigger the build based on the specific condition match.
 
  *Build: The section contents the build steps that can be performed by adding Batch or shell command.
 
  *Post-build Actions: The section contents the build steps that can be performed after the build action done.


Let’s add a build step that prints date by adding the “Executing shell” step.
Click on the “Add build step” drop-down and select “Execute shell”

## job2 :
Automatic launch and training the model.
Go to build triggers and click on 'Build after other projects are built' and give the name of your previous job.

Now to go to Build Execute shell and type the following command.
You can also use if sudo grep "sigmoid" /root/code/code_file2.py or if sudo grep "softmax" /root/code/code_file2.py

This will first check the model and will start the container accordingly.

## job3 :

Train our model and predict accuracy or matrics.
## job4 : 
Tweaking the model :
We already have a file "code_file2.py" that contains the code of the model.
But the tweak files contain the function used in code_file2.py which can be added for more layers.
For checking the accuracy and running the model for improved accuracy we use the following code.


    actual_accuracy=$(sudo cat /root/code/acc.txt)</br>
    expected=95</br>
    compare=$(echo "$actual_accuracy > $expected" | bc )</br>


    while [[ $compare != 1 ]]</br>
    do</br>
    if sudo grep "deep" /root/code/code_file2.py</br>
    then</br>
       test -t 1 && USE_TTY="-t"</br>
       sudo docker rm -f os1</br>
       filenames = ['code_file2.py', 'code_file0.py']</br>
       with open('output_file.py', 'w') as outfile:</br>
            for fname in filenames:</br>
                    with open(fname) as infile:</br>
                            for line in infile:</br>
                                    outfile.write(line)</br>
        sudo docker cp output_file.py ./root/code/</br>
        sudo docker run -i -v /root/code:/root/my_model --name os1 deep:v1</br>
    elif sudo grep "neural_net" /root/code/code_file2.py</br>
    then</br>
        test -t 1 && USE_TTY="-t"</br>
        sudo docker rm -f os1</br>
        filenames = ['code_file2.py', 'code_file0.py']</br>
        with open('output_file.py', 'w') as outfile:</br>
            for fname in filenames:</br>
                    with open(fname) as infile:</br>
                            for line in infile:</br>
                                    outfile.write(line)</br>
        sudo docker cp output_file.py ./root/code/</br>
        sudo docker run -i -v /root/code:/root/my_model --name os1 deep:v1 </br>

    compare=$(echo "$actual_accuracy > $expected" | bc )</br>
    done</br></br>
For the tweaking you can refer to another code in the repository.
This code will check the model for the accuracy.
If the accuracy is less than required, then this program will append the two program files and will create a new output file.
Here I have given two tweaking files but you can add more provided you have to add the conditions fir more files in this program.

## job5 :
Retrain the model or notify that the best model is created  :

Add another job in Jenkins and name it.
Go to build triggers and click on 'Build after other projects are built' and give the name of your previous job.

Now in the Execute shell type the following command.

## job6 :
  Start the jobs using pipeline view :
  
First install delivery pipeline and build pipeline plugin on jenkins.
Click on '+' in Jenkins Dashboard and select pipeline and give a name to the pipeline.
Now select the Job that you want to start the pipeline from, here i have given it my first job.
This is the final look after all the jobs will be build.
