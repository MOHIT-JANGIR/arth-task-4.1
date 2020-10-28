# <p align="center">Contributing Limited Amount of Data-Node Storage to Hadoop Cluster</p>
![](https://miro.medium.com/max/875/1*EfM2GIjSp5YHvBfFx2bsoA.png)

# hello All,
# After Reading this article you will come to know how to contribute limited storage from data-node to Hadoop Cluster. I have done all the setup on AWS cloud so, No Need to worry about the resources such as RAM, CPU etc.
## So Without any further Delay, Let’s Get Started.


# Our Primary Goals:
## >> Launch a Name-node and a Data-node of AWS cloud.
## >> Create an EBS volume of 1 Gib and Attach it to Data-Node
## >> Make sure to install Hadoop software and JDK software in both the nodes.
## >> Create Partition(s) in attached volume.
## >> Format it and finally mount it on the Data-node Directory
## >> Setup the Name-node and Data-node
## >> Contribute Limited Storage from Data-node to Hadoop Cluster.
## >> assuming that you have some basic knowledge about Hadoop cluster and Cloud Computing as a prerequisite.
# First of I have launched 2 instances with RedHat Linux Image of Amazon with 10Gib EBS Storage in North Virginia region.
![](https://miro.medium.com/max/875/1*JUyh3jZa_f9VVuDUR2SIow.png)

# <p align="center">Instances Launched</p>
## After the instances are launched to do remote login I used PuTTY software.
# `Now after this we will create EBS volume of 1GiB by clicking on Volumes in AWS Console. I named it slave-volume. Notice here the 2 volumes of 10GiB are Root Volumes.`
![](https://miro.medium.com/max/875/1*PXUHxaRXuAVkmzi3NSamlQ.png)

# <p align="center">Slave Volume Created</p>
# *Now, We will attach our slave-volume to our datanode instance running. It is just like we insert pendrive in our local system or we create a virtual harddisk and attach to it. Mainly we have to give the instance id only while attaching*
# Attaching the volume is so simple, we just need to select that volume first, click on Actions Button and after that we have to provide instance id.
![](https://miro.medium.com/max/875/1*7j0sZuMeeCbu60Nvxw1deQ.png)

#  <p align="center">After attaching</p>
# `After attaching volume to data-node we can see that volume is in in-use status.`
# As we remotely logged in data-node instance previously, now we can see similar linux terminal and to confirm that EBS volume we created or not we can use fdisk -l command.
![](https://miro.medium.com/max/875/1*f2_NsUvtB_xl0og1nUaXzA.png)

# <p align="center">Volume is attached</p>
# Now the main concept of Partitions come into play. To share limited storage we are going to create partition in that 1GiB volume attached. Here we created partition of 512M(512 MiB). Here We can see the device /dev/xvdf1 created
![](https://miro.medium.com/max/875/1*A7C3FYf_-46tzm2lC5_WHQ.png)


# <p align="center">partition Created</p>
# Now we know we share the storage of Data-node to Name-Node to solve problem of storage we have in BigData. So, For storing any data in this partition, we have to first Format it.
# `To format that partition we created we need to run a command.`
# ```mkfs.ext4 /dev/xvdf1```
![](https://miro.medium.com/max/875/1*fegNOfdR5PiltBhZ5MPbxA.png)
# <p align="center">Formatted</p>
# Now, We have to mount it on the same directory of data-node we will be using in Hadoop Cluster.
# *To mount the partition on desired directory, we have to run the following command:*
# ```mount /dev/xvdf1 /dn2```
![](https://miro.medium.com/max/875/1*ayH3PPSWSTVCzgt8QUDqHg.png)

# After mounting we can confirm or see the size using df -hT command.
![](https://miro.medium.com/max/875/1*4S9c4Ldbs8qD02mge2dc7Q.png)

# Here all the steps of partitioning and mounting are completed.
# `Now we have to configure the hdfs-site.xml file and core-site.xml file in Name-node and Data-node.`
![](https://miro.medium.com/max/875/1*sauRAjW6kNywA_hBowbQkA.png)

# <p align="center">hdfs-site.xml file of Namenode</p>
![](https://miro.medium.com/max/875/1*cIKza8mFfEgmMR8YMKBVHg.png)

# <p align="center">core-site.xml file of Datanode</p>
# Remember, In Namenode we have we have to format /nn directory we created and After that start the service of NameNode using hadoop-daemon.sh start namenode.
# `Now we have to also configure the same files in Datanode also in the similar way, just remember to give IP of Name-node in core-site.xml file.`
![](https://miro.medium.com/max/875/1*jugVWtfHF2Zyj684qWDk0A.png)

# <p align="center">core-site.xml file of Datanode</p>
## Atlast, start the service of datanode also.
# Finally, Our all configurations are completed here.
# *Now by command hadoop dfsadmin -report ,We can see the status of hadoop cluster. This Command can be run from any of the node.*
# `Finally,we have set the limitation to data-node storage size.`
![](https://miro.medium.com/max/875/1*-y0ZmXyp2phdtJDMkLbN5Q.png)

# <p align="center">report of hadoop cluster</p>
