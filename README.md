# code-challenge

For this exercise, I took the approach of creating the HA infrastructure within a VPC as described in the problem. Within the VPC there are 2 public subnets, and 2 autoscaling groups spanning each subnet. In case of an AZ going down, the load balancers always have a target and always returns a web page, providing the best user experience.

First, I started out by building the VPC with appropriate components. This involves creating the VPC itself, and adding the subnets. You also need routing and networking components, so I added a web server security group for the instances themselves. 

Additionally, in order to service internet traffic, I also created an internet gateway as well as a route table with a public route back out to the internet through the IGW.

Next, I created the autoscaling group and launch configurations to serve the simple static page. To route traffic to each of these, I added the ALB deployed to both subnets, as well as the target groups.

In order to deploy this stack, you must provide the number of instance you want per target group, instance type, EC2 key pair, and the IP you would like to SSH in from. All of these have a default value you don't need to touch except the EC2 key pair.

For the bonus points, I created an S3 bucket containing an image, with a CloudFront distribution pointing to it (with caching). You can see this image in the web page.

There is a second application deployed with this template. In order to access it, you can simply add a parameter to your URL to see it. This is accomplished through the ALB Listener, which looks for a query parameter "switch" and if it is set to "true", it forwards the request to the other application. No need to run a script to switch the ALB targets, its already built in to the load balancer's listener.
