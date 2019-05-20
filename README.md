# ftp-server
Create an ftp server quickly on aws

## create an ec2 instance (t3a-micro) and set security groups

inbound ports - 20, 21, 21100-21110

## install docker

sudo apt-get update && sudo apt upgrade -y

## create new EBS sc1 volume of 1TB and mount 

sudo mkfs -t ext4 /dev/nvme0n1

mkdir /home/ubuntu/ftpstore

sudo mount /dev/nvme0n1 /home/ubuntu/ftpstore

sudo vi /etc/fstab

add:
/dev/nvme0n1       /home/ubuntu/ftpstore   ext4    defaults,nofail               

## Start vsftpd service

docker run -d -v /home/ubuntu/ftpstore:/home/vsftpd \
-p 20:20 -p 21:21 -p 21100-21110:21100-21110 \
-e FTP_USER=camuser -e FTP_PASS=SuperPassword#123 \
-e PASV_ADDRESS=18.224.36.65 -e PASV_MIN_PORT=21100 -e PASV_MAX_PORT=21110 \
--name vsftpd --restart=always fauria/vsftpd

