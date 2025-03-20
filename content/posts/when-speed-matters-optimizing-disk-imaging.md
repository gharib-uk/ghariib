---
title: "When Speed Matters: Optimizing Disk Imaging"
date: 2025-01-15
categories: 
  - "general"
  - "hdd"
tags: 
  - "disk-image"
  - "osforensics"
  - "oxygen-forensics-detective"
  - "sdd"
---

![](https://blog.elcomsoft.com/wp-content/uploads/2024/09/disk-speed.jpg)

We recently shared an article about maximizing disk imaging speeds, which sparked a lot of feedback from our users and, surprisingly, from the developers of one of the disk imaging tools who quickly released an update addressing the issues we discovered in the initial test round. We did an additional test, and we’re ready to share further insights into the performance of disk imaging.

## Test Environment and Measuring Methodology

The hardware setup for the new tests remained largely unchanged from the previous test. We used the same ASUS Vivobook S16 S5606MA laptop (Intel Core Ultra 9 185H, 32GB RAM, Thunderbolt 4). However, this time, the storage device used was a powerful Samsung 860 PRO (512GB), which is miles ahead of the previously used test drive. Over 100GB of compressed data (a bunch of zip archives) was written to the drive for testing.

Key changes included:

1. **No write blocker**: we skipped using a write blocker, as it negatively impacted copy speed. Instead, we used a fast and reliable Startech adapter.
2. **Software adjustments**: Arsenal Image Mounter was dropped. While it has an impressive feature set, it is not designed for efficient disk imaging due to its slower speed and reduced flexibility.
3. **Output formats**: for practical reasons, this time we focused on just two common formats: RAW and EnCase E01 with medium compression.

We measured the performance of each tool by creating full disk images and manually recording the time, even though some of the tools can measure this automatically. The average speed in MB/s can be easily calculated based on the size of the storage device.

## The Contenders

We’ve tested a bunch of tools, mostly the same as in the previous test, with two exceptions. First, we dropped Arsenal Image Mounter as an obvious outsider. Second, we’ve added an updated version of OSForensics; more on that later.

#### OSForensics

In our previous tests, **OSForensics** by PassMark Software (Australia) was the fastest tool for saving RAW disk images, though it underperformed when creating images in the compressed E01 format. However, after our initial tests, the developers have made significant improvements based on our feedback. They optimized their compression algorithm, which resulted in a dramatic speed boost, especially for on-the-fly E01 compression. The developers provided us with a new tool and a free license (we sincerely appreciate that!) to test the updated version, which delivered impressive results.

The earlier version of OSForensics was found to dropping the imaging speed from 535MB/s to around 70MB/s when encountering a chunk of non-compressible data, while the CPU utilization fluctuated around the low 9%. The new version maintained a steady average speed of about 520MB/s, with pure compression speeds varying from 110MB/s (on already compressed data) to an incredible 3.5GB/s on empty disk sectors. Memory usage was around 700MB, and CPU load peaked at 40%.

![](https://blog.elcomsoft.com/wp-content/uploads/2024/09/osforensics.png)

Additionally, the new version of OSForensics introduced a feature allowing to specify the number of CPU threads used for compression when making an E01 image; the default is 16. You may want to change this number according to your CPU model.

#### FTK Imager

**FTK Imager**, which is popular in forensic circles, performed similarly to previous tests. Despite its popularity, we find the tool to be outdated, with user interface being simplistic in a bad way, offering limited functionality, and imaging slower speeds. CPU utilization ranged from 30-50%, and memory use was minimal (around 50MB).

#### X-Ways Imager

We also re-tested **X-Ways Imager**, a simplified version of X-Ways Forensics. While it is functional, X-Ways Imager and its evil twin, WinHex, have a steep learning curve, especially for inexperienced users. These products lack intuitive design and require significant effort to perform even basic tasks. Nonetheless, X-Ways Imager is very resource-efficient, consuming between 10-25% CPU and a few megabytes of RAM during imaging.

## Test Results

Now, to the test results.

![](https://blog.elcomsoft.com/wp-content/uploads/2024/09/speed.png)

As expected (and spoiled), OSForensics 11 led the pack in both raw and compressed disk imaging (RAW and E01 respecticaly). The newly provided version maintained high imaging speeds even when dealing with pre-compressed data, with only minor drops in performance. The compressed E01 image created by OSForensics was slightly larger than those made by FTK Imager and X-Ways (107GB vs. 104GB), but this came at the benefit of much higher imaging speed and correspondingly shorter imaging time. FTK Imager and X-Ways were way slower but achieved slightly better compression rates.

In the older version of OSForensics, OSF 10, the speed dropped significantly when handling non-compressible data, but this issue has been resolved. The new version managed to maintain high speeds even in these cases, thanks to the improved compression algorithm.

## Oxygen Forensic Detective: a Disappointing Newcomer

While preparing this article, we tested **Oxygen Forensic Detective**, a well-known software that recently added disk imaging functionality. Unfortunately, the results were disappointing. The software took nearly 30 minutes to create a raw image and almost an hour to create an E01 image, even with minimal compression. These speeds were significantly slower than the other tools tested, so we did not include it in our main comparison table.

![](https://blog.elcomsoft.com/wp-content/uploads/2024/09/keyscout.png)

Interestingly, the Russian twin of this program, distributed in Russia under the name **“Mobile Criminalist”** (developed by **MKO Systems**, formerly **Oxygen Software**), allegedly has similar functionality. However, we couldn’t find the disk imaging option at all. Despite identical version numbers and nearly identical interfaces, the Russian version simply lacks the button to create disk images. It’s hard to believe that these American-Cypriot and Russian products were developed by different teams.

![](https://blog.elcomsoft.com/wp-content/uploads/2024/09/scout.png)

### KeyScout and MK Scout: One More Thing

We also explored **KeyScout** from the same company (known as **“MK Scout”** in its Russian variant), which offers various functions, including disk imaging. It’s part of a larger forensic product suite and can be used independently without a license, so one can say it’s a free tool, with a caveat: the resulting disk image can only be opened by Oxygen Forensic Detective due to proprietary format.

KeyScout’s performance was subpar, showing signs of inefficient optimization. I have reasons to believe that the developers based their tool off dc3dd, a poorly optimized open-source product. As part of the imaging process, the tool produced a text report during the imaging process, which seemed to have been inspired (not to say “borrowed”) by FTK Imager’s reporting format.

![](https://blog.elcomsoft.com/wp-content/uploads/2024/09/reports.png)

## Why Speed Matters in Disk Imaging

Faster is better, but why do we care about disk imaging speeds at all? Speed is not a crucial factor in disk imaging, yet it certainly plays a role when imaginng larger disks or multiple storage devices. Imaging an SSD drive, which are rarely larger than 4TB, can take anywhere from one hour to several hours depending on the tool. When imaging magnetic hard drives, the stakes are even higher. A 16TB HDD with an average read speed of 200MB/s (more at the beginning, half the original speed close to an end) will take about 24 hours to image. A mere 10% difference in speed can save hours of work, making the choice of the right tool essential. For such cases, standalone imaging hardware might be a better solution.

Real-world speeds often differ from benchmarks due to a variety of factors, such as the state and, most importantly, content of the drive, making it essential to carefully select both software and hardware for imaging.

## OSForensics: Our Favorite Disk Imaging Tool

Given its performance, we’re considering starting an OSForensics fan club. The developers have put considerable effort into maximizing speed, and their willingness to share insights with us has been invaluable. They noted that while raw images are generally faster to create (due to the lack of compression), E01 is more practical in most cases due to reduced storage requirements without data loss.

![](https://blog.elcomsoft.com/wp-content/uploads/2024/09/osf2.png)

Their feedback is provided below with the permission of the company:

_Source drive: 512GB NVMe SSD  
Data on source drive: Windows boot drive + various office software installed  
Destination: USB-C 3.2 connected SSD.  
Format: E01 compressed format (medium compression). Single file (not split file)  
CPU: Ryzen 5600X (mid range desktop CPU)_

_Note that imaging empty (or partially empty) drives is much faster than drives with data as there is special handling of sectors which are all zeros. This is also why imaging speeds up and gets slower during the process._

_Imaging of data that is already compressed (or very random) is slower than data that can be compressed. SHA1 hashing is done at the same time as disk imaging. So avoids another pass at the end._

_Note that the final result of 421MB/s isn’t very fast compared to disk speeds. It is only fast in the context of writing a compressed E01 format file with hashing._

_If you have fast SSDs and plentiful disk space on the destination drive, raw image format will still be faster (between 50% and 100% faster). e.g. 7min instead of 12min for this example._

_We found that it was impossible to use all CPU capacity for E01 compression as RAM bandwidth presents a bottle neck during the compression process. So in additional to having fast SSDs, having fast RAM and at least a 4 Core CPU can help with imaging speeds._

_Compression rate was roughly 76% (so 465GB compressed to 111GB)_

## Why E01 and SATA?

Despite its age and limitations, **E01** remains the de facto standard for disk images due to its widespread support across forensic tools. The more modern AFF4 format offers numerous technical improvements but hasn’t been widely adopted. While raw images (RAW) are faster and require no compression, E01 is more space-efficient and practical for most users.

Although NVMe is becoming more common, **SATA** remains the most widely used interface for storage devices. SSDs provide consistent read speeds (though write speeds vary), making them a perfect source for testing. We plan to test NVMe drives in the future, as their faster speeds may offer even more interesting results. Memory card imaging is another area we aim to explore.

## Why (Not) FTK?

The popularity of FTK Imager remains somewhat puzzling. Its main advantage is that it’s free. While it’s relatively simple and convenient to use, offering basic disk imaging and some additional features, the real reason for its widespread use seems to be habit – one of those habits that can be hard to break.

We recommend using OSForensics for disk imaging if you have the budget (currently, that’s a subscription of $899 per user, per year). At least until a format like AFF4 becomes the industry standard, which will require significant effort from software vendors and have significant impact on the industry.

One key takeaway is that you shouldn’t trust all-in-one software solutions that claim to handle a wide range of tasks. Whether you like it or not, universal programs tend to perform worse than specialized ones. Yes, this means you’ll need multiple tools (for example, for working with already created images, we recommend **Arsenal Image Mounter**, which we consider best-in-class).

Returning to the issue of free software, it’s undeniably a great advantage, especially when it’s also open source. But as is often the case, advantages can quickly become drawbacks. With commercial software, you can request improvements and bug fixes from the developers, and statistically they are more likely to respond to your feedback. Talking to the makers of free software is a whole different story; it may actually cost you more to have _your_ bug fixed or _your_ feature implemented. Speaking of FTK Imager, the tool has simply stopped evolving, and despite being known for its lack of speed, there’s little hope for future updates.

## Conclusion: The Right Tool for the Job

Speed is critical in disk imaging, and the right tool can save hours of time, especially when working with large drives. While tools like FTK Imager are free and widely used, they lag behind specialized solutions like OSForensics in terms of speed and functionality.

Ultimately, we recommend OSForensics for those who can invest in a license. While free tools have their place, specialized tools provide better performance, flexibility, and support. As always, when choosing imaging software, we recommend prioritizing speed, reliability, and efficiency. I want to quote OSForensics once again: _So in conclusion. There are lots of variables. To get the best speeds, you need good SSDs, CPU, RAM and IO ports (e.g. USB3.2), plus the software to use them. Any one of these can end up being a bottleneck._

Go to Source
