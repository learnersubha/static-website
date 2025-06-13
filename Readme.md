 ############################# STATIC WEBSITE HOSTING BY S3 ##########################

 Step-1 :  **Prepare your website**
    === ----->> Create a folder on your local and enter to the folder (mkdir my-website && cd my-website)
        ----->> add a basic index.html file  index.html
                                 <html>
                                   <head><title>My Portfolio</title></head>
                                   <body><h1>Hello from S3 Website!</h1></body>
                                 </html>
 Step-2 :  **Create an IAM role with S3 access**
    === ----->> Required this IAM role with S3 access for uploading file in S3 and also configure aws in local.
               --> in aws search IAM and open it --> click on Users --> click on create user and give a name of your user and click on next (for this user not required aws console access not check on provide user console access) --> click on attached policy directly and search for S3 acess and check on that and click on --> click on create user
               
 Step-3 :  **Configure S3 bucket**     
    === ----->> Open your AWS accout --> search S3 and click on S3 --> click on create bucket --> give a unique name your S3 bucket (my-website.com) --> uncheck block all public access
       Upload your index.htm into S3 bucket: -->
            For uploading file in S3 from your local required aws cli to access your S3 --> First download aws cli zip file in your local ($ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"unzip awscliv2.zip) --> For access the file you must be dowload unzip (sudo apt-get install unzip) --> then install aws cli (sudo ./aws/install)
        --> upload the file index.html to the S3 bucket (aws s3 cp /path/to/local/file.txt s3://your-bucket-name/) 
  Step-4 :  **Enable static hosting** 
     === -----> Open your bucket --> click on properties --> go to static hosting and click on edit --> static website hosting: Enable --> Index Document: file name which one you want to hosted --> click on save changes  --> open your bucket again and click on permission (For make file permission) --> bucket policy --> and paste the below 
               {
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::S3 bucket name/*"
		}
   
	]
}
   -----> click on save chjange 
   --> again click on your bucket --> properties --> static website hosting --> you get a website endpoint --> copy that and check on your browser.

   Step-5 : **Now its time to setup Route53**
     === -----> For setup route53 you must need a domain. 
          --> Go to route53 and open it. Now if you buy domain from aws route53 you got hosted zone other wise if you buy a domain from hostinger or godaddy you need to create a hosted zone. --> click on hosted zone in the above left side of your screen --> click on create hosted zone and give your domain name and keep type public hosted zone and click on create hosted zone.
   Step-6 : **Create Route53 record**
     --> Make sure your S3 bucket name and your domain name is same. because S3 Static Website Hosting Directly Uses the Bucket Name as the Website Endpoint. If bucket name does not match with the domain it would not work directly with static website hosting, At that time you need CloudFront whick can point to any bucket.
     ---> Record Name: You can use www or you can keep it blank
          Record type: A Routes traffic to an IPv4 address and some aws resources
	  Alias : Yes
          Route traffic to : alias to S3 website endpoint
	  Region : Choose your region 
          alias target : 	s3-website-eu-west-1.amazonaws.com
	  Routing policy : Simple
          Evaluate target health : No

   Now create the record and go to any browser and check with your domain.
  
    
