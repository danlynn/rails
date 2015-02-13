## Supported tags and respective `Dockerfile` links

+ [`3.2.21`,`latest` (3.2.21/Dockerfile)](https://github.com/danlynn/rails/blob/master/Dockerfile)

This image contains rails, ruby, and support for installation of most gems (including mysql and postgres drivers).  The container's working dir is /myapp so that you can setup a volume mapping your project dir to /myapp in the container.


## How to use - Easy Way

Clone the [ember-cli-rails-template](https://github.com/danlynn/ember-rails-docker-template) project from github which already has everything all setup to start using this image.  It also conveniently sets up aliases for the ember, bower, and npm commands so that you don't need to type `fig run --rm <command>`.  Instead, you can simply type the command just as if it was running locally instead of in a docker container.

## How to use - Hard Way

The harder way is to manually setup a project to use this container via [fig](http://www.fig.sh).

1. Create new project dir and add a fig.yml file similar the following:
   
   ```
   mysqlsvr:
     image: mysql:5.5
     ports:
       - "3306:3306"
     environment:
       - MYSQL_ROOT_PASSWORD=password
       - MYSQL_DATABASE=core_development
       - MYSQL_USER=developer
       - MYSQL_PASSWORD=password
   
   railssvr:
     image: danlynn/rails:3.2.21
     command: rails server -p 3000
     volumes:
       - .:/myapp
     ports:
       - "3000:3000"
     links:
       - mysqlsvr
   ```
   
2. Create a rails app in the current dir:

	```
	fig run --rm railssvr rails new . --database=mysql
	```
   
3. Start the rails server:
   
   ```
   $ fig up
   ```
   
   This launches the rails server on port 3000 in the docker container. As you make changes to the rails webapp files, they will automagically be detected and immediately reflected in the browser.

4. Launch the rails webapp:
   
   You will need to first determine the IP of the docker container:
   
   ```
   $ boot2docker ip
   
   192.168.59.103
   ```
   
   Next open that ip address in your browser on port 3000:
   
   + http://192.168.59.103:3000

## Command Usage

The rails, bundle, and rake commands can be executed in the container to effect changes to your local project dir as follows.  You basically put a "fig run railssvr --rm" in front of any of the 3 commands and pass the normal command options as usual.

Example:

```
fig run --rm railsvr bundle install
fig run --rm railsvr rails console
fig run --rm railsvr rake db:create
```
