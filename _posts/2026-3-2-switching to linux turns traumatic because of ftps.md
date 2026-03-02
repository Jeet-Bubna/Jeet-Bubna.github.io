---
categories:
- linux
- rageposting
date: 2026-3-2
tags:
- linux
- windows
- ntfs
title: switching to linux turns traumatic because of ftps
---

So I wanted to switch over to Linux, you know, like a sane person. 

I couldn't have foreseen the absolute terror I was going to go through
for the next four hours.

Let me take you back to the starting.

# The Starting

I wanted to switch to Linux, so like any regular sane person, I made a
boot drive on my pendrive. After using some careful YouTube and ChatGPT
skills, I managed to create one --- pretty big of a feat for someone who
has not done this forever, however so it may be in my eyes.

Now, I wanted to partition some space for my Linux system. I had 89,
almost 90 GB remaining in my C drive.

"Why not just partition it here?" --- this was what led me spiraling
down a rabbit hole which made me realize just HOW annoying Windows is.

I went to partition the disk by "shrinking it" --- lo and behold --- the
great Microsoft gods had decided to grant me a measly 500 MB of
shrinkable disk space.

*What?* I whispered to myself, being baffled at such a sight. Seeing
(and more importantly not trusting) any YouTube videos with 3 views, I
decided to go to the monster which will inevitably steal this blog and
use it to make such blogs --- ChatGPT. Thanks, Sam Gptman.

# Trying to Disable Windows Files

Now, in my research, I found two files which I didn't know existed ---
`powercfg`, which contained the data for hibernation. No --- don't
confuse hibernation with sleep. They are two VERY different things.

Sleep is just your display shutting off, and your PC still being
relatively active. Hibernation, on the other hand, is your system
basically shutting down, except some important files are stored in your
disk so they can be retrieved easily. This is what allows your computer
to save a lot of time when you don't shut it down and let it just sit
there, and suddenly decide to open it.

The second file was `pagefile.sys`. This handles the paging for files.
From what I understand, these two files may be taking up space at the
end of the disk, so partitioning won't happen easily. This is because
partitioning works with uninterrupted free space.

After disabling them, I got --- lo and behold --- a whole 300 MB back.

# Almost Had Me Killed

So I tried to use GParted, because I thought this must be a problem with
Windows running and me trying to partition it --- maybe some system
runtime files are classified as immovable and would be removable with
Linux.

But before that, my naive mind got a jump scare.

I have never worked with boot drives before, and after I restarted my
computer, with my boot drive still plugged in, to implement the changes
after the file-disabling fiasco --- to my horror, the Linux boot menu
appeared on my screen. To my shock, I did not see the Windows option.

My mind began racing. I began questioning my existence.

Did I just erase my entire disk? Oh my mom's gonna kill me.

But what I found, to my relief, is that THAT IS SUPPOSED TO HAPPEN while
plugging in a boot disk. So after 10 minutes of terror and high
cortisol, I began to implement my plan.

When I tried to use GParted, I spectacularly spotted a crucial detail
--- the disk was BitLocker encrypted.

Now, I know nothing of the proceedings that would ensue after one
decides to partition a protected disk, but it must not be pleasant to
the user.

# Back to Windows *sighs*

I did the decryption. I sat there for 45 minutes.

Then I opened Disk Management, and what I found really moved me to the
core --- the whole 2--3 hour maneuver gave me A WHOPPING 6 MB extra.

At this point, I was fed up.

After some more research, I found out this was probably some NTFS
structure at the end, probably an MFT, which is causing all this.

So What Even Was That NTFS Thing?

After calming down (barely), I did some more research and found out this probably wasn’t Windows being randomly evil (cant trust billy too much tho). It was NTFS.

NTFS is basically the file system Windows uses to organize everything on your disk. It doesn’t just dump files wherever. It keeps track of every single file using something called the MFT — Master File Table.

Think of it like an index of a book. Every file has an entry there. Even tiny files. Even hidden system files. Even the ones you didn’t know existed and now kind of wish you still didn’t.

Here’s where it gets the blood boiling.

When you shrink a partition, Windows can only shrink it up to the point where the last “immovable” file is sitting. And NTFS sometimes places important system structures, like parts of the MFT, near the end of the partition.

So even though I had almost 90 GB “free”, that free space wasn’t one nice clean block. It was scattered. Fragmented. And somewhere near the very end of the disk was some tiny but extremely important structure that Windows absolutely refused to move.

So the shrink tool basically looked at me and said:
“Yeah no. 500 MB. Screw your money and your storage”

Disabling hibernation helped a little. Disabling paging helped a little. Decrypting BitLocker removed one more restriction. But that NTFS structure was still sitting there at the edge like a chad with 6 MB of torture -- that b*tch.

And that is how 3 hours of effort translated into 6 MB.

So this is how I found that Windows absolutely sucks.
