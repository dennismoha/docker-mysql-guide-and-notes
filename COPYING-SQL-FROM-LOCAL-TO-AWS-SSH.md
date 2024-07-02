# HOW TO COPY SQL FROM LOCAL PC TO AWS SSH


1) for ubuntu

    -  scp -i path/to/your/.pemkey  filename ubuntu@eEc2PublicIp:/home/ubuntu/

    example:

    ` scp -i ubuntufile.pem  file.sql ubuntu@19.182.171.171:/home/ubuntu/`
2) if on `Amazon Linux AMi` replace `ubuntu` with `ec2-user`


- login to ssh using the following commands  ssh -i  sshkey ec2-user@publicip


## connecting to  EC2 Via ssh
    command:

    ubuntu:  ssh -i ath/to/your/.pemkey ubuntu@19.182.171.171
    Amazon AMi Linux: ssh -i ath/to/your/.pemkey ec2-user@19.182.171.171

- <b><i>
      if you are using Amazon Linux Ami then the user is `ec2-user` else if you are using the ubuntu AMI then user is `ubuntu`. More is in this [link](https://stackoverflow.com/a/18552866)</i></b>


- if you get the error `Permission denied (publickey) when SSH Access to Amazon EC2 instance [closed]` then use this [solution](https://stackoverflow.com/questions/18551556/permission-denied-publickey-when-ssh-access-to-amazon-ec2-instance)

# how to copy file to mysql

- Now once in the ssh

- login to mysql using `mysql -u root -p`
        
    - `show databases` to show a list of the databases;
    - `create database databasename` to create a database;
    - `use database databasename` to use that database;
    - `source /path/to/your/data.sql` - this processes the sql data in your local disk to mysql. 
    - `show tables` - to show a list of all tables; 