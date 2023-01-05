<h1> Practical Threat Modeling with MITRE ATT&CK </h1>

# Day 1 / Lab 1: Threat Modeling - Am I a Target?

Follow along using the MITRE ATT&CK Navigator https://mitre-attack.github.io/attack-navigator/enterprise/#

# Day 1 / Lab 2: Slicing and Dicing ATT&CK with MITRE Assistant

Mitre Assistant allows you to slice and dice the ATT&CK matrix quickly to get meaningful insights of what that matrix has to offer.

Mitre Assistant was created and is maintained by Carlos Diaz (@dfirence)

## Setup

1. Install Rust & Cargo: 

    https://doc.rust-lang.org/cargo/getting-started/installation.html

2. Install mitre assistant

    `cargo install mitre-assistant`

## Getting started & initializing Mitre-Assistant

3. You can find all the information on how to use mitre-assistant here: https://docs-ma.vercel.app/docs/project/Brief_Overview. 
    
4. Test that Mitre-Assistant is correctly installed:

    `mitre-assistant -h`

5. Download the ATT&CK Enterprise Matrix (notice that it does NOT support proxies)

    `mitre-assistant download -m enterprise`

6. Parse the downloaded matrix in JSON format so Mitre-Assistant can process it

    `mitre-assistant baseline -m enterprise`

7. Validate your setup with a simple query, searching for technique T1480

    `mitre-assistant search -m enterprise -t "t1480"`

## Statistics

Mitre Assistant is developed to be a batteries included utility for end-users who should only need to focus on asking questions, and finding answers from the ATT&CK dataset.

All queries that are prefixed with `stats:` support the way most professionals approach a dataset as they are exploring it to make sense of what the data may have to offer. When you are using Mitre Assistant, you are encouraged to use these queries as you are discovering ways to operationalize the ATT&CK matrix in your security program and support its defensive requirements.

8. Obtain stats by adversaries

    `mitre-assistant search -m enterprise -t "stats:adversaries"`

9. Obtain stats by malware

    `mitre-assistant search -m enterprise -t "stats:malware"`

10. Obtain stats by tools

    `mitre-assistant search -m enterprise -t "stats:tools"`

11. Obtain stats by platforms

    `mitre-assistant search -m enterprise -t "stats:platforms"`

12. Obtain stats by tactics

    `mitre-assistant search -m enterprise -t "stats:tactics"`

13. Obtain stats by data sources

    `mitre-assistant search -m enterprise -t "stats:datasources"`

14. Obtain stats by techniques

    `mitre-assistant search -m enterprise -t "stats:techniques"`

15. Obtain stats by sub-techniques 

    `mitre-assistant search -m enterprise -t "stats:subtechniques"`

## Searching queries

The Mitre Assistant cli uses the search subcommand. This subcommand mode is designed as a search engine. Users of ATT&CK who know the terminology of the matrix should just have to provide inputs (search terms), and get relevant answers they are seeking.

The subcommand accepts both single and multiple inputs with comma , to indicate their needs for searching are various types of input. 

16. Search for these terms

    All techniques for the windows platform

    `mitre-assistant search -m enterprise -t "windows"` 

    All techniques for persistence tactic

    `mitre-assistant search -m enterprise -t "persistence"`

    All techniques whose name contains the "Exploitation" keyword

    `mitre-assistant search -m enterprise -t "Exploitation"`

    All techniques whose datasource contains "wmi-creation"

    `mitre-assistant search -m enterprise -t "wmi-creation"`

    All techniques for the adversary "apt1"

    `mitre-assistant search -m enterprise -t "apt1"`

    All techniques for the malware "poisonivy"

    `mitre-assistant search -m enterprise -t "poisonivy"`


17. Profiling adversaries, malware and tools

    `mitre-assistant search -m enterprise -t "fin7"`

    The summary query result from above is very useful to get a high level understanding, you might have noticed the technique_id was provided, but you did not get the names of the techniques. So it would be annoying for you to do that analysis manually.

    To avoid this frustration the Mitre Assistant cli allows you to get these details by using the -c or --correlate switch parameter.

    The correlated details provide you a compact understanding of all of the previous items, plus the following:

    - The Tactic Name mapped to the Technique ID, and the Technique Name
    - The Technique Name mapped to its Data Sources
    - The Malware Name, and Tool Name mapped to the adversary and technique
    
    `mitre-assistant search -m enterprise -t "fin7" -c`   

    In cases where you want more fine-grained control over the adversary details, you can express semantics with an AND operator by using the colon : delimiter. This query answers the question: What are the techniques for FIN7 in the persistence Tactic?

    `mitre-assistant search -m enterprise -t "fin7:persistence" -c`

    You are not limited to one tactic with queries of adversaries focused on filters by tactics. You just need to use a : delimiter for each tactic.

    `mitre-assistant search -m enterprise -t "fin7:persistence:defense-evasion:exfiltration" -c`

    We can also leverage Mitre Assistant to give us the results for malware or tool entities by simply querying them by their name.

    `mitre-assistant search -m enterprise -t "griffon"`

    We can again use the `-c` or `--correlate` parameter like this:

    `mitre-assistant search -m enterprise -t "griffon" -c`

    Similar to filtering adversary objects by tactics, you can also filter techniques of malware and tool objects by tactic, like this:

    `mitre-assistant search -m enterprise -t "griffon:lateral-movement" -c`

    `mitre-assistant search -m enterprise -t "mimikatz:credential-dumping" -c`

## Exporting queries

Mitre Assistant can export both to CSV and JSON. The export functionality is provided by the -e or --export-to parameter switch.
    
Exporting to csv is done by -e csv, like this:

`mitre-assistant search -m enterprise -t "windows" -e csv` 

Exporting to CSV to a file of your choosing is done by adding the -f or --file switch, like this:

`mitre-assistant search -m enterprise -t "windows" -e csv -f the_windows_techniques.csv`

18. Export the statistics we produced earlier to CSV, open it on your spreadsheet application, and sort by fields like Platforms, Tactics, Techniques, Adversaries, Sub-techniques, Malware or Tools. For example:

    `mitre-assistant search -m enterprise -t "stats:adversaries" -e csv -f adversaries.csv`

    `mitre-assistant search -m enterprise -t "stats:malware" -e csv -f malware.csv`

    `mitre-assistant search -m enterprise -t "stats:tools" -e csv -f tools.csv`

    `mitre-assistant search -m enterprise -t "stats:platforms" -e csv -f platforms.csv`

    `mitre-assistant search -m enterprise -t "stats:tactics" -e csv -f tactics.csv`

    `mitre-assistant search -m enterprise -t "stats:datasources" -e csv -f datasources.csv`

    `mitre-assistant search -m enterprise -t "stats:techniques" -e csv -f techniques.csv`

    `mitre-assistant search -m enterprise -t "stats:subtechniques" -e csv -f subtechniques.csv`

19. Finally, aggregate the data for FIN6, FIN7, FIN8 and correlate their tactics and techniques with their malware and tools used (follow the weapons methodology). 

    `mitre-assistant search -m enterprise -t “fin6,fin7,fin8” -c -f fin6fin7fin8.csv`

At this time Mitre Assistant does not export in MITRE Navigator format. 

As you can see, Mitre Assistant helps you to slice and dice the ATT&CK matrix quickly to get meaningful insights of what that matrix has to offer.

# Day 2 / Lab 1 - Threat Modeling – Visibility & Coverage

DeTT&CT aims to assist blue teams in using ATT&CK to score and compare data log source quality, visibility coverage, detection coverage and threat actor behaviours.

The DeTT&CT framework consists of a Python tool, YAML administration files, the DeTT&CT Editor and scoring tables for the different aspects.

## Setup

1. Install docker for your platform
    - https://docs.docker.com/get-docker/

2. Get the latest image for DeTT&CT from Docker Hub
    `docker pull rabobankcdc/dettect:latest`

3. Execute the appropriate command to create the container and mount the input and output directories. These directories will be created in your current working directory.

- Linux and MacOS: 
    `docker run -p 8080:8080 -v $(pwd)/output:/opt/DeTTECT/output -v $(pwd)/input:/opt/DeTTECT/input --name dettect -it rabobankcdc/dettect:latest /bin/bash`

- Windows (cmd.exe): 
    `docker run -p 8080:8080 -v %cd%/output:/opt/DeTTECT/output -v %cd%/input:/opt/DeTTECT/input --name dettect -it rabobankcdc/dettect:latest /bin/bash`
    
- PowerShell: 
    `docker run -p 8080:8080 -v ${PWD}/output:/opt/DeTTECT/output -v ${PWD}/input:/opt/DeTTECT/input --name dettect -it rabobankcdc/dettect:latest /bin/bash`

- To start the container when it is no longer running (this should bring you straight back into the container with an interactive Bash shell)

    `docker start -i dettect`

## Using DeTT&CT Editor

1. Using the local editor 

    `python dettect.py editor`

2. Alternatively you can also use the online editor

    https://rabobank-cdc.github.io/dettect-editor/#/home

Don't forget to save the YAML files once edited.

## Data sources

Data sources are the raw logs or events generated by systems, security appliances, network devices, etc. ATT&CK has over 30 different data sources, which are further divided into over 90 data components. All of the data components are included in this framework.

For each data source, among others, the data quality can be scored. The scoring tables are available here: https://github.com/rabobank-cdc/DeTTECT/raw/master/scoring_table.xlsx

Within ATT&CK these data sources are listed within the techniques themselves. To see what data sources are available per platform, see https://github.com/rabobank-cdc/DeTTECT/wiki/Data-sources-per-platform

If you want to use the online editor, the sample datasource file is available here:
https://github.com/rabobank-cdc/DeTTECT/blob/master/sample-data/data-sources-endpoints.yaml

1. Generate your first JSON layer to import in ATT&CK Navigator based on the sample data sources file provided by DeTT&CT. This file represents the data sources available in your environment. This gives you a rough overview of your visibility coverage. Often, this is the first step in getting an overview of your actual visibility coverage.
       
    `python dettect.py ds -fd sample-data/data-sources-endpoints.yaml -l`

    The results of executing this command will be saved to the `output` directory both on your machine and inside the container. 

2. Import the JSON file into ATT&CK Navigator to see what techniques you would have visibility for based on those data sources 

    For layers files created by DeTT&CT, it is best to use this URL to the Navigator as it will make sure metadata in the layer file does not have a yellow underline: 
    
    https://mitre-attack.github.io/attack-navigator/#comment_underline=false

3. Try to create a new YAML file with only traditional network based data sources and see what techniques visibility would be available in your environment. Save it to your system and load it again on Navigator. 

4. You can build on this last layer to visualize the gaps between your visibility and the techniques used by threat groups of interest. Use the **selection controls** panel's **multi-select** control to select all of the techniques used by the FIN7 threat groups; then close the **multi-select** drop-down.

## Adjust Visibility & Detection

It is possible and recommended to adjust the visibility score per technique based on expert knowledge and the previously defined quality of your data sources. This will be required in cases when a single data source that we have available is not be sufficient for at least a minimum detection level for a specific technique. And hence, the visibility score based on the number of data sources needs to be adjusted.

To administrate your detection and visibility scores per ATT&CK technique, you can use this YAML file https://github.com/rabobank-cdc/DeTTECT/wiki/YAML-administration-techniques_v1_2

For details on how to adjust visibility and detection quality for each technique see https://github.com/rabobank-cdc/DeTTECT/wiki/How-to-use-the-framework#detection

We do not recommend doing this for every technique due to the administrative burden involved, but rather on a case by case basis.

5. Adjust visibility and detection assigning individual scores to each technique. The following command will generate a file of techniques that will only include those for which we have defined data sources previously. 

    `python dettect.py ds -fd sample-data/data-sources-endpoints.yaml --yaml`

6. Open up the file with the browser, using the local or the online editor:

    `python dettect.py editor`

7. Adjust the individual scores if needed and save the file. 

8. Generate a new JSON file for ATT&CK Navigator with your adjusted visibility coverage:

    `python dettect.py v -ft sample-data/techniques-administration-endpoints.yaml -l`

9. Do the same with detection coverage. This command will generate a new JSON file with your adjusted detection coverage:

    `python dettect.py d -ft sample-data/techniques-administration-endpoints.yaml -l`

10. Import the resulting JSON files into Navigator to see what techniques you would have visibility and detection for based on your adjusted input. 
    
    https://mitre-attack.github.io/attack-navigator/#comment_underline=false

## Threat Actor heatmaps

You can generate an ATT&CK Navigator layer file based on threat actor group data in ATT&CK. Or your threat actor data stored in a group YAML file.

11. The below-generated layer file contains a heat map based on all threat actor data within ATT&CK. The darker the color in the heat map, the more often the technique is being used among groups. Please note that, like all data, there is bias.

    `python dettect.py g`

12. Generate a JSON file with the heatmap corresponding to the groups we're interested in (FIN6, FIN7, FIN8) and import it into Navigator.

    `python dettect.py g -g 'fin7,fin8,fin6'`

## Visibility & Detection Gap Analysis (Purple Teaming)

You can compare a group YAML file with, for example, data on a red team exercise or a specific threat actor group with your detection or visibility. DeTT&CT can generate an ATT&CK Navigator layer file in which the differences are visually shown with a legend explaining the colors.

13. Generate a custom heatmap in a JSON file using the selected threat actor report from Red Canary and the sample YAML template for detection: 

    `python dettect.py g -g threat-actor-data/20210331-RedCanary.yaml -o sample-data/techniques-administration-endpoints.yaml -t detection`

14. Do the same for visibility

    `python dettect.py g -g threat-actor-data/20210331-RedCanary.yaml -o sample-data/techniques-administration-endpoints.yaml -t visibility`

# Day 2 / Lab 2 - Cyballistics – Follow the Weapons with VirusTotal 

Follow the instructions given in class to obtain your temporary Virustotal Intelligence license. Login to VT with your account, and using the advanced search modifiers in VT Enterprise, find relevant patterns or behaviors for:

- Microsoft Word documents (doc) that have macros, that execute files and that have Cyrillic language in the metadata.

- PHP files named gate.php that have been observed as part of network communications (useful to find samples that exhibit specific Command and Control behavior).

- Samples where wscript is observed executing .vbs files, through behavioral analysis (sandboxes)

- Google drive URLs with more than 20 detections.

- PCAPs tagged with exploit-kit.

- Samples that modify a specific registry key for persistence purposes.

- PDFs that contain JavScript and contains an automatic action (perhaps to launch the previous JavaScript).

We already identified that Hacme Cats needs to focus on increasing visibility and detection on Spearphishing Attachments, among others techniques. 
  
Spearphishing Attachments is a Sub-Technique of Technique T1566 in the MITRE ATT&CK Matrix, part of the Initial Access Tactic. 

Since we're studying Phishing, and more specifically Spear-Phishing techniques, we can ask the 'Google of malware' to research samples exhibiting malicious behaviors. A quick look at MITRE's description for this technique show us threat groups that are quite active at the time of this writing, and that have been involved with popular campaigns like Ryuk ransomware, Emotet, Trickbot or Bazar. We're talking about the Wizard Spider or UNC1878 group: https://attack.mitre.org/groups/G0102/

Notice how this group makes active use of Microsoft documents containing macros or PDFs containing malicious links to download either Emotet, Bokbot, TrickBot, or Bazar. 

Let's make this more actionable and try to expand our understanding of how attackers (any attacker, not just Wizard Spider) make use of these documents. In order to try to extract a study base of malicious PDFs from VirusTotal the first idea that comes to our minds is to use a search query as simple as:

```text
type:pdf positives:5+
```

This query returns PDF files that are detected by five antivirus solutions or more. Make sure you are signed in with your activated account, since this is a privileged functionality in VT.

We can learn more of these samples by selecting any of them. The tags added by VT gives us an indication of the behavior of these files. For example, we can observe that some of them make use of sandbox evasion techniques to defeat analysts and security products. Notice the `detect-debug-environment` tag. 

Compare the results of this search, with the results of the next one. This query searches for Excel spreadsheets (xls) that execute other processes and that have been reported by more than 10 antivirus engines:

```text
type:xls have:execution_parents p:10+
```

Notice how the tags give us more context on what these samples are doing. You can pivot and expand the results by clicking on any of those tags.

Let's do a similar search but this time for RTF documents that execute other processes and that have been reported by 10 AV engines or more:

```text
type:rtf have:execution_parents p:10+
```

Notice how again the tags give us more context on what these samples are doing. Not only we know they contain macros, as expected, but we can also see the vulnerability that's being exploited, in this case CVE-2017-11882. You can pivot and expand the results by clicking on any of those tags.

As you can see, despite being patched for three years already, attackers are still relying on this old remote code execution vulnerability in Microsoft Office to infect victims with malware. According to the threat analysis report from HP Bromium, the flaw accounts for nearly three-quarters of all exploits leveraged in Q4 2020. You can find more information on CVE-2017-11882 and associated mitigations here: https://socprime.com/blog/cve-2017-11882-two-decades-old-vulnerability-in-microsoft-office-still-actively-leveraged-for-malware-delivery/

We can now pivot and expand our search, to find any samples submitted to VT that have been tagged with this specific Common Vulnerability and Exposure (CVE) identifier and with the tag exploit:

```text
tag:cve-2017-11882 tag:exploit
```

VT Intelligence allows you to run very specific searches. Now take some time to 'fly solo' and experiment with additional searches like:

- Microsoft Word documents (doc) that have macros, that execute files and that have Cyrillic language in the metadata.

```text
type:doc p:10+ tag:macros tag:run-file metadata:Cyrillic
```

- PHP files named gate.php that have been observed as part of network communications (useful to find samples that exhibit specific Command and Control behavior).

```text  
behavior_network:"gate.php"
```

- Samples where wscript is observed executing .vbs files, through behavioral analysis (sandboxes)

```text  
behavior_files:"wscript.exe" behavior_files:".vbs"
```

- Google drive URLs with more than 20 detections:

```text
p:20+ itw:"drive.google.com" 
```  

- PCAPs tagged with exploit-kit:

```text
type:pcap tag:"exploit-kit" 
```  

- Samples that modify a specific registry key for persistence purposes:

```text
behavior_registry:"Software\Microsoft\Windows\CurrentVersion\Run"
```  

- PDFs that contain JavScript and contains an automatic action (perhaps to launch the previous JavaScript):

```text
type:pdf tag:autoaction tag:js-embedded
```  

For a full reference on VT file search modifiers check this reference:

https://support.virustotal.com/hc/en-us/articles/360001385897-File-search-modifiers


# Day 2 / Lab 3 - Cyballistics – Follow the Weapons with VirusTotal 

1. Use the Jupyter Notebook here

    https://github.com/aboutsecurity/jupyter-notebooks

2. Click on Launch BINDER 

    https://mybinder.org/v2/gh/aboutsecurity/jupyter-notebooks/HEAD

3. Open the JSON parser folder and click on the json-parser.ipynb (Jupyter Notebook)

4. Go to ATT&CK extractor and copy all the techniques obtained as a result of running the script

    https://d3fend.mitre.org/tools/attack-extractor/?q=%5B%5D

5. Examine the mapping results. Do you miss any defensive countermeasures?



