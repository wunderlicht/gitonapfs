# git on APFS
You got a new Mac and got it set up with the standard filesystem to get started quickly. What could go wrong some month down the line?\
After using your Mac for quite some time you have to work with your team on a piece of software. You clone its repository and... Oh, no!
```
❯ git clone git@github.com:wunderlicht/gitonapfs.git
Cloning into 'gitonapfs'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 15 (delta 2), reused 5 (delta 0), pack-reused 0
Receiving objects: 100% (15/15), 4.24 KiB | 4.24 MiB/s, done.
Resolving deltas: 100% (2/2), done.
warning: the following paths have collided (e.g. case-sensitive paths
on a case-insensitive filesystem) and only one from the same
colliding group is in the working tree:

  'tests/Test.txt'
  'tests/test.txt'
```

## Why does it happen?
In the repository you want to clone are files with the same name only distinguished by casing as shown in the following example.

```
test
Test
./tests/
./Tests/
```

Ok, but someone has put it there in the first place?\
Sure, some of your fellow developers is most likely on a Linux system with a **case sensitive** filesystem.


The standard filesystem for new Macs with SSDs is APFS. Unfortunately in the case **in**sensitive variant. That means 'file' and 'File' are the same.

## What now?
What are your options?
- Reformat your Mac. Yeah, right. You got work to do and can't spend half a day reformatting your Mac and restoring from a Time Machine backup
- Ask a fellow developer of your team to get rid of 'same' names in the repository. Obviously you can't do it yourself.
- or can you?

Yes, you can! The solution are sparcebundles (a type of image) with a case sensitive filesystem that you can mount into your existing filesystem and voila you're good to go.

### Create a suitable Image
```
hdiutil create -size 100M -fs "Case-sensitive APFS" -type SPARSEBUNDLE -volname devimage devimage
```
This creates a sparsebundle with 100MB initial size with a casesensitive APFS filesystem with a volume name 'devimage' in a new bundle directory 'devimage.sparcebundle'

### Mount/attach the Image
We need a mount point first.
```
mkdir ./repos
```

Now let's mount it.
```
hdiutil attach devimage.sparsebundle -mountpoint ./repos
```

### Clone a Repository into the attached Image
Change into the 
```
cd ./repos
git clone <repository>
```
Now, get some work done on your repository. Maybe rename the files to something more distinguishable, s. [Take away](#take-away) below 

Note: You cannot clone a repository directly into ./repos. Once the image is mounted some meta data of the image exist as a hidden directory. Hence ./repo isn't empty and git will complain.

### Unmount/detach the Image
Once your work is done, detach the image.
```
hdutil detach ./repos
```

## Take away
- When you're installing a new Mac consider to format your SSD with the APFS (Case-sensitive) filesystem (preferrably with its encrypted variant) or
- Use clearly distinguishable names in your codebase (the preffered option). 
