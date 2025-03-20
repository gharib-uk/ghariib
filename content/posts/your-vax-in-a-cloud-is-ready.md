---
title: "Your VAX in a Cloud is Ready"
date: 2025-01-29
---

![](https://hackaday.com/wp-content/uploads/2025/01/vms.png?w=800)

For many people of a certain age, the DEC VAX was the first computer they ever used. They were everywhere, powerful for their day, and relatively affordable for schools and businesses. These minicomputers were smaller than the mainframes of their day, but bigger than what we think of as a computer today. So even if you could find an old one in working order, it would be a lot more trouble than refurbishing, say, an old Commodore 64. But if you want to play on a VAX, you might want to get a free membership on DECUServe, a service that will let you remotely access a VAX in all its glory.

The machine is set up as a system of conferences organized in notebooks. However, you do wind up at a perfectly fine VAX prompt (OpenVMS).

What can you do? Well, if you want a quick demo project, try editing a file called NEW.BAS (`EDIT NEW.BAS`). You may have to struggle a bit with the commands, but if you (from the web interface) click VKB, you’ll get a virtual keyboard that has a help button. One tip: if you start clicking on the fake keyboard, you’ll need to click the main screen to continue typing with your real keyboard.

Once you have a simple BASIC program, you can compile it (`BASIC NEW.BAS`). That won’t seem to do anything, but when you do a `DIR`, you’ll see some object files. (`LINK NEW`) will give you an executable and, finally, `RUN NEW` will pay off.

Some quick searches will reveal a lot more you can do, and, of course, there are also the conferences (not all of them are about VAX, either). Great fun! We think this is really connected to an Alpha machine running OpenVMS, although it could be an emulator. There are tons of emulators available in your browser.

Go to Source
