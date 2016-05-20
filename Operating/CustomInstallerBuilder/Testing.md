# Testing the Custom Installer Builder with Django 1.6 Support
This page outlines how to test the [Custom Installer Builder](https://seattle.poly.edu/wiki/CustomInstallerBuilder) for basic correctness using Django's built-in test server on a Linux system. For production-level deployment on a real web server, reference [these instructions](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Deployment.md) instead.

We'll assume that you have completed the previous steps to [install](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Installation.md), [configure](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Configuration.md), and [customize](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/CustomizeAndBuild.md) your local Custom Installer Builder already.

----
## Revisit Django settings

Under the Custom Installer Builder's user account, edit `~/custominstallerbuilder/local/settings.py` to match your local configuration. Ensure that the following items are set up correctly:

```python
SERVE_STATIC = True
...
BASE_URL = 'http://your-actual-custominstallerbuilder-test-url:PORT/'  # Note the trailing slash!
...
PROJECT_URL = BASE_URL
```

If you are testing locally, you will use ```http://127.0.0.1:PORT/``` for the `BASE_URL`. You may use your public facing IP if you want it to be accessible via the Internet. `PORT` must be an open port number greater than 1024. For smaller port numbers, administrative privileges are required.


## Start Django test server
Ensure that the environment variable `PYTHONPATH` includes both the parent directory of your deployed `clearinghouse` and the Repy runtime directory, and `DJANGO_SETTINGS_MODULE` points to your local settings file:

```sh
$ export PYTHONPATH=$PYTHONPATH:/home/cib:/home/cib/repy_runtime
$ export DJANGO_SETTINGS_MODULE='custominstallerbuilder.local.settings'
```


Then, run the Django test server:

```sh
$ ./custominstallerbuilder/manage.py runserver 0.0.0.0:PORT   # This will log some information to the prompt
Validating models...

0 errors found.
Django version 1.3.5, using settings 'local.settings'
Development server is running at http://0.0.0.0:8080/
Quit the server with CONTROL-C.
```

You should now be able to access your Custom Installer Builder test server at the address specified for `BASE_URL` above. Don't worry if the media files (images, CSS, JavaScript) are missing for the time being. The [production deployment guide](https://github.com/SeattleTestbed/docs/blob/master/Operating/CustomInstallerBuilder/Deployment.md) will add the missing bits and pieces of configuration to rectify that.

Do keep in mind that the Django development web server is **not meant for production use**.