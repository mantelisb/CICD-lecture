# Instructions:

Register to Heroku on https://heroku.com. You might need to add your payment card but don't worry - you shouldn't exceed the free tier limit (If you have Revolut, just use virtual card for that).

## Examples

### Already configured repos
- https://github.com/mantelisb/mantas-2021-springboot
- https://github.com/mantelisb/mantas-angular-2021

### Already working apps (might be sleeping)
- https://mantas-2021-springboot-staging.herokuapp.com/
- https://mantas-2021-angular-staging.herokuapp.com/

## Back-End application

We will use simple springboot-example to set up everything.

First we need to clone already created repo into your GitHub account

Go to GitHub -> Repositories -> New -> Press "import a repository" ->
in old repository url enter https://github.com/mantelisb/springboot-demo-empty.git ->
enter name (it is needed that your name will be unique, from your colleagues, because later it can make collisions while deploying) ->
(leave privacy as Public) and press "Begin import"

Now it can run up to minute, and you will see success message:  `Importing complete! Your new repository "example repo name" is ready.`


### Set up Travis:

Now we are going to set up Travis for already created application

Go to Travis-ci.com and Sign up with GitHub. -> 
Accept the Authorization of Travis CI  ->
Click on your profile picture in the top right of your Travis Dashboard, click Settings and then the green Activate button, and select the repositories you want to use with Travis CI. ->
(If you still dont see newly created springboot application in travis, click "Manage repositories on GitHub", and select repos, you want be accesible for travis) ->
copy travis file for your repository from example: https://github.com/mantelisb/CICD-lecture/blob/master/Spring%20application%20files/.travis.yml ->
One of the option is to "Add file" directly to GitHub

After `.travis.yml` will be added, you will be able to see yellow/orange bubble in the repository, which means that Travis build is in progress, and we need to wait until it finishes

#### Travis possible build indicators:
- Yellow bubble which is indicating that this branch has Travis build in progress
- Red x mark, will show you that build failed
- Green check mark will tell you that Travis build succeeded

#### Dont be afraid of build failures, just press on it, and find the build logs, to find out what happened. Below i gathered failures which you probably will face it, while setupping everything:
- if you get `/home/travis/.travis/functions: line 351: ./gradlew: Permission denied`
that is fine, you just need to give permission by cloning repo and entering command in command line:
`git update-index --chmod=+x gradlew` ->
then commit and push


### Set up Heroku:

No we are going to deploy already created application in Heroku

go to https://dashboard.heroku.com/apps ->
New ->
Create a new pipeline ->
Enter pipeline name for your springboot application ->
Create pipeline -> 
add app in staging ->
Create new app... `your-repo-name-staging` ->
press on the app name ->
Deploy ->
Connect to GitHub ->
Search for a repository to connect ->
Connect ->
Mark `"Wait for CI to pass before deploy"` ->
Enable automatic deploys ->
deploy branch

Now you can go to Overview of this app, and in the Latest activity you will see that Build in progress, You can always View build log, to see the progress, and if any failures occurs, they also be visible in the log.

#### Database setup

good job, first app deployed!!! but it will fail, because it requires DB connection
go to Overview in app view in Heroku ->
Configure Add-ons ->
Search for Heroku Postgres -> 
and select Heroku Postgres ->
double check if Plan name is `Hobby Dev - Free` ->
Submit Order Form

Now we need to properly set up database credentials for our app.

go to Resources for app in Heroku ->
press on Heroku Postgres ->
Settings ->
View Credentials

Open Heroku app in separate tab ->
Settings ->
Reveal Config Vars

And here we will need to add 3 properties, based on information gathered from View Credentials.

- key: DATABASE_ADDRESS
value: postgresql:`Host`:`Port`/`Database`
- key: DATABASE_USERNAME
value: `User`
- key: DATABASE_PASSWORD
value: `Password`

Now press Open app, add /message at the end of url and Enjoy first response from you BE application!!!

If you see BE works:
that means that your application is up and running

Other wise try to Deploy your branchand test again, and if the problem persists you need to press More -> View logs -> and find out where is the problem.

Now try to append `/name` at the end of url, and it will add your name in to database. To check that delete `/name` from url and leave `/message` ending.

If you see BE works DB success message: `name`:
Congrats!!! continue


## Front-End application
Go to GitHub -> Repositories -> New -> Press "import a repository"  ->
in old repository url enter https://github.com/mantelisb/angular-demo-empty.git ->
enter name (it is needed that your name will be unique, from your colleagues, because later it can make collisions while deploying) ->
and press begin import


### Set up Travis:
Set up the same as for BE only travis file should be taken from: https://github.com/mantelisb/CICD-lecture/blob/master/Angular%20application%20files/.travis.yml


### Set up Heroku:
No we are going to deploy already created application in Heroku

go to https://dashboard.heroku.com/apps ->
New ->
Create a new pipeline ->
Enter pipeline name for your Angular application ->
Create pipeline -> 
add app in staging ->
Create new app... `your-repo-name-staging` ->
press on the app name ->
Deploy ->
Connect to GitHub ->
Search for a repository to connect ->
Connect ->
Mark `"Wait for CI to pass before deploy"` ->
Choose a branch to automatic deploy `master` ->
Enable automatic deploys

Go to settings, and add to Config Vars: ->
Key: `API_HOST`
Value: `your back end application url`
deploy branch

After deployment go to the app and you should see:
`Message from server: BE works DB success message: name`
good job, first app deployed!!!

if you see :
`Message from server: BE works`
that means DB is not connected to BE, or DB is empty

if you see: 
`Message from server: server is down...`
that means FE could not reach BE, most probably:
- BE is dead, 
- sleeping (Heroku app goes to sleep after 30 minn inactive, in this case try to directly call BE application and wait for it to wake up),
- you didnt entered API_HOST before deploying FE app


# Bonus:
- Create production apps in both pipelines!
- Try to add failing test, so Travis would fail, and Heroku won't auto-deploy
- Fix that test, so Heroku would auto-deploy  
- Set up branch permissions in Git (for your own good): Settings -> Branches -> [x] Require Pull Request -> [x] Require status checks to pass before merging and -> [x] Require pull request reviews before merging


#Result
- In order to present what you have done Send BE and FE apps and github repositories links (at total 4 links) 