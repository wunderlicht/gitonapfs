# git on APFS
You got a new Mac and got it set up with the standard filesystem to get started quickly. What could go wrong some month down the line?\
After using your Mac for quite some time you have to work with your team on a piece of software. You clone it and what do you get? Oh, no!

### Why does it happen?
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

### What now?
What are your options?
- Reformat your Mac. Yeah, right. You got work to do and can't spend half a day reformatting your Mac and restoring from a Time Machine backup
- Ask a fellow developer of your team to get rid of 'same' names in the repository. Obviously you can't do it yourself.
- or can you?

Yes, you can! The solution are sparceimages that you can mount into your existing filesystem and voilá you're good to go.


### Take away
- When you're installing a new Mac consider to format your SSD with the APFS (Case-sensitive) filesystem or (preferrably with its encrypted variant).
- Use clearly distinguishable names in your codebase. 
