# Welcome to the Autolab Docs

Autolab is a course management and homework autograding platform that enables instructors to offer programming labs to their students. It includes gradebooks, rosters, handins/handouts, lab writeups, code annotation, manual grading, late penalties, grace days, cheat checking, meetings, partners, and bulk emails.

## Getting Started

For information on how to use Autolab go to the [instructors page](/instructors). To learn how to write an autograded lab go to the [lab authors page](/lab).

Autolab consists of two services, the Ruby on Rails frontend and [Tango](/tango), the Python grading server. In order to use all features of Autolab, we highly recommend installing both services.

Currently, we have support for installation on [Ubuntu 14.04+](#ubuntu-1404) and [Mac OSX](#mac-osx).

### Ubuntu 14.04+

The fastest way to install Autolab on Ubuntu is the OneClick option. This is recommended for running on external services like Heroku, EC2, or other Ubuntu VM providers. Find more information [here](/one-click).

#### Development Only

The following command runs a script that installs Autolab and all gems. You will be prompted for the `sudo` password and other confirmations. You can see the details of the script [here](https://github.com/autolab/Autolab/blob/master/bin/setup.sh).

```bash
AUTOLAB_SCRIPT=`mktemp` && \curl -sSL https://raw.githubusercontent.com/autolab/Autolab/master/bin/setup.sh > $AUTOLAB_SCRIPT && \bash $AUTOLAB_SCRIPT
```

Next, [install Tango](/tango), the RESTful autograding server, seperately.

### Mac OSX

Follow the step-by-step instructions below:

1. Install [rbenv](https://github.com/sstephenson/rbenv) (use the Basic GitHub Checkout method)

2. Install [ruby-build](https://github.com/sstephenson/ruby-build) as an rbenv plugin:
		
		:::bash
        git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
   Restart your shell at this point in order to start using your newly installed rbenv
        

3. Install the correct version of ruby:
        
        :::bash
        rbenv install $(cat .ruby-version)
   At this point, confirm that `rbenv` is working (you might need to restart your shell):
        
        :::bash
        $ which ruby
        ~/.rbenv/shims/ruby

        $ which rake
        ~/.rbenv/shims/rake

4. Install `bundler`:
        
        :::bash
        gem install bundler
        rbenv rehash

5. Clone the Autolab repo into any desired directory:
        
        :::bash
        git clone https://github.com/autolab/Autolab.git

6. Install the required gems (run the following commands in the cloned Autolab repo):

        :::bash
        cd bin
        bundle install
   Refer to the [FAQ](#faq) for issues installing gems

7. Install one of two database options

    * [SQLite](https://www.tutorialspoint.com/sqlite/sqlite_installation.htm) should **only** be used in development
    * [MySQL](https://dev.mysql.com/doc/refman/5.7/en/osx-installation-pkg.html) can be used in development or production

8. Configure your database:
      
        :::bash
        cp config/database.yml.template config/database.yml
   Edit `database.yml` with the correct credentials for your chosen database. Refer to the [FAQ](#faq) for any issues.

9. Configure the Devise Auth System with a unique key (run these commands exactly):

        :::bash
        cp config/nitializers/devise.rb.template config/initializers/devise.rb
        sed -i "s/<YOUR-SECRET-KEY>/`bundle exec rake secret`/g" initializers/devise.rb
   Fill in `<YOUR_WEBSITE>` in `config/initializers/devise.rb` file. To skip this step for now, fill with `foo.bar`.

10. Configure school/organization specific information (new feature):
        
        :::bash
        cp config/school.yml.template config/school.yml
    Edit `school.yml` with your school/organization specific names and emails

11. Create and initialize the database tables:

        :::bash
        bundle exec rake db:create
    Do not forget to use `bundle exec` in front of every rake/rails command.

12. Populate dummy data (development only):
        
        :::bash
        bundle exec rake autolab:populate

13. Start the rails server:

        :::bash
        bundle exec rails s -p 3000

14. Go to localhost:3000 and login with `Developer Login`:
      
        :::bash
        Email: "admin@foo.bar".

15. Install [Tango](/tango), the backend autograding service.

16. Now you are all set to start using Autolab! Visit the [instructors](/instructors) or [lab authors](/lab) pages for more info.


## FAQ

This is a general list of questions that we get often. If you find a solution to an issue not mentioned here,
please contact us at <autolab-dev@andrew.cmu.edu>

####Ubuntu Script Bugs
If you get the following error
```bash
Failed to fetch http://dl.google.com/linux/chrome/deb/dists/stable/Release  
Unable to find expected entry 'main/binary-i386/Packages' in Release file (Wrong sources.list entry or malformed file)
``` 
then follow the solution in [this post](http://askubuntu.com/questions/743814/unable-to-find-expected-entry-main-binary-i386-packages-chrome). 

####Where do I find the MySQL username and password?
If this is your first time logging into MySQL, your username is 'root'. You may also need to set the root password:

Start the server:
```bash
sudo /usr/local/mysql/support-files/mysql.server start
```

Set the password:
```bash
mysqladmin -u root password "[New_Password]"
```

If you lost your root password, refer to the [MySQL wiki](http://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html)

####Bundle Install Errors
This happens as gems get updated. These fixes are gem-specific, but two common ones are

`eventmachine`
```bash
bundle config build.eventmachine --with-cppflags=-I/usr/local/opt/openssl/include
```

`libv8`
```bash
bundle config build.libv8 --with-system-v8
```

Run `bundle install` again

If this still does not work, try exploring [this StackOverflow link](http://stackoverflow.com/questions/23536893/therubyracer-gemextbuilderror-error-failed-to-build-gem-native-extension)

####Can't connect to local MySQL server through socket
Make sure you've started the MySQL server and double-check the socket in `config/database.yml`

The default socket location is `/tmp/mysql.sock`.   

