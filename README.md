# Okta SSO Integration

## Overview ##
In this lab, I use some free online resources to build an integration between Okta and a Spring Boot web application to provide single-sign-on (SSO) authentication capabilities.

## Resources Used ##
The following resources were used to create this lab.

1. My laptop
2. Windows Subsystem for Linux (WSL) to run the Ubuntu terminal on my Windows OS: [link](https://ubuntu.com/desktop/wsl)
3. An Okta developer account: [link](https://developer.okta.com/signup/)
4. The code hosted on Okta's GitHub repo: [link](https://github.com/okta/samples-java-spring/tree/master/okta-hosted-login)

## Provision the Okta Developer Portal ##

1. On the dashboard at the far-right side of the top banner, the Okta domain is displayed. Take note of this value, it will be used later.

![dashboard](https://github.com/user-attachments/assets/2269a83c-cdf9-4348-83ba-0218f6399229)



2. I created an application called *My Web App*
   
![2](https://github.com/user-attachments/assets/97c9f7b8-fbd9-4844-970d-0e29cc42d4ce)

![3](https://github.com/user-attachments/assets/5f31a963-1429-4594-8a5a-33d750907c2b)



3. Take note of the Client ID and Client Secret, they will be used later.

![my web app id secret](https://github.com/user-attachments/assets/36fb1bb2-cc1f-45bb-800f-06f552d20f5c)


4. Take note of both the Sign-in and Sign-out redirect URLs. These are auto-populated by default and will conveniently work for this lab.
   
   The default Federation Broker Mode is enabled. I disabled it to better demonstrate the configurability of the access controls. 


![urls](https://github.com/user-attachments/assets/3099053a-ef7f-4577-9ae4-ad2127893324)



## Create a group and user for access to *My Web App* ##

1. I created a group called *My Web App Users*.

![8](https://github.com/user-attachments/assets/0ea0f4f8-8889-47c3-b168-e682d9bf0982)


## Create a user ##

1. I created a user called *John Wick*. The status is Pending as the user needs to complete registration with the Okta org.

![9](https://github.com/user-attachments/assets/30b45b1b-3255-4086-9e3e-0d1e151d8789)



2. User John Wick has just received the automated registration email from Okta.

![10](https://github.com/user-attachments/assets/b21a6b7d-6749-42ef-9a7b-af36821c41b5)


3. User John Wick clicks the link within the email.

![11](https://github.com/user-attachments/assets/b68df949-5b24-4470-b988-9da21042f6b5)



4. User John Wick creates a new password and enrolls in Google Authenticator for MFA.

![12](https://github.com/user-attachments/assets/3bf52c1a-4952-4d94-9c31-1ea400f0d60e)

![13](https://github.com/user-attachments/assets/8ba5d9f3-52e9-4119-8876-5bfd7a8a5da0)

![14](https://github.com/user-attachments/assets/704ec18c-62c8-40ae-a736-1e5f6758e7ef)

![15](https://github.com/user-attachments/assets/cb4ce28d-fec0-4598-8e4c-52d8f34b92c6)

![16](https://github.com/user-attachments/assets/a147581d-2d72-4a9b-b4ef-9a51b15e7c98)


5. Registration is complete and the user application dashboard is displayed.

![17](https://github.com/user-attachments/assets/da0c4c73-fb3c-4960-bda4-117743c6ddb9)



## Installing the pre-requisite software ##

1. From the WSL terminal, I downloaded updates then installed Maven and Java. I also had to update the WSL system time as there was significant drift compared to my Windows OS--if not corrected, it may cause issues later in the lab.

    sudo apt update
  
    sudo apt install maven
  
    sudo apt install openjdk-21-jdk-headless

    sudo ntpdate pool.ntp.org

## Begin the Lab ##

At this point, the instructions on the [README](https://github.com/okta/samples-java-spring/tree/master/okta-hosted-login) page of the GitHub repo can be followed. 

## Clone the GitHub repo that is listed in #4 above ##

git clone https://github.com/okta/samples-java-spring.git

## Navigate to the following directory ##

cd samples-java-spring/okta-hosted-login

## Enter the following commands to start the web application ##

The Okta Domain, Client ID, and Client Secret were found above.

mvn spring-boot:run -Dspring-boot.run.arguments="--okta.oauth2.issuer=https://**{OktaDomain}**/oauth2/default \
    --okta.oauth2.clientId=**{clientId}** \
    --okta.oauth2.clientSecret=**{clientSecret}** \
    --okta.oauth2.postLogoutRedirectUri=**{absoluteLogoutRedirectUri}**" 

![terminal](https://github.com/user-attachments/assets/2f9126e1-d74d-4af6-88d2-0a7017b39e7b)



## Browse to the web application and login ##
The web app is now running on the local machine and is listening for connections at HTTP port 8080. A web browser is used to navigate to the application.

http://localhost:8080

![my web app login](https://github.com/user-attachments/assets/35752dc1-1123-40a2-b734-502cab24ba74)




1. User John Wick attempts a login to the application. At this time, the authentication policy assigned to *My Web App* requires only one authentication factor, password.

![my web app login](https://github.com/user-attachments/assets/b5b5881d-c016-4a3a-af6f-ece2a73f60f3)





2. User John Wick has successfully authenticated to the web application.

![my web app auth](https://github.com/user-attachments/assets/9f8bdb5c-0585-4482-8519-3f32eb4597d5)





## Adding an authentication factor to enforce MFA ##

1. The following is a list of authenticators that Okta supports.

![21](https://github.com/user-attachments/assets/202a2e1a-a1c8-4116-bf39-3c6a13891e5e)


2. I choose *Google Authenticator* which will allow users to authenticate using a one-time passcode (OTP) from the Google Authenticator App installed on their     smartphone.

![22](https://github.com/user-attachments/assets/d26c64c8-a54d-4504-a7bc-f3ee627510e0)



3. The authentication policy on *My Web App* is set to *Password only*.

![23](https://github.com/user-attachments/assets/64b509a3-73eb-4c82-8627-14253e3c79f3)


4. I created an authentication policy called *Password and Google Authenticator* and assigned to *My Web App*.

![24](https://github.com/user-attachments/assets/171c7a82-9241-4266-bac5-3b34bc25c135)


![25](https://github.com/user-attachments/assets/e1ba5dc2-e2c0-455d-8954-eae86fb3513a)


![26](https://github.com/user-attachments/assets/943f5b48-58f7-453e-b19e-27f2e52574b4)



5. A new browser window is opened and user John Wick attempts another login to the web application using the password.

 ![27](https://github.com/user-attachments/assets/ffd65ab5-3fdf-461d-a6db-6d6d96cce044)


6. User John Wick is then prompted for MFA using Google Authenticator.

![28](https://github.com/user-attachments/assets/94a7ca00-2921-4f25-a56c-3e54b93c3988)


   Authentication is successful.
   
![my web app auth](https://github.com/user-attachments/assets/4a1e1c04-46d4-4c38-a326-cc008fc73228)





7. Below is a list of claims that was provided from the Okta access token.
   
![30](https://github.com/user-attachments/assets/6d7e0743-e7e9-4d48-8ce0-7b9ea4f7b79e)



8. Below is an access log tracking the activities of user John Wick.

![31](https://github.com/user-attachments/assets/00b33590-60c9-4447-8769-38b925a42315)



## Demonstrating SSO ##
Now let's demonstrate single-sign-on (SSO) by integrating Okta with a second web application.

## Creating a second web app within Okta Developer Console ##
I created a second web app called *My AWS Web App* within the console using the same process as above.


![32](https://github.com/user-attachments/assets/ac8aca75-fa62-41f3-af1c-27d23ba2f8df)


## Creating a second web server in AWS ##
I spun-up an EC2 instance within AWS that is running Ubuntu Server. The same GitHub repo was cloned over and this instance was configured using the same process as above.


![33](https://github.com/user-attachments/assets/e279f4ec-07a6-4706-a798-6f3b0be85136)


## Attempting login to the new web app ##
1. The cookies/cache on the web browser is cleared.
   
2. User John Wick attempts a login to the original *My Web App* running on the local machine.

![34](https://github.com/user-attachments/assets/df327ead-fd93-4525-a2e2-889bde62112c)

![35](https://github.com/user-attachments/assets/17ebbe65-c97c-496a-8f2d-9f1df16df610)

![36](https://github.com/user-attachments/assets/cc0f02f3-5800-4826-893c-38522c20e38b)



Authentication is successful.

![37](https://github.com/user-attachments/assets/0adabed7-ec60-4a88-ae18-7a0bddb2ab41)



3. User John Wick opens a new browser tab and navigates to the second web app, *My AWS Web App*.

![38](https://github.com/user-attachments/assets/d890ecd0-6636-4504-8569-9314e44a11c5)


4. User John Wick has been automatically authenticated to *My AWS Web App* without the need to manually login.

![39](https://github.com/user-attachments/assets/e2bebd60-314d-4e13-a787-06785c6fc833)



## Conclusion ##
In summary, a web application was configured within the Okta Developer Console which created a client ID and secret. A Spring Boot web application was then spun-up on my local machine. The application was configured to point to my Okta development domain and the same client ID and secret was specified for authentication. Once the integration was made, a test user was able to successfully authenticate to the web app using their Okta credentials. A new web application was added to the Okta Developer Console and deployed to an AWS EC2 instance. After authenticating to *My Web App*, the user was automatically authenticated to *My AWS Web App* without manually logging in. The benefit to leveraging SSO with federated identity providers like Okta is that users can authenticate to several applications using the same credentials. This was a fun lab. As someone with little programming experience and a growing interest in Identity Management, I appreciate the opportunity to have gotten some hands-on experience using these free resources.
