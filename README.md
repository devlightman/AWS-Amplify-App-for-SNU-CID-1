# AWS Amplify Serverless App for SNU Creative Integrated Design 1

This is the code repository of Team S for [Creative Integrated Design 1](http://dcslab.snu.ac.kr/courses/2021f/project/). This project aims to implement a cross-platform hybrid serverless mobile app for art class teachers and students based on Amazon Web Services (AWS); in particular, AWS Amplify.

## Developer Guide

Refer to the [Github Wiki](https://github.com/devlightman/AWS-Amplify-Serverless-App-for-SNU-CID-1/wiki) for detailed information on installation and setup.

## Abstract
We plan to implement a cross-platform hybrid serverless mobile app for art class management with interactive features. For convenience of our team members, we used and tested the application on android platform only, but it can be implimented and used on iOS platform without ant core change to the codebase. Most of the promised features are implemented and functioning well, with only a few minor functionalities cut due to limited time and resources. It has a complete serverless backend architecture and MVVM frontend architecture offering support for new user signing-up, logging-in, creating, searching, joining and unjoining classes, creating assignments, uploading and sharing artwork, social media account log-in, scoring feature for the submitted assignment, comment on the artworks uploaded by classmates, as well as interacting with other users via likes. New features can be easily added because of the advantages in the overall architecture. As some of the features are not fully implemented during the semester due to limited time and resources, we added as much feature as we could to the art class management application.

## Introduction
Smartphones have been popular since early 2000s. To this day, modern smart phones have large screens, stable internet connectivity and mature software distribution channels providing a lively environment for various 3rd party applications. Meanwhile, the development of cloud services also makes server setting up easier than before.
Our project is to develop an Android application for art class management, utilizing AWS platform for backend service. The application provides elegant UI with ease of usage designed for all age groups. The core functionalities include account management, joining/retrieving from a class, publishing student homework, uploading artwork and interacting with other users.
We will first introduce the background study of which niche our application fits in the market and the development environment we used. Next, we will describe the goal of our project, the problems to solve and the requirements to meet. Afterwards, we will discuss our approaches and project architecture for our application and provid the implementation details. The appendix attached is the detailed spec with the functions and logics we implemented.

## Background Study
### a. Analysis of Related Approaches and Technologies
The existing apps with similar features can be roughly classified in three categories: social media apps with art sharing functionalities, professional artwork sharing platforms and class management apps.
Popular social media apps like Twitter, Instagram, etc, all have picture sharing function built in. The advantages of these platforms are as follow. First, most users already have the account and client for daily use. Second, the UI and UX design of these popular applications have been mature and time proven. The users do not need to learn a new operating logic. Finally, these apps provide complete and various ways for interaction between users. The disadvantages include the followings. They do not have a class management function for either class member management or homework publishing. A student’s uploaded homework will be publicly available by default if he/she is using an existing daily account. These drawbacks make them not suitable for art class management.
Professional artwork sharing platforms like Pinterest focus on picture publishing. They have less irrelevant functionalities and provide better support for art sharing. However, these platforms usually lack means for interaction, making discussion among students and with teachers difficult. In addition, similar to the social media apps, they do not provide a class management function as well.
Class management apps are less populated, and we failed to find a dominating example in this category. They usually provide basic classroom management functions but lack other features. Most of them are designed for either a complete off-line classroom or complete on-line education, unsuitable for the targeted group of our project. Other than the lack of artwork sharing and interactive features, the UIs are usually less polished and not easy for senior users.

### b. Project Development Environment
The project is divided into the frontend and backend part.
The frontend utilizes Android Studio based on IntelliJ IDEA as IDE, and Ionic with React as the framework to construct the app’s UI. Visual Studio Code is also used as light-weight code editor when testing with WebView UI.
Ionic is an open-source mobile toolkit for building high quality, cross-platform native and web app experiences. It provides support for the latest UI style and is easy to use for fast development as well as deployment. This project plans to make heavy use of web view technology and Ionic provides easy transformation between web page and native web view fragment for mobile systems. This also allows easy transplant for other mobile systems like iOS.
For backend part, instead of the traditional client-server structure, this project uses a serverless model, which means it contains no dedicated backend server code but rather relies on cloud computing service to provide proper backend support. In this project, the app utilizes APIs of AWS (Amazon Web Service) Amplify for user credential management, database management and cloud storage.

## Goals & Requirements
The goal of our project is to implement an art class management application on Android system. The application should have the following features:
  * User signing up, logging in, changing password, and deleting account
  * User searching a class
  * User commenting and liking others’ pictures
  * Teacher hosting and retrieving a class
  * Teacher allowing and rejecting a student to join the class
  * Teacher publishing and retrieving a homework assignment
  * Student joining and unjoining a class
  * Student uploading pictures or text for homework
  * Log-in feature using social media 
  * Scoring feature for the submitted assignments

Other than implementing the features above, the application should have a clear logic, elegant UI design and offer support for accessibility features like changing font size, and dark mode.

## Approach
In terms of UI framework choice, Ionic is chosen to develop Hybrid WebView app in this project. It allows us to develop using pure web tech stack and provide one-time coding, running everywhere capability for easier debugging and future transplanting. Furthermore, Ionic provides UI components with native translation, resulting in better visual alignment with the operating system visual style as well as better performance. Finally, Ionic provide complete documentation and good beginner guide. This helps us better understand and utilize the functionalities of the framework.
For script language framework, React is used in this project. The reason to choose React over other similar targeting ones like Angular or Vue is the following two points. First, React provides more fine-grained event-driven functionality. This is beneficial especially to the MVVM design paradigm we decide to use. Second, compared to the competitors, React is more friendly to use for beginners due to the lack of experience in our team.
We used AWS Amplify for serverless backend for its elastic deployment and scalability. It also grants us access to various time-tested services from Amazon Cloud, which is helpful for our project with a tight development schedule.
Overall, the Model-View-ViewModel (MVVM) design paradigm is used when designing the project structure. The loose coupling between frontend and backend allows more parallelism in development and less trouble when refactor is needed.

## Project Architecture
### a. Architecture Diagram
![image](https://user-images.githubusercontent.com/20808296/171089272-4a207374-9a3f-458d-b4aa-3e1c2688452a.png) Figure 1. Frontend Client Architecture

![image](https://user-images.githubusercontent.com/20808296/171089579-9ec5e949-5201-4b22-976f-5cfc072a2fcc.png) Figure 2. AWS Cloud Based Backend Architecture

### b. Architecture Description
The project is divided into two parts, the frontend client application built with Ionic framework and the AWS Cloud based backend.
In the frontend client, there are three main components, as App.tsx, AuthorizedRoute, UnauthorizedRoute and LocalStore. When the app starts, App.tsx works as the entry point of the application. LocalStore is the local storage for data such as user information, cached pictures, class, assignments, etc. It also works as the bridge between the client and AWS Cloud services. AuthorizedRoute and UnauthorizedRoute are different workflow for different users.
When the client starts, based on the local authentication status, the control will be directed to UnauthorizedRoute or AuthorizedRoute. UnautorizedRoute is for unlogged in users. It consists of one login page, with LoginView and SignupView and their shared ViewModel controlling the logging-in and signing-up process. AuthorizedRoute is for logged in users. It contains three pages as FeedPage, ClassPage and ProfilePage. FeedPage is to show the uploaded work from other users and interact via likes. ClassPage is for creating, searching, joining and unjoining a class. ProfilePage is for managing a user’s profile, including the account information and client local settings. Each page consists of a View, a ViewModel and several Modals, which have a similar structure as a page.
The client communicates with AWS Cloud via Amplify SDK APIs. Several AWS services are involved in the app’s backend. AWS DynamoDB is used for storing information including students, teachers, classes, comments and assignments. AWS S3 is used for storing the user uploaded pictures as well as the compressed thumbnails. AWS Lambda allows functions being run on the cloud as well as updates DynamoDB and S3 for data storage. It also enables the pictures to be compressed on the cloud. AWS Cognito is used for user signing up verification, and AWS AppSync is used for accessing data in DynamoDB.

## Implementation Spec
### a. MVVM Structure Implementation
We used MobX to implement MVVM architecture. MobX provides the follow key features that allow its users to easily implement the observer pattern design paradigm: MobX allow its user to mark something as “observable” and something as its “observer”; the functions marked as “action” will set the value of the observables and functions marked as “computed” will get the value of the observables.
In practice, the Models are represented by Store. And the properties in Store are marked as “observables”. The setters and getters are marked as “actions” and “computed” respectively. View and ViewModel marked as “observer”.
The binding between Store and ViewModel is realized by injecting the Store to be observed into ViewModel via React Context.
Apart from the canonical observer pattern, we also implemented “autoRun” to persist data intto local storage when needed.

### b. MVVM Components
The main components are introduced in b. Architecture Description. The Models abstracted in our application are included in LocalStore components, consisting of UserStore, ClassStore and AssignmentModel, and their features are as the names suggest. The Views and ViewModels are included in each page and modal component for drawing the UI elements and logics respectively.

### c. Special Components
The special components are mostly for handling page navigation logic. They are dispatched in App.tsx, which is the entrypoint of the application. To further handle the navigation for different log-in status, two helper components called AuthorizedRoute and UnauthorizedRoute are created for logged in user and non-logged in user respectively. The navigation is made possible using the React Router component.

## Results

### a. Structures
All of the structures described in "b. Architecture Description" are implemented. Due to the limited time and resources, some UI elements are not in the most elegant form but all of them are functioning well.

### Functionalities 
In the final product, the following functionalities are completed:

  * A new user can sign up with email address, password and account type as either Student or Teacher. After submitting the sign-up request, a verification     email will be sent to the registered email address with verification code. The user will be automatically logged in after entering the correct verification code.
  * A user with existing account can directly log in with the email address and password.
  * After logged in, there are three main pages, Feed, Class and Profile.
  * Assignment grading function for teachers
  * Social media account login (SNS login)
  * Comment function in the Feed page
  * In Feed, a user can see the uploaded work from other users who are in the same class with him/her. The user can hit the like button beneath the picture to interact with the uploader. User can also upload pictures for certain assignment given by the teacher of the class the user joined.
  * In Class, different features are given based on the user account type. A Teacher can create a class with titles and descriptions. A teacher can also give out assignments for a class, so that every student should upload a picture to finish it. A student can search for and join classes with keywords, or unjoin a class he/her is already in. Students can also check the details of a class to see whether there is a new assignment to finish.
  * In Profile, a user can upload or edit its avatar, check the build number of the client or choose to log out.
  * The basic functions in the initial plan are mostly finished with a few exceptions due to the limited time and resources, including push notification, commenting artworks, 3rd party account binding and UI accessibility features.

## Conclusion
This project developed an AWS Cloud Native Art Class Application using Ionic framework. It offers cross-platform accessibility with features for managing an art class and sharing artworks. The utilization of AWS Cloud services allows for simple yet powerful serverless backend support, and Ionic framework on top of React gives excellent flexibility and elegant UI components.

## [Appendix A] Detailed Implementation Spec
Because the concept of AWS Cloud based serverless backend is adopted, the configuration of the backend service is done through webpage GUI interface provided by Amazon. Thus, no detailed implementation spec can be shown for the backend part.
Due to the utilization of MVVM design paradigm, the most important logics are implemented in each ViewModels. Views are only for drawing UI and Models are only for data storage. Furthermore, most components does not have any logic after creation and initialization, and because of the functional programming paradigm, each component is a callable object with most of its CRUD logics wrapped in. Thus, only some of the key functions in ViewModels are listed.

### AuthViewModel
  1. const isUsernameValis = (username: string): boolean
     Verify whether the coming username is in valid form.
  2. const isPasswordValid = (password: string): boolean
     Verify whether the coming password is in valid form.
  3. const signUp = async (username: string, password: string)
     Sign up a new user with provided credential.
  4. const confirm = async (username: string, password: string, code: string)
     Send the user credential with the verification code, will confirm the account if correct.
  5. const onSubmit = async (username: string, password: string)
     Submit the user credential and try to login.
 
### FeedViewModel
 ### a. ArtworkModal
 
   1. const tryLike =
   
      Try to like current shown artwork. If the current user already likes it, cancel the like.
 
 ### ClassViewModel
  ### a. ClassItemModal
   1. const setShowClassDetails = shouldSho w: boolean
   
      Show the class details when interacted with.
    
  ### b. ClassDetailsItemModal
   1. const setAddAssignment = should Add: bo olean
   
      Enter add assignment interface when interacted with.
  
  ### c. AddAssignmentModal
   1. const setDescription = description: string
   
      Set the description for an assignment.
   2. const set StartTime = startTime: strin g
   
      Set the start time of an assignment.
   3. const setDeadline = deadline: string
   
      Set the deadline of an assignment.
      
 ### ProfileViewModel
   1. const signOut = async ()
   
      Sign out the current user.
  
  ### ProfileEditModal
   1. const getPhoto = async (sourceStr: string, shouldCrop: boolean)
   
      Get a photo for user avatar from specified source.
 
 ### ArtWorkStore
   1. updateArtWorkGrade = async (artwork: Artwork, grade: number) => {}
   
      Update the grade of the given artwork.
      
 ### CommentStore
   1. constructor(rootStore: RootStore) {}
   
      Calls initialize() to initialize the store. User autorun() to listen to any possible changes that should trigger update in the values in the store.
   2. Initialize = async () => {}
   
      Fetch comments from the backend datastore.
   3. addComment = async (artwork: ArtWork, message: string) => {}
   
      Add a comment to the backend datastore.
   4. deleteComment = async (artwork: ArtWork, comment: Comment) => {}
   
      Delete a comment from the backend datastore.
   5. updateComment = async (artwork: ArtWork, comment: Comment) => {}
   
      Update a comment and upload to the backend datastore
      
 ### UserStore
   1. federatedLogIn = (provider: string) => {}
   
      Implement sign in using social media accounts
      
## [Appendix B] User Manual

The distributed product of this project is an android installation pack file (.apk). Clicking on the installation file will trigger the system installation process. Follow the instruction given by the OS to complete installing.
After installation, an icon should show up in the launcher, click the icon to start the app. The instructions for different screens of the app are shown as follow.


![image](https://user-images.githubusercontent.com/20808296/171093261-4a13d377-2066-4933-8d41-f1967b365b01.png)
[First Screen]

① Log in with a Google account on device

② Create an account for a new user to sign up

③ Log in with an existing account (Login with 3rd party apps is currently not supported)


![image](https://user-images.githubusercontent.com/20808296/171093315-c2775ef4-b6a5-4b2f-9fff-e7932d5ed2dc.png)
[Sign Up Screen]

① Enter the email address bind with the new account

② Enter the password (must be combination of Upper-case letters, smaller case letters and numbers)

③ Repeat the password

④ Select the role between student account and teacher account

⑤ With all information correctly entered, sign up new account


![image](https://user-images.githubusercontent.com/20808296/171093356-e84f3145-50e0-4e70-9c61-b293475df9f9.png)
[Log In Screen]

① Enter the email address

② Enter the password of the account

③ With all information correctly entered, log in with the provided account


![image](https://user-images.githubusercontent.com/20808296/171093380-de170ef7-6bfd-4c0a-85e2-00be469a58df.png)
[Feed Screen]

① Tab here to enter the Feed page (default)

② Tab to enter Class page

③ Tab to enter Profile page

④ Current page location

⑤ Artwork uploaded by other users

⑥ Like button to show whether you like this artwork (tap again to cancel)

⑦ Type your comment here to express your feelings


![image](https://user-images.githubusercontent.com/20808296/171093428-35afaad4-e3b9-486b-bb81-14ceed6cc15a.png)
[Class Screen]

① List of currently attending classes, tab to show the details and assignments of this class

② Tab to create a new class (or search for a class if using a student account)


![image](https://user-images.githubusercontent.com/20808296/171093466-b862150a-a58a-47d9-ba8e-f917dc4ff9e6.png)
[Class Details Screen]

① Tab here to return to previous screen

② Title of the class

③ Class description

④ List of uploaded assignments, tab to show details (red circle for an overdue assignment)

⑤ Upcoming assignment, tab to show details


![image](https://user-images.githubusercontent.com/20808296/171092636-772fbd23-b50e-4ef7-9fdc-0cca6121da1d.png)
[Assignment Details Screen]

① Tab here to return to previous screen

② Assignment name

③ Assignment description

④ Assignment deadline

⑤ Submission condition of the assignment

⑥ Tab here to expand submission menu


![image](https://user-images.githubusercontent.com/20808296/171092660-09d8c11a-7463-4cc3-87da-b5f7763a5889.png)
[Assignment Details Screen - Menu Expanded]

① Take a photo using camera

② Select a picture from gallery


![image](https://user-images.githubusercontent.com/20808296/171093647-5f82a430-3243-4c01-8510-fa3cf7d05636.png)
[Assignment Details Screen (Teacher)]

① Tab here to return to previous screen

② Assignment name

③ Assignment description

④ Uploaded artwork from students

⑤ Tab here to grade the student’s work


![image](https://user-images.githubusercontent.com/20808296/171093826-15a9811a-f56f-482d-acf3-aced274bad6e.png)
[Assignment Details Screen (Teacher Grading)]

① Enter the grade for this student’s work

② Cancel or confirm the grading


![image](https://user-images.githubusercontent.com/20808296/171093903-01aa8e8c-4ea2-46ca-9383-39e73a6db27d.png)
[Class Search Screen]

① Tab here to return to previous screen

② Enter the keywords to search for relevant classes


![image](https://user-images.githubusercontent.com/20808296/171093941-9bd1864b-8d63-448c-83bc-5d49529dc2d9.png)
[Class Creation Screen]

① Tab here to return to previous screen

② Enter the name for the new class

③ Enter the class description

④ Tab here to confirm creating the class


![image](https://user-images.githubusercontent.com/20808296/171093991-bdc0bcf2-90e5-4aa4-9230-15152b38526b.png)
[Assignment Creation Screen]

① Tab here to cancel the creation

② Enter the description for this assignment

③ Enter the start time that students can submit works

④ Enter the deadline for the assignment

⑤ Confirm adding the assignment


![image](https://user-images.githubusercontent.com/20808296/171094065-63eb4efb-4742-4d9d-b9fb-b83abf8d86ac.png)
[Assignment Start/End Time Edit Screen]

① Slide the slider to select correct time

② Cancel or confirm the time editting


![image](https://user-images.githubusercontent.com/20808296/171092724-1039beb0-b1cf-4e01-8f94-82be707e0c1c.png)
[Profile Screen]

① The username for current user

② Avatar for the current user, tab to edit

③ Tab to log out the current account

④ Tab to see the client build number


![image](https://user-images.githubusercontent.com/20808296/171094152-ff86ff99-b8db-4a21-9209-13ebbe0101ce.png) 
[Avatar Edit Screen]

① Tab here to return to previous screen

② Preview of the current user avatar

③ Tab to see the expand menu for uploading new avatar, a pop-up window will show up for choosing from gallery or taking a photo
