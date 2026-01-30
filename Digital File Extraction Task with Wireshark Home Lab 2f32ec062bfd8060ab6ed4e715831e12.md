# Digital File Extraction Task with Wireshark | Home Lab

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/4b9f7d7c-b540-4094-a0aa-8a569775e4b4" />


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

<img width="1917" height="942" alt="image" src="https://github.com/user-attachments/assets/92170358-2d56-4871-9a1f-a496369f6e10" />


As seen in the image, we have **5740 packets** to work with. I will perform this investigation based on the objectives given.

1. **This is a http traffic: filter the http traffic and the number of http packets.**

<img width="1663" height="863" alt="image" src="https://github.com/user-attachments/assets/e1422e71-5580-48f1-9f3d-2a6e5a638547" />

To filter the **http** traffic, we need to set the protocol name in the **display filter** tab. As shown in the image, objective 1:

- list only the **HTTP protocol**
- and finally, the number of http packets displayed as **26**

1. **Protocol discovery and port: find the protocol that advertise and discover network services and devices and identify the port used.**

With objective 2, after doing a search, I realized the protocol to be **SSDP (Simple Service Discovery Protocol)** which is designed to advertise and discover network services.

<img width="1510" height="872" alt="image" src="https://github.com/user-attachments/assets/75bdda49-e32d-4627-9eed-cb79d0733f1c" />

After applying the filter, **ssdp,** it displayed the protocol based on the filter set.

- Discovered protocol is **SSDP**
- Port identified is **1900**

1. **Try to pull out these images and files with wireshark: there are images and some different files in that traffic that when sent were captured.**

<img width="1084" height="578" alt="image" src="https://github.com/user-attachments/assets/56200c27-89e0-452a-b6ea-e4ae6dd67d2a" />

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

<img width="1639" height="768" alt="image" src="https://github.com/user-attachments/assets/293c717c-1c26-4b4f-acdd-7d4e9625dc29" />

To get the image in the first method, I viewed the HTTP stream by:

- Right-clicking a desired packet
- Select **Follow** from the pop-up menu
- from the toggled list, click on **HTTP Stream**

We could see the image was a **GET** request, meaning it was downloaded and other header information.

<img width="1346" height="797" alt="image" src="https://github.com/user-attachments/assets/97c4741a-6fc6-4ae4-8b31-90c07520d7e2" />

In order to view the hex format, I changed the **ASCII** option in the “**Show as”** drop down to **RAW,** this will give the hex data of the file.

<img width="1344" height="811" alt="image" src="https://github.com/user-attachments/assets/a801dae5-4850-4c44-b8d3-59a7f302a5da" />

After the change is applied, in the **“Find:”** tab, input the jpg hex signature, **ffd8** to indicate the start of the file.

After the first hex data is identified, click the **Next** button, to find the last part of the hex data.

> **NOTE: after identifying the first “ffd8” the next is to identify the “ffd9” portion for a complete file.**
> 

<img width="1285" height="513" alt="image" src="https://github.com/user-attachments/assets/481d7b44-201a-4915-9ac0-75f6f01a69b2" />

After identifying the whole data, copy all into our baking tool, “**CyberChef”** to decode it into the image.

<img width="1615" height="774" alt="image" src="https://github.com/user-attachments/assets/272da0e1-71c4-4ac3-b0e8-a53e3ede203b" />

In the decoding tool, CyberChef:

- paste the copied data into the **Input** field
- unde the **Operations field,**  search for **image**
- select **Render Image** and drag it to the middle field to cook the hex data
- then change the input format to **Hex** under the Render Image

after a successful move, the image was presented to as output.

### Method 2 - Exporting HTTP Objects

This is the simplest way to extract files from a packet capture all in seconds.

<img width="1526" height="872" alt="image" src="https://github.com/user-attachments/assets/170a6f03-2b96-4241-8976-85daafd910fb" />

Firstly, to do this, we first need the http filter, then:

- select **File** as shown in the image above
- from the drop down, select **Export Objects**
- select **HTTP** from the toggle box

<img width="867" height="603" alt="image" src="https://github.com/user-attachments/assets/b92843ad-51f3-49bf-a0da-fa5b32acf287" />

In the new window, you can choose to:

- apply **Content-Types**
- or add **Text Filter**

in this case, I am ignoring all and just save the results, click on **“Save All”.** This will prompt you to save to a destination folder or location.

<img width="717" height="403" alt="image" src="https://github.com/user-attachments/assets/23bb61db-c07e-4831-8f55-ffc590ae8dcb" />

Success has been achieved and can be seen in the destination folder. 

- **13 files (**including jpg, png, docx, txt and pdf**)** excluding the .pcapng file.

> NOTE: In our view, the **broken.png** file is actually broken.
> 

<img width="582" height="500" alt="image" src="https://github.com/user-attachments/assets/77056c6f-35ac-46f6-97a3-43a567ef18fb" />

After attempting to open it, it prompted me with a **“Loading ‘broken.png’ failed”.**

I decided to use the Method 1 approach, if it’s going to work.

<img width="826" height="826" alt="image" src="https://github.com/user-attachments/assets/4c6aae67-5422-43d0-a8d1-268a50dda97d" />

After checking and comparing the bytes, I used my alternate tool, AI (ChatGPT) to check and observe a few strings I copied to it. It indicates a **base64-encoded data.**

> NOTE: AI is also a tool, I use it often in my workflow.
> 

- Here I copied the whole base64 string to decode it with a different tool, https://codebeautify.org/base64-to-image-converter# - CodeBeautify.

<img width="1288" height="816" alt="image" src="https://github.com/user-attachments/assets/c519958c-c7bb-48f2-9c33-c6a2d50e75cb" />

After pasting, it successfully generated the broken.png image. This can also be done with CyberChef, as shown below.

<img width="1485" height="798" alt="image" src="https://github.com/user-attachments/assets/86cd66cf-e749-4980-bb04-757d6accb8e4" />

Using different tools in your analysis and investigation ensures complete visibility, accurate correlation of events, and effective incident response, since each tool reveals different aspects of a security threat.

## Final Checks

After a successful extraction, I decided to check all files, then I found something interesting, steg files and extension file format.

<img width="941" height="552" alt="image" src="https://github.com/user-attachments/assets/eef8648e-14b7-4ffb-88d1-c8f936291e36" />

Upon trying to check the files, I stumbled on a **hiddenmessage2.txt** file, which shows a different file format.

<img width="1398" height="748" alt="image" src="https://github.com/user-attachments/assets/74e2c3cd-c3cc-4dd3-9004-6ff9b6c221bb" />

By using the **file** command to check the type of file, it shows it’s a **jpeg** image.

To convert the file to a jpeg file, we can use the command:

```bash
mv  hiddenmessage2.txt <your_new_filename>.jpg
```

as shown in the above image to convert the **.txt file** to a .**jpg file.**

<img width="1016" height="204" alt="image" src="https://github.com/user-attachments/assets/106cc21d-0bd5-4191-8399-079808172dc8" />

Whiles doing a deep analysis, I realized the **“how-to-commit-crimes.docx file** was not also in its correct extension format.

This does not allow to view the original content of the file. By converting, **using the “mv” command,** I could read the actual content of the .txt file.

> NOTE: ASCII text files most commonly use the
> 
> 
> **.txt** extension, representing plain text data that can be read across different platforms.
> 

<img width="930" height="161" alt="image" src="https://github.com/user-attachments/assets/58ef3051-b227-42c6-ae30-ef2693847821" />

By using the **file** command against the **securepdf.pdf file,** I realized it is also a **zip file** instead of a pdf. 

By trying to unzip the zip file file, I was prompted with a password for the zip file to be extracted. Also it can be seen that in the zip file, there is a **rawpdf.pdf file.**

<img width="1027" height="105" alt="image" src="https://github.com/user-attachments/assets/ad728321-3b78-42c6-acc6-e5505235567b" />

With no password clue in mind, I decided to use the **strings command**, which prints the sequences of printable characters in files.

By running a full strings against the securepdf.pdf file, it printed a long list of unreadable strings, but gave me a clue of the password. In order of print only that line, I used the command:

```bash
strings securepdf.pdf | grep -i password
```

this command will filter and print out any line with the word **“password”** in it. Finally I was presented with the password as **“secure”.**

<img width="1170" height="225" alt="image" src="https://github.com/user-attachments/assets/cf76db86-2432-4974-bc1c-6f955cff69bf" />

 By using the password **secure** to unzip the file, we were presented with our **rawpdf.pdf file** in the list of files, as shown above.

Conducting deep analysis of files and digital artifacts is essential for understanding how data is structured, manipulated, or hidden by attackers. It enables accurate identification of malicious behavior through file signatures, metadata, and embedded artifacts that are not visible on the surface. This approach reinforces proper forensic methodology, ensuring evidence integrity, documentation accuracy, and reliable conclusions. By thinking like an attacker and validating findings methodically, analysts can make confident, defensible decisions during investigations.

## Conclusion

In this project, Wireshark was used to analyze a packet capture in a controlled home lab environment to understand network traffic behavior and identify suspicious activities. By applying protocol filters, stream analysis, and packet inspection techniques, relevant network events were successfully isolated and examined. This investigation improved visibility into how data moves across a network and how malicious or abnormal patterns can be detected.

Overall, the lab strengthened practical skills in traffic analysis, alert validation, and incident investigation, which are essential for real-world SOC operations.

## Thank you.
