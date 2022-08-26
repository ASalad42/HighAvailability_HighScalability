
# Highly Available and Scalable solutions with AWS

## Autoscaling and load balancer
Diagram of my architecture explaining steps and functionality of ALB -ASG - TG - Multi- AZs deployment

![image](https://user-images.githubusercontent.com/104793540/186675351-2cec10a9-2868-4b2b-8ac9-a0bd0551d8aa.png)
 

- Application Load Balancer distributes the traffic between EC2 instances so that no one instance gets overwhelmed 
- Autoscaling will automatically adjust the amount of computational resources based on the server load 
- Highly Available & Scalable 
- note: Vpc > subnet for private and public + gateways 
https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html


![image](https://user-images.githubusercontent.com/104793540/186645511-810feced-32b6-471c-a9ba-ab684bb591ec.png)
![image](https://user-images.githubusercontent.com/104793540/186645544-ab9dea3e-0ec8-438e-b06e-3c3676666ca6.png)
![image](https://user-images.githubusercontent.com/104793540/186645852-7eaf2dca-b91a-49e8-9da7-4a5ae3313028.png)
![image](https://user-images.githubusercontent.com/104793540/186646249-f55cd730-845a-4aee-862d-bf16e3556068.png)
![image](https://user-images.githubusercontent.com/104793540/186646325-a693b20c-4587-457b-b076-eacbc8340733.png)
![image](https://user-images.githubusercontent.com/104793540/186646812-b7bda242-fc84-48a3-a687-440414cb12f9.png)


highly available 
![image](https://user-images.githubusercontent.com/104793540/186648963-a8a4fbbf-7007-4d4e-8273-714a81df638c.png)
![image](https://user-images.githubusercontent.com/104793540/186663691-9fd4449e-e78b-460e-bb85-06dc39c1ab01.png)

create an autoscaling group with min 2, desired 2 max 3 - based on cpu usage 40%:
![image](https://user-images.githubusercontent.com/104793540/186688879-197d58d2-8271-48fb-a580-3e4bb77911a4.png)
![image](https://user-images.githubusercontent.com/104793540/186689668-6c2798a0-8e0d-434f-82d8-d6bbff3d8cc1.png)
![image](https://user-images.githubusercontent.com/104793540/186690007-10f31dc9-5b7c-48c6-a4e2-d51500bcd01d.png)
![image](https://user-images.githubusercontent.com/104793540/186691684-f302c843-2b7f-405e-b03f-3fcf1cf15451.png)
![image](https://user-images.githubusercontent.com/104793540/186691927-e41755f3-faf9-4259-a062-911865573d29.png)
![image](https://user-images.githubusercontent.com/104793540/186692021-e58976c0-1a65-4288-ab94-9d6e2a89e5ec.png)



Debugging npm for ami before asg:

- Npm is starting when ec2 is being installed 
- User data shell gets terminated after instance is launched 
- Create service > enable service  > build ami and npm will already running
- Ec2 with nodeapp working, enabled in the background > make ami and it should be working > it did!
- Issue was: User data was being run while machine was being created then its gone 

https://unixcop.com/how-to-create-a-systemd-service-in-linux/
https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html

in nodeapp and database > while pages were working i did the following:

- You can find running linux service under path /etc/systemd/system
- `cd /etc/systemd/system`
- `sudo nano npm.service`
- add the following:
- 
```
[Unit]
Description=running npm app

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/deployment/app
ExecStart=/usr/bin/npm start
Restart=always

[Install]
WantedBy=multi-user.target
```

- `sudo systemctl daemon-reload`
- `sudo systemctl start npm.service`
- `sudo systemctl status npm.service`
- `sudo systemctl enable npm.service`

![image](https://user-images.githubusercontent.com/104793540/186716710-f75f542c-1ebb-453b-b2cc-c1a8dc6785cd.png)

- create ami of this now (YESS instantly worked with ip) (made ami using nodeapp 
- launched
- ![image](https://user-images.githubusercontent.com/104793540/186875890-aac128aa-7b54-4e6a-b75a-eb66c95cde2e.png)

- ![image](https://user-images.githubusercontent.com/104793540/186875921-1c327ba9-4460-4207-ab9d-ed1a4b4274c2.png)

To do:
- launch template from that 

![image](https://user-images.githubusercontent.com/104793540/186877303-dce6d6b2-bded-4b26-8996-058dbcba072f.png)

- auto scale 
![image](https://user-images.githubusercontent.com/104793540/186877594-f9c8ed13-5745-4113-aca7-f2944e51bbb1.png)

target group - Listeners and routing
![image](https://user-images.githubusercontent.com/104793540/186877913-8ad5dca1-c4da-4a1d-be93-81c979012c43.png)

Results:
![image](https://user-images.githubusercontent.com/104793540/186879017-0af48590-d85e-4f59-b54f-7bf2910aa6fa.png)
![image](https://user-images.githubusercontent.com/104793540/186879142-a287a65b-1e55-4cad-a0f9-c5c1d789e0f6.png)
![image](https://user-images.githubusercontent.com/104793540/186878973-b7dfc6c5-6406-49f6-bbbe-56921d4e082d.png)


