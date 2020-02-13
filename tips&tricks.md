# Setting up CI/CD (Swedbank IT Academy 2020)

## Examples:

- Angular: https://github.com/mantelisb/angular-demo
- SpringBoot: https://github.com/mantelisb/springboot-demo

## GitHub

1. Set up project on GitHub
2. Set up branch permissions (for your own good): Settings -> Branches -> [x] Require Pull Request -> [x] Require status checks to pass before merging and _optional_ [x] Require pull request reviews before merging

## Travis

1. Register on https://travis-ci.org, connect your GitHub account and add the repository.
2. Add `.travis.yml` to your repo.

## Heroku

1. Register to Heroku on https://heroku.com. You might need to add your credit card but don't worry - you shouldn't exceed the free tier limit.
2. Create a pipeline (https://devcenter.heroku.com/articles/pipelines#creating-pipelines).
3. Create your first app (https://devcenter.heroku.com/articles/pipelines#adding-apps-to-a-pipeline).

   - _Tip_. Name it something like _your-app-staging_ and add it to your staging pipeline step. You can add production app afterwards.
   - _Important._ Go to (App settings -> Deploy) and enable automatic deploys (probably) from master and check _Wait for CI to pass before deploy_. This way you are less failproof.
   - _Important #2_. Don't forget to add all the required environment variables (Config Vars) in your Heroku App settings. For the Angular app, the required environment variable is API_URL.
   - _TIP #2_. You can connect to different backends by changing API_URL. So your staging app might connect to staging backend and your production app might connect to production backend.

4. Create a production app. Follow the same guidelines. The only thing different is that you **don't want** automatic deploys and you **want** to use "_Promote to production_" mechanism when you have your staging application tested.

5. Enable review apps (https://devcenter.heroku.com/articles/github-integration-review-apps). You can enable them by adding `app.json` to your project manually or by pressing "_Enable Review Apps..._" and commiting the file straight to master.

## Your full workflow should look like this:

Write code -> Push to git -> Travis builds/tests/lints -> Check the review app -> Merge -> Check stage one more time -> Promote to production

## Tips

- Application Error on Heroku? Don't jump out of the window yet - go to More -> view logs and hunt down that problem.
- Want to set up new Travis/Heroku? Have you tried cleaning cache before?
- Use environment variables
- In git and GitHub, you can choose how you merge your commits. I usually go with _Squash and merge_ - this way I have clean commit history in my master branch. To enforce this behavior, you can go to _Repository Settings -> Merge button_ and leave only the type you wish to leave.
- Drink plenty of water
   
## Tips by students

- _Write here_
