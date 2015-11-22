# The brewber vagrant configuration steps

Add the brewber development url to your /etc/hosts file by:

    sudo vi /etc/hosts

Then append this to the end of the file:

    192.168.56.200  brewber.dev

Create a folder to hold the vagrant configuration in, then fetch the repo:

    git init

    git remote add origin https://github.com/john-sparwasser/brewber_vagrant.git

    git pull origin master

Create a link to your local brewber folder from within the vagrant folder by:

    ln -s /path/to/brewber brewber

Start the vagrant and begin the initialization:

    vagrant up

    vagrant ssh

    cd /var/www/brewber

    ./init

Select development for the init script, and don't replace any of the files if it asks.

Add this array to the common/config/main-local.php file so yii knows what the connection info is:

    'db' => [
        'class' => 'yii\db\Connection',
        'dsn' => 'mysql:host=localhost;dbname=brewber',
        'username' => 'brewber',
        'password' => 'password',
        'charset' => 'utf8',
    ],

Run migrations to initialize the database:

    php yii migrate

    php yii migrate --migrationPath=@yii/rbac/migrations

    php yii rbac/init

# Brewber deployment steps

Add these lines to your ~/.ssh/config file if they aren't already there:

    Host brewber-app1
    Hostname 54.84.152.118
    user ubuntu
    IdentityFile ~/.ssh/brewber_production.pem

Navigate to your local brewber project folder

    git remote add production ssh://brewber-app1/var/repo/brewber.git

When you want to deploy to production:

    git push production master
