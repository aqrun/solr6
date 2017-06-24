# 3.3. Logging

When you click the link for "logging", a page similar to the one below will be displayed:

**Figure 1. The main logging Screen, including an example of an error due to a bad document sent by a client**

While this example shows logged messages for only one core, if you have multiple cores in a single instance, they will each be listed, with the level for each.

## Selecting a Logging Level

When you select the Level link on the left, you see the hierarchy of classpaths and classnames of your instance. A row highlighted in yellow indicates that the class has logging capabilities. Click on highlighted row and a menu will appear to allow you to change the log level for that class. Characters in boldface indicate that the class will not be affected by level changes to root.

**Figure2. Log level selection**

For an explanation of the various logging levels, see [Configuring logging]().