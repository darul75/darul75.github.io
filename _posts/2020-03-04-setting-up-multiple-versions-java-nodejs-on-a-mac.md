---
layout: post
title: Setting up multiple versions of Java and NodeJS on a Mac
---

Some developers may need to use different versions of Java or NodeJS on their machine and below are some hints do get it working.

## homebrew

To start with here are 2 instructions to install and get homebrew up to date:

```javascript
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

brew update
```

installing cask extension

```javascript
brew tap caskroom/cask
```

allow brew to lookup versions

```javascript
brew tap homebrew/cask-versions
```

## java

List available java versions

```javascript
brew search java
```

To find out info

```javascript
brew cask info java8

brew cask info java => Java 13
```

Install Java 13

```javascript
brew cask install java
```

or 

```javascript
brew cask install java8 => java 8
```

## jenv

Tool to set JAVA_HOME 

```javascript
brew install jenv
```

Edit your ~/.bash_profile and add

```javascript
# To use Homebrew's directories rather than ~/.jenv add to your profile:

export JENV_ROOT=/usr/local/opt/jenv

# To enable shims and autocompletion add to your profile:

if which jenv > /dev/null; then eval "$(jenv init -)"; fi
```

List all your java versions

```javascript
/usr/libexec/java_home -V
```

should give you such output

```javascript
Matching Java Virtual Machines (3):
13.0.2, x86_64:     "OpenJDK 13.0.2"        /Library/Java/JavaVirtualMachines/openjdk-13.0.2.jdk/Contents/Home
11.0.6, x86_64:     "Java SE 11.0.6"        /Library/Java/JavaVirtualMachines/jdk-11.0.6.jdk/Contents/Home
1.8.0_231, x86_64:  "Java SE 8"     /Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk/Contents/Home
```

registering your java versions to [jenv](https://github.com/jenv/jenv)

```javascript
jenv add /Library/Java/JavaVirtualMachines/openjdk-13.0.2.jdk/Contents/Home
jenv add /Library/Java/JavaVirtualMachines/jdk-11.0.6.jdk/Contents/Home
jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_231.jdk/Contents/Home
```

List versions

```javascript
jenv versions
```

Switch to another version 

```javascript
jenv global 13.0.2
```

or locally

```javascript
jenv local 13.0.2
```

## nvm

```javascript
brew install nvm
```

edit your ~/.bash_profile and add

```javascript
export NVM_DIR="$HOME/.nvm"

[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm

[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

get list of available nodejs versions

```javascript
nvm ls-remote
```

or to filter only v10 versions

```javascript
nvm ls-remote | grep v10 
```

installing a specific version

```javascript
nvm install v10.18.0
```

seeing installed versions

```javascript
nvm list
```

using specific version

```javascript
nvm use v8.17.0
```

```javascript
mvn use system
```

## Conclusion

This type of setup should save you some time to work on different projects using different Java NodeJS versions, happy coding all!

## References

[installing multiple java versions with cask](https://medium.com/@brunofrascino/working-with-multiple-java-versions-in-macos-9a9c4f15615a)

[switching java versions with jenv](https://medium.com/@brunofrascino/working-with-multiple-java-versions-in-macos-9a9c4f15615a)


[nodejs version manager with nvm](https://github.com/nvm-sh/nvm)

 
----------

Tags: *Java* *NodeJS* *NVM* *jenv*
