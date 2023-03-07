r## **What does MISP support?**   


MISP provides the following core functionalities:  

-   **IOC database:** This allows for the storage of technical and non-technical information about malware samples, incidents, attackers and intelligence.
-   **Automatic Correlation:** Identification of relationships between attributes and indicators from malware, attack campaigns or analysis.
-   **Data Sharing:** This allows for sharing of information using different models of distributions and among different MISP instances.
-   **Import & Export Features:** This allows the import and export of events in different formats to integrate other systems such as NIDS, HIDS, and OpenIOC.
-   **Event Graph:** Showcases the relationships between objects and attributes identified from events.
-   **API support:** Supports integration with own systems to fetch and export events and intelligence.


### to the room to practice.

-   [MISP Book](https://www.circl.lu/doc/misp/)
-   [MISP GitHub](https://github.com/MISP/)
-   [CIRCL MISP Training Module 1](https://www.youtube.com/watch?v=aM7czPsQyaI)
-   [CIRCL MISP Training Module 2](https://www.youtube.com/watch?v=Jqp8CVHtNVk)


```
SELECT "C:/" + FullPath AS Path_Path, FileName AS File_Name,parse_pe(file="C:/" + FullPath) AS PE
From parse_mft(filename="C:/$MFT", accessor="ntfs")
where NOT IsDir
AND FullPath =~"Windows/System32/spool/drivers"
AND PE

```

