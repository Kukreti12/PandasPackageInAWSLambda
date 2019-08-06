# PandasPackageInAWSLambda
How to create pandas package for AWS Lambda

1.    Connect to your EC2 Linux instance using SSH. For more information, see Connecting to Your Linux Instance Using SSH.

2.    Perform a yum install update.

[ec2-user ~]$ sudo yum update
3.    Launch Python, and then use the version command to find what Python versions are installed.

[ec2-user ~]$ python --version
4.    Install Python 3, if it's not already installed.

[ec2-user ~]$ sudo yum install python3 -y
5.    Use the which command to confirm that the install was successful.

[ec2-user ~]$ which python3 
/usr/bin/python3
Install virtualenv and create the python3 environment

1.    Install virtualenv for the current user using pip3.

pip3 install --user virtualenv
2.    Create a directory to hold your virtualenv environments, and then use the cd command to make it your current directory. In the following example, the environments are stored in the venv directory, under the ec2-user directory.

[ec2-user ~]$ pwd
/home/ec2-user
[ec2-user ~]$ mkdir venv
[ec2-user ~]$ cd venv
[ec2-user ~]$ pwd
/home/ec2-user/venv

3.    Use the virtualenv command to create the python3 environment.
[ec2-user ~]$ virtualenv -p /usr/bin/python3 python3
Running virtualenv with interpreter /usr/bin/python3
Using base prefix '/usr'
New python executable in python3/bin/python3
Also creating executable in python3/bin/python
Installing setuptools, pip...done.


4.  Activate the environment by sourcing the activate file in the bin directory under your project directory.

[ec2-user ~]$ source /home/ec2-user/venv/python3/bin/activate
5.   Use the which command to verify that you are now referencing the new environment.

[ec2-user ~]$ which python

~/venv/python3/bin/python   # python3 is now the default

6. Go to the site packages and download the Pandas and dependencies
[ec2-user ~]$ cd venv/python3/lib/python3.7/site-packages

7.
[ec2-user ~]$ sudo yum install zip
[ec2-user ~]$ zip -r pandas_archive.zip pandas
[ec2-user ~]$ zip -r pandas_archive.zip numpy
[ec2-user ~]$ zip -r pandas_archive.zip pytz
[ec2-user ~]$ zip -r pandas_archive.zip six.py
[ec2-user ~]$ zip -r pandas_archive.zip dateutil
[ec2-user ~]$ chmod 777 pandas_archive.zip


8. Navigate to the sit-packages and you will see the zip file ready. I am using WINSCP to connect to EC2 and check the folders. 

9. Create Python file and save with the name of "lambda_function.py" and add to the zip file
[ec2-user ~]$ zip -r pandas_archive.zip lambda_function.py

10. Download the zip file and upload in AWS lambda. As the package is greater than the 10 mbs we are exporting it to S3

[ec2-user ~]$ aws s3 cp "path of Zip file" "s3://bucketname/foldername/filename.zip

11. Go to Lambda function and give the path of zip from s3 and test it out. What ever code you have written in lambda_function.py will execute successfully using pandas library
