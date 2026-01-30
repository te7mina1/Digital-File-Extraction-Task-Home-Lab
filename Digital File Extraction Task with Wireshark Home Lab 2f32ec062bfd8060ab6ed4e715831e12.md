# Digital File Extraction Task with Wireshark | Home Lab

![digital.jpeg](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/digital.jpeg)

## Overview

As part of a group of cybersecurity learners and enthusiasts, we were provided with a packet capture (.pcap) file, **Digital_Investigation Task (pcap file).pcapng,** to analyze based on specific objectives. As a SOC Level 1 learner, I chose to treat this exercise as a home lab project, since packet analysis is a core responsibility of a SOC analyst.

## Introduction

Packet capture (.pcap) is a fundamental skill in Cybersecurity and SOC environment. It involves analyzing network traffic to detect malicious and suspicious activities and helps to understand communication patterns between systems. For SOC analyst, packet analysis helps to detect attacks, investigate incidents, and understand abnormal network behavior.

## Objectives

We are to perform the following activities on the PCAP capture given:

1. This is a http traffic: filter the http traffic and the number of http packets.
2. Protocol discovery and port: find the protocol that advertise and discover network services and devices and identify the port used.
3. Try to pull out this images and files with wireshark: there are images and some different files in that traffic that when sent were captured.
4. Denote them using the right file signatures: using wireshark and the right file signatures denote and pull out those files and images that were captured. 

## List of files to extract (13)

anz-png.png, ANZ_Document.pdf .anz-logo.jpg, atm-image.jpg, bank-card.jpg, ANZ1.jpg, ANZ2.jpg, broken.png, how-to-commitcrimes.docx, ANZ_Document2.pdf, evil.pdf, hiddenmessage2.txt, securepdf.pdf

## Practical Demo

Opening the file content with Wireshark to start the investigation.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image.png)

As seen in the image, we have **5740 packets** to work with. I will perform this investigation based on the objectives given.

1. **This is a http traffic: filter the http traffic and the number of http packets.**

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%201.png)

To filter the **http** traffic, we need to set the protocol name in the **display filter** tab. As shown in the image, objective 1:

- list only the **HTTP protocol**
- and finally, the number of http packets displayed as **26**

1. **Protocol discovery and port: find the protocol that advertise and discover network services and devices and identify the port used.**

With objective 2, after doing a search, I realized the protocol to be **SSDP (Simple Service Discovery Protocol)** which is designed to advertise and discover network services.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%202.png)

After applying the filter, **ssdp,** it displayed the protocol based on the filter set.

- Discovered protocol is **SSDP**
- Port identified is **1900**

1. **Try to pull out these images and files with wireshark: there are images and some different files in that traffic that when sent were captured.**

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%203.png)

Going back to the http filter, the **Info** column displays some files in the format:

- **.png**
- **.pdf**
- **.txt**
- **.docx**
- **and .jpg**

these are the files in the traffic that was to be pulled out.

### Common File Signatures (Hexadecimal)

When analyzing raw packet data, you look for these signatures in the hex viewer to determine the file type:

- **JPEG:** `FF D8 FF E0` or `FF D8 FF E1`
- **PNG:** `89 50 4E 47 0D 0A 1A 0A`
- **GIF:** `47 49 46 38 39 61` (GIF89a)
- **PDF:** `25 50 44 46 2D` (%PDF)
- **ZIP/DOCX/JAR:** `50 4B 03 04`
- **EXE/DLL:** `4D 5A` (ASCII representation of MZ)

1. **Denote them using the right file signatures: using wireshark and the right file signatures denote and pull out those files and images that were captured.** 

Now, this process can be done in several methods, but I will show how it can be done in two (2) ways.

### Method 1 - Following the HTTP Stream

![Screenshot (173).png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/81870416-701e-454c-8f74-0dfa3f88fba4.png)

To get the image in the first method, I viewed the HTTP stream by:

- Right-clicking a desired packet
- Select **Follow** from the pop-up menu
- from the toggled list, click on **HTTP Stream**

We could see the image was a **GET** request, meaning it was downloaded and other header information.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%204.png)

In order to view the hex format, I changed the **ASCII** option in the “**Show as”** drop down to **RAW,** this will give the hex data of the file.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%205.png)

After the change is applied, in the **“Find:”** tab, input the jpg hex signature, **ffd8** to indicate the start of the file.

After the first hex data is identified, click the **Next** button, to find the last part of the hex data.

> **NOTE: after identifying the first “ffd8” the next is to identify the “ffd9” portion for a complete file.**
> 

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%206.png)

After identifying the whole data, copy all into our baking tool, “**CyberChef”** to decode it into the image.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%207.png)

In the decoding tool, CyberChef:

- paste the copied data into the **Input** field
- unde the **Operations field,**  search for **image**
- select **Render Image** and drag it to the middle field to cook the hex data
- then change the input format to **Hex** under the Render Image

after a successful move, the image was presented to as output.

### Method 2 - Exporting HTTP Objects

This is the simplest way to extract files from a packet capture all in seconds.

![Screenshot (174).png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/df8f9d03-8d96-45cc-a5eb-4238f28cde40.png)

Firstly, to do this, we first need the http filter, then:

- select **File** as shown in the image above
- from the drop down, select **Export Objects**
- select **HTTP** from the toggle box

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%208.png)

In the new window, you can choose to:

- apply **Content-Types**
- or add **Text Filter**

in this case, I am ignoring all and just save the results, click on **“Save All”.** This will prompt you to save to a destination folder or location.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%209.png)

Success has been achieved and can be seen in the destination folder. 

- **13 files (**including jpg, png, docx, txt and pdf**)** excluding the .pcapng file.

> NOTE: In our view, the **broken.png** file is actually broken.
> 

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2010.png)

After attempting to open it, it prompted me with a **“Loading ‘broken.png’ failed”.**

I decided to use the Method 1 approach, if it’s going to work.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2011.png)

After checking and comparing the bytes, I used my alternate tool, AI (ChatGPT) to check and observe a few strings I copied to it. It indicates a **base64-encoded data.**

> NOTE: AI is also a tool, I use it often in my workflow.
> 

- Here I copied the whole base64 string to decode it with a different tool, https://codebeautify.org/base64-to-image-converter# - CodeBeautify.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2012.png)

After pasting, it successfully generated the broken.png image. This can also be done with CyberChef, as shown below.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2013.png)

Using different tools in your analysis and investigation ensures complete visibility, accurate correlation of events, and effective incident response, since each tool reveals different aspects of a security threat.

## Final Checks

After a successful extraction, I decided to check all files, then I found something interesting, steg files and extension file format.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2014.png)

Upon trying to check the files, I stumbled on a **hiddenmessage2.txt** file, which shows a different file format.

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2015.png)

By using the **file** command to check the type of file, it shows it’s a **jpeg** image.

To convert the file to a jpeg file, we can use the command:

```bash
mv  hiddenmessage2.txt <your_new_filename>.jpg
```

as shown in the above image to convert the **.txt file** to a .**jpg file.**

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2016.png)

Whiles doing a deep analysis, I realized the **“how-to-commit-crimes.docx file** was not also in its correct extension format.

This does not allow to view the original content of the file. By converting, **using the “mv” command,** I could read the actual content of the .txt file.

> NOTE: ASCII text files most commonly use the
> 
> 
> **.txt** extension, representing plain text data that can be read across different platforms.
> 

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2017.png)

By using the **file** command against the **securepdf.pdf file,** I realized it is also a **zip file** instead of a pdf. 

By trying to unzip the zip file file, I was prompted with a password for the zip file to be extracted. Also it can be seen that in the zip file, there is a **rawpdf.pdf file.**

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2018.png)

With no password clue in mind, I decided to use the **strings command**, which prints the sequences of printable characters in files.

By running a full strings against the securepdf.pdf file, it printed a long list of unreadable strings, but gave me a clue of the password. In order of print only that line, I used the command:

```bash
strings securepdf.pdf | grep -i password
```

this command will filter and print out any line with the word **“password”** in it. Finally I was presented with the password as **“secure”.**

![image.png](Digital%20File%20Extraction%20Task%20with%20Wireshark%20Home%20L/image%2019.png)

 By using the password **secure** to unzip the file, we were presented with our **rawpdf.pdf file** in the list of files, as shown above.

Conducting deep analysis of files and digital artifacts is essential for understanding how data is structured, manipulated, or hidden by attackers. It enables accurate identification of malicious behavior through file signatures, metadata, and embedded artifacts that are not visible on the surface. This approach reinforces proper forensic methodology, ensuring evidence integrity, documentation accuracy, and reliable conclusions. By thinking like an attacker and validating findings methodically, analysts can make confident, defensible decisions during investigations.

## Conclusion

In this project, Wireshark was used to analyze a packet capture in a controlled home lab environment to understand network traffic behavior and identify suspicious activities. By applying protocol filters, stream analysis, and packet inspection techniques, relevant network events were successfully isolated and examined. This investigation improved visibility into how data moves across a network and how malicious or abnormal patterns can be detected.

Overall, the lab strengthened practical skills in traffic analysis, alert validation, and incident investigation, which are essential for real-world SOC operations.

## Thank you.