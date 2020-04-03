---
layout: post
title: Note for Maven
categories: [faq]
tags: [maven, note]
---

### Tag `<parent>` in pom.xml
Maven reads the parent POM from your local repository (or proxies like Nexus) and create an 'effective POM' by merging the informcation from parent and module POM
### `mvn package`
Will compile your code and also package it. For example, if your pom says the project is a jar, it will create a jar for you when you package it and put it in somewhere in the target directory (by default)
### `mvn install`
Will compile and package, but it also put the package in your local repository. This will make it so other projects can refer to it and grab it from your local repository.
