Introduction to Cloud Computing

You have been assigned to develop an e-commerce website for a new online marketplace named "MarketPeak." This platform will feature product listings, a shopping cart, and user authentication. Your objective is to utilize Git for version control, develop the platform in a Linux environment, and deploy it on an AWS EC2 instance. You can find a suitable website template here to kickstart your development

Task
1. Implement Version Control with Git

1.1. Initialize Git Repository
Create a project directory named "MarketPeak_Ecommerce" and initialize a local git repository by running git init in the MarketPeak_Ecommerce.
![create project directory](./img/initiatlizing%20git.png).


1.2. Obtain and Prepare the E-Commerce Website Template
Instead of developing the website from scratch, you'll use a pre-existing e-commerce website template. This approach allows you to focus on the deployment and operational aspects, rather than on web development. The actual web development is done by web/software developers on the project.

Download a Website Template: Visit Tooplate or any other free template resource, and download a suitable e-commerce website template. Look for templates that are ready to use and require minimal adjustments.

It is recommended you download the specific template

Prepare the Website Template: Extract the downloaded template into your project directory, MarketPeak_Ecommerce.

Customize the Template (Optional): If desired and you have basic web development skills, you can make minor customizations to the template to tailor it to "MarketPeak" This might include updating logos, changing color schemes, or modifying text to better fit the marketplace's brand identity.
downloaded website

1.3. Stage and Commit the Template to Git
Add your website files to the Git repository.
Set your Git global configuration with your username and email.
Commit your changes with a clear, descriptive message.

Run the following command to add the downloaded image to local github repo.
git add .
git config --global user.name "YourUsername"
git config --global user.email "youremail@example.com"
git commit -m "Initial commit with basic e-commerce site structure"

![adding to github](./img/git%20add.png)

1.4 Push the code to your Github repository
After initializing your Git repository and adding your e-commerce website template, the next step is to push your code to a remote repository on GitHub. This step is crucial for version control and collaboration.

Create a Remote Repository on GitHub: Log into your GitHub account and create a new repository named "MarketPeak_Ecommerce" Leave the repository empty without initializing it with a README, .gitignore, or license.

![creating remote repo](./img/creating%20git%20repo.png)

Link Your Local Repository to GitHub: In your terminal, within your project directory, add the remote repository URL to your local repository configuration.

copy git remote add origin https://github.com/your-git-username/MarketPeak_Ecommerce.git
Note: Make sure you replace "your-git-username" with your actual git username

![cloning git remote](./img/recloning%20after%20git%20installation.png)

Push Your Code: Upload your local repository content to GitHub, by running the following command

git push -u origin main
This command pushes your commits from your local main branch to the remote repository on GitHub, enabling you to store your project in the cloud and share it with others.

![pushing to main branch ](./img/git%20push.png)

2. AWS Deployment
To deploy "MarketPeak_Ecommerce" platform, you'll start by setting up an Amazon EC2 instance:

2.1. Set Up an AWS EC2 Instance
Log in to the AWS Management Console.
Launch an EC2 instance using an Amazon Linux AMI.
Connect to the instance using SSH.

![](./img/connect%20to%20EC2%20instance.png)
2.2. Clone the repository on the Linux Server
Before deploying your e-commerce platform, you need to clone the GitHub repository to your AWS EC2 instance. This process involves authenticating with GitHub and choosing between two primary methods of cloning a repository: SSH and HTTPS. To see the ssh or http link to clone your repository

Navigate to your repository in github console

Select the code as highlighted in the image below.

SSH Method:

On your EC2 instance, generate SSH keypair using ssh-keygen.Alt text
![generating key gen](./img/generating%20key-gen.png)

Display and Copy your public key
![public key](./img/copying%20public%20key.png)


Copy
cat /home/ubuntu/.ssh/id_rsa.pub
Alt text

Note: Your ssh public key will different

Add the SSH public key to your GitHub account.
![](./img/public%20ssh%20key%20added%20to%20git.png)

Use the SSH clone URL to clone the repository:
![](./img/error%20trying%20to%20clone%20git%20hub-ssh.png)

This error was encountered while trying to run the git clonme command, hence the need to intsall git. To install git, we 
- update the server as seen below
![](./img/updating%20and%20installing%20git.png)

confirm git is installed by running the command `ssh -T git@github.com

![](./img/running%20confirmation%20to%20git.png)

Reclone the repository by copying and pasting below command.
Copy `git clone git@github.com:yourusername/MarketPeak_Ecommerce.git`
![](./img/recloning%20after%20git%20installation.png)

HTTPS Method:

For repositories that you plan to clone without setting up SSH keys, use the HTTPS URL. GitHub will prompt for your username and password:

Copy
git clone https://github.com/yourusername/MarketPeak_Ecommerce.git

2.3. Install a Web Server on EC2
Apache HTTP Server (httpd) is a widely used web server that serves HTML files and content over the internet. Installing it on Linux EC2 server allows you to host MarketPeak E-commerce site:

Install Apache web server on the EC2 instance. Note that httpd is the software name for Apache on systems using yum package manager, copy and run the following command

Copy
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
This first updates the linux server and then installs httpd (Apache), starts the web server, and ensures it automatically starts on server boot.

![](./img/installing%20apache%20web%20service.png)

2.4. Configure httpd for Website
To serve the website from the EC2 instance, configure httpd to point to the directory on the Linux server where the website code files are stored. Usually in /var/www/html.

Prepare the Web Directory: Clear the default httpd web directory and copy MarketPeak Ecommerce website files to it. then
Copy
sudo rm -rf /var/www/html/*
sudo cp -r ~/MarketPeak_Ecommerce/* /var/www/html/
The directory /var/www/html/ is a standard directory structure on Linux systems that host web content, particularly for the Apache HTTP Server.

![](./img/clearing%20directory%20and%20copying%20marketPeak.png)

When you install Apache on a Linux system, the installation process automatically creates this directory. It's designated as the default document root in Apache's configuration, meaning that Apache is set up to serve web files (such as HTML, CSS, and JavaScript files) located in this directory to visitors of your website.

Reload httpd: Apply the changes by reloading the httpd service.

Copy below command and run 
sudo systemctl reload httpd

![](./img/reload%20changes%20by%20applying%20apache.png)
2.5. Access Website from Browser
With httpd configured and website files in place, MarketPeak Ecommerce platform is now live on the internet:

Open a web browser and access the public IP of your EC2 instance to view the deployed website.
N.B: enable inbound traffic on port 80 on the amazon web service.

![](./img/fully%20served%20web%20page.png)

3. Continuous Integration and Deployment Workflow
To ensure a smooth workflow for developing, testing, and deploying your e-commerce platform, follow this structured approach. It covers making changes in a development environment, utilizing version control with Git, and deploying updates to your production server on AWS.

Step 1: Developing New Features and Fixes

Create a Development Branch: Begin your development work by creating a separate branch. This isolates new features and bug fixes from the stable version of your website.

Copy
git branch development
git checkout development

![](./img/cfreating%20development%20branch.png)
![](./img/switching%20to%20dev%20branch.png)

Implement Changes: On the development branch, add your new features or bug fixes. This might include updating web pages, adding new products, or fixing known issues.
![switching branch](./img/switching%20to%20dev%20branch.png)

Step 2: Version Control with Git

Stage Your Changes: After making your changes, add them to the staging area in Git. This prepares the changes for a commit. Below change has been made to the telephone number
![](./img/data%20changed.png)

Copy
git add .
Commit Your Changes: Securely save your changes in the Git repository with a commit. Include a descriptive message about the updates.
![](./img/staging%20the%20changes.png)

Copy
git commit -m "Add new features or fix bugs"
Push Changes to GitHub: Upload your development branch with the new changes to GitHub. This enables collaboration and version tracking.

Copy
git push origin development
![](./img/pushing%20changes.png)


Step 3: Pull Requests and Merging to the Main branch
Create a Pull Request (PR): On GitHub, create a pull request to merge the development branch into the main branch. This process is crucial for code review and maintaining code quality.

![](./img/pulling%20dev%20to%20main%20branch.png)
and creating the pull request
![](./img/creating%20pull%20request.png)

Review and Merge the PR: Review the changes for any potential issues. Once satisfied, merge the pull request into the main branch, incorporating the new features or fixes into the production codebase.


Merge the pulled changes, by using the below command or by merging on github directly as shown below
Copy
git checkout main
git merge development
Push the Merged Changes to GitHub: Ensure that your local main branch, now containing the updates, is pushed to the remote repository on GitHub.

![](./img/merging%20the%20pulled%20changes%20to%20main%20branch.png)
and confirming merged changes
![](./img/confirm%20merged%20change.png).

Copy
git push origin main
Step 4: Deploying Updates to the Production Server

Pull the Latest Changes on the Server: SSH into your AWS EC2 instance where the production website is hosted. Navigate to the website's directory and pull the latest changes from the main branch.

Copy
git pull origin main
Restart the Web Server (if necessary): Depending on the nature of the updates, you may need to restart the web server to apply the changes.

![](./img/pulling%20the%20changes%20from%20remote%20github%20main%20to%20local%20main.png)

To effect the change and view on the site, we need to restart or reload the server.

sudo systemctl reload httpd

![](./img/Restarting%20apache%20serve%20to%20invoke%20project%20index.html%20change.png)
Step 5: Testing the New Changes

Access the Website: Open a web browser and navigate to the public IP address of your EC2 instance. Test the new features or fixes to ensure they work as expected in the live environment.

![](./img/changed%20telephone%20number.png)

This workflow emphasizes best practices in software development and deployment, including branch management, code review through pull requests, and continuous integration/deployment strategies. By following these steps, you maintain a stable and up-to-date production environment for your e-commerce platform.

This is the end of the project
