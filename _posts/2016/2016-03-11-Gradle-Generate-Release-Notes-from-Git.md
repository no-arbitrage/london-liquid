---
title: "Gradle Generate Release Notes from Git"
date: "2016-03-11T17:30:52"
tags: [
  "android",
  "development",
  "scripting"
]
image: assets/images/gradle-generate-release-notes-AndroidStudio-300x168.png
---
I thought my ‘Gradle Generate Release Notes’ script might be useful for others, so here you go:

If you are using the built in [Gradle](http://gradle.org/getting-started-android/) tool for [Android Studio](http://developer.android.com/sdk/index.html) and you want to automagically generate release notes (or a list of commit messages) from Git then this little script might help (whilst it is pretty specific for [Android Studio](http://developer.android.com/sdk/index.html), it could easily be modified for any [Gradle](http://gradle.org)

![AndroidStudio](/assets/images/gradle-generate-release-notes-AndroidStudio-300x168.png)

I got the inspiration from [this post over at coders kitchen](http://coders-kitchen.com/2014/03/13/creating-beautiful-release-notes-with-git-gradle-and-markdown/) – but it seemed a little to complex for my needs, so I cut it right back to just text based, and just dates and commit messages between the tags. More than sufficient for my needs.

## Gradle Generate Release Notes Script

```gradle
println "Generating release notes (releaseNotes.txt)"
def releaseNotes = new File('app\build\outputs\apk\releaseNotes.txt')
releaseNotes.delete()
def lastTag = ""
def tags = []
def procTags = "git tag -l".execute()
procTags.in.eachLine { line -> tags += line }
tags += "head"

tags.each { tag ->
    releaseNotes << "[[ Changes from $lastTag to $tag ]]\n"
    def cmdLine = "git log $lastTag..$tag --pretty=format:\"%cd - %s\" --date=short"
    //releaseNotes << "$cmdLine\n\n"
    def procCommit = cmdLine.execute()
    procCommit.in.eachLine { line -> releaseNotes << line + "\n" }
    releaseNotes << "\n\n\n"
    lastTag = tag
}
```

It will create a file named ‘releaseNotes.txt’ in the build folder that has a list of the changes between each git ‘tag’.  
So, if you  add a git ‘tag’ for each release you do, you should be able to see the commit messages from the last release to the current one.

## Sample Output

Here’s a sample of the output from the Gradle Generate Release Notes from Git script.

```
[[ Changes from v1.1.1 to v1.1.2 ]]
2016-03-10 - Changes to release note generator
2016-03-10 - Better format for release notes

[[ Changes from v1.1.2 to head ]]
2016-03-10 - More changes to release note generator
```

I might look at changing the output to Markdown – should be pretty simple…