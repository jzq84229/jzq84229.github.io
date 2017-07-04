Cannot change dependencies of configuration ':EmergencyTimeLine:classpath' after it has been resolved.

Update: Since Android Studio 2.2, new projects are created with this gitignore file:

```
*.iml
.gradle
/local.properties
/.idea/workspace.xml
/.idea/libraries
.DS_Store
/build
/captures
.externalNativeBuild
```

Verify if it suits your needs and if not, read on.

Previous:

A late answer but none of the answers here and here was right on the money for us...

So, here's our gitignore file:
```
#built application files
*.apk
*.ap_

# files for the dex VM
*.dex

# Java class files
*.class

# generated files
bin/
gen/

# Local configuration file (sdk path, etc)
local.properties

# Windows thumbnail db
Thumbs.db

# OSX files
.DS_Store

# Eclipse project files
.classpath
.project

# Android Studio
*.iml
.idea
#.idea/workspace.xml - remove # and delete .idea if it better suit your needs.
.gradle
build/

#NDK
obj/
```

Optional - for older project format, add this section to your gitignore file:

```
/*/out
/*/*/build
/*/*/production
*.iws
*.ipr
*~
*.swp
```

This file should be located in the project's root folder and not inside the project's module folder.

Edit Notes:

1. Since version 0.3+ it seems you can commit and push \*.iml and build.gradle files. If your project is based on Gradle: in the new open/import dialog, you should check the "use auto import" checkbox and mark the "use default gradle wrapper (recommended)" radio button. All paths are now relative as @George suggested.
2. Updated answer according to @128KB attached source and @Skela suggestions

[https://stackoverflow.com/questions/16736856/what-should-be-in-my-gitignore-for-an-android-studio-project](https://stackoverflow.com/questions/16736856/what-should-be-in-my-gitignore-for-an-android-studio-project)
