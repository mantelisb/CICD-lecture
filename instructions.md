# Instructions:

Register to Heroku on https://heroku.com. You might need to add your credit card but don't worry - you shouldn't exceed the free tier limit.

## Examples:

- SpringBoot: https://github.com/mantelisb/springboot-demo
- Angular: https://github.com/mantelisb/angular-demo

## Back-End application

Go to GitHub -> Repositories -> New -> Press "import a repository"  ->
in old repository url enter https://github.com/mantelisb/springboot-demo-empty.git ->
enter name (it is needed that your name will be unique, from your colleagues, because later it can make collisions while deploying) ->
and press begin import


### Set up Heroku:
go to https://dashboard.heroku.com/apps ->
create new pipepline for springboot application `your-repo-name` ->
add app in staging ->
Create new app... `your-repo-name-staging` ->
press on the app name ->
Deploy ->
Connect to GitHub ->
Search for a repository to connect ->
Mark `"Wait for CI to pass before deploy"` ->
Enable automatic deploys ->
deploy branch ->

if you get `./gradlew: Permission denied`
that is fine, you just need to give permission by cloning repo and entering command in command line:
`git update-index --chmod=+x gradlew` ->
then commit and push

if you get `Could not target platform: 'Java SE 11' using tool chain: 'JDK 8 (1.8)'.`
clone repo and add `system.properties` file from: https://github.com/mantelisb/CICD-lecture/blob/master/Spring%20application%20files/system.properties ->
then commit and push

good job, first app deployed!!! but it will fail, because it requires DB connection
go to Overview in app view in Heroku ->
Configure Add-ons ->
Search for mongo, and select mLab MongoDB

press on add-on mLab MongoDB ->
Add Collection ->
with name: message ->
press on created collection and Create Document ->
`enter: {"id":"1","message":"DB works"}` ->
deploy branch again

Now press Open app and Enjoy first response from you BE application!!!

If you see BE works:
that means connection with DB doesn't work, or entry in DB is not inserted

If you see BE works DB works:
Congrats!!! continue


### Set up Travis:
Follow instructions: https://docs.travis-ci.com/user/tutorial/  ->
Create travis file frome example: https://github.com/mantelisb/CICD-lecture/blob/master/Spring%20application%20files/.travis.yml

if you get `/home/travis/.travis/functions: line 351: ./gradlew: Permission denied`
that is fine, you just need to give permission by cloning repo and entering command in command line:
`git update-index --chmod=+x gradlew` ->
then commit and push

if you get `com.example.demo.DemoApplicationTests > contextLoads FAILED`
keep calm, we need to copy env variables from heroku app

go to heroku app ->
go to Settings ->
Reveal Config Vars ->
Copy mongodb url that looks similar to `mongodb://heroku_......` ->
now we will make Travis build green :)

Go into Travis again ->
From My Repositories choose which failed before ->
More options -> 
Settings ->
Where is Environment variables we add one element:
Name: `MONGODB_URI`
Value: paste which looks similar to `mongodb://heroku_......`
Press add ->
More options -> 
Trigger build ->
Enjoy you green CI job
now every commit will invoke Travis build, and if Travis build is successful, Heroku will deploy it :)



## Front-End application
Go to GitHub -> Repositories -> New -> Press "import a repository"  ->
in old repository url enter https://github.com/mantelisb/angular-demo-empty.git ->
enter name (it is needed that your name will be unique, from your colleagues, because later it can make collisions while deploying) ->
and press begin import


### Set up Travis:
Continue from 3-rd step in https://docs.travis-ci.com/user/tutorial/ ->
Create travis file frome example: https://github.com/mantelisb/CICD-lecture/blob/master/Angular%20application%20files/.travis.yml


### Set up Heroku:
go to https://dashboard.heroku.com/apps ->
create new pipepline for Angular application `your-repo-name` ->
add app in staging ->
Create new app... `your-repo-name-staging` ->
pres on the app name ->
Deploy ->
Connect to GitHub ->
Search for a repository to connect ->
Mark `"Wait for CI to pass before deploy"` ->
Enable automatic deploys ->
Go to settings, and add to Config Vars: ->
Key: `API_HOST`
Value: `your back end application url`
deploy branch

After deployment go to the app and you should see:
`Message from server: BE works DB works`
good job, first app deployed!!!

id you see :
`Message from server: BE works`
that means DB is not connected to BE, or DB is empty

if you see: 
`Message from server: server is down...`
that means FE could not reach BE, most probably:
- BE is dead, 
- sleeping (Heroku app goes to sleep after 30 minn inactive),
- you didnt entered API_HOST before deploying FE app



# Bonus:
- Create production apps in both pipeplines and connect them together!
- Set up branch permissions in Git (for your own good): Settings -> Branches -> [x] Require Pull Request -> [x] Require status checks to pass before merging and -> [x] Require pull request reviews before merging

