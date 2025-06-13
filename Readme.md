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
    
