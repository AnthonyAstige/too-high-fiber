## Setup steps

1. `meteor create too-high-fiber`
1. `meteor npm install`
1. `meteor npm install --save fibers`

### What works

`meteor` Running the app with bundled meteor command appears to work just fine

### Reproducing the problem (Tested Meteor 1.3 & Meteor 1.3.1)

1. Start a brand new Ubuntu 14.04.4 Machine on Digital Ocean
1. `sudo apt-get update` Just to be safe
1. `sudo apt-get install -y git` Install git
1. `curl https://install.meteor.com/ | sh` Install meteor
1. Install & configure nvm
 1. `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash`
 1. (Reconnect for nvm access)
 1. `nvm install v0.10.44` Install v0.10.44 for Meteor / Fiber compatability
 1. `node -v ` Sanity check node version is v0.10.44
1. `git clone https://github.com/AnthonyAstige/too-high-fiber.git`
1. `cd ~/too-high-fiber && meteor build ~/built-with-fiber` Build
1. `cd ~/built-with-fiber && tar -xvf too-high-fiber.tar.gz` Decompress
1. `cd ~/built-with-fiber/bundle/programs/server && npm install` Prepare **[THIS STEP FAILS AS SHOWN](https://raw.githubusercontent.com/AnthonyAstige/too-high-fiber/master/fiber-fail-meteor1.3.1.png)**
1. `cd ~/built-with-fiber/bundle && PORT=80 ROOT_URL=http://my-app.com node main.js` Run **ONLY WORKS AFTER WORK-AROUND**

#### Working around the problem

`cd ~/built-with-fiber/bundle/programs/server && npm install fibers@1.0.7`

#### What also doesn't work

Changing dependency in package.json to `"fibers": "1.0.7"` it looks like node still tries to install fibers 1.0.8

#### Thoughts

Fibers was upgraded to 1.0.8 noted @ https://github.com/meteor/meteor/issues/5788#issuecomment-180643955
