

## Table of Contents

1)[ Coreferencing ](#desc1) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a. [ Description ](#desc1.1) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;b. [Run the Pipeline](#desc1.2) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Requirements <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Details <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- The Scripts <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Run the script <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c. [Running raw text files as input for the pipeline](#desc1.3) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Use the scripts included in this repository<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- How to write your own script file to run raw texts <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;d. [BRAT](#desc1.4) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Standoff Annotation Format <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Setting up the brat standalone server<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Using the brat standalone server to visualize .ann files<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Using the brat standalone server to annotate manually<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring brat<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;e. [Edit the Pipeline](#desc1.5) <br>


2)[ NER ](#desc2) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a. [ PubTator Central](#desc2.1) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;b. [Pubtator API for processing raw text](#desc2.2) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c. [BioC](#desc2.3) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;d. [MetaMap Lite](#desc2.4) <br>
    

3)[ Graphviz](#desc3) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;a. [ Running](#desc3.1) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;b. [ Dot Language ](#desc3.2) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;c. [Running Graphviz](#desc3.3) <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;d. [Creating the Interactive Webpage](#desc3.3) <br>

<a name="desc1"></a>
# 1) Coreferencing
<a name="desc1.1"></a>
## a. Description
This pipeline performs general coreference resolution for biomedical text. It incorporates a variety of coreference types (anaphora, appositive, etc.) and their textual expressions (definite noun phrases, possessive pronouns, etc.).

The paper looks at three domains:
* spl(corpus of structured drug labels annotated with fine-grained coreference information): used for development and evaluation
* i2b2(i2b2/VA discharge summaries): used only for evaluation
* bionlp(protein coreference dataset): used only for evaluation

Read [Paper](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0148538)

Download [Code](https://github.com/kilicogluh/Bio-SCoRes)
<a name="desc1.2"></a>
## b. Run The Pipeline
### Requirements
* Java (jar files have been generated with 1.8, though it is possible to recompile with 1.7)
* Ant (needed for recompilation, version 1.8 was used for compilation)

### Details
**Input:**

* all input files must be in XML format(.xml)

**Output:**
* the output is generated in [Standoff Annotation Format](https://brat.nlplab.org/standoff.html)(.ann)

### The Scripts

#### Details
The package above contains 7 scripts, and can be found under the folder **bin**. They do not require any arguments and point to existing input and output directories under the folder **DATA**.  Running them will demonstrate the capabilities of Bio-SCoRes.

* **bionlp:** this script only runs anaphora/appositive resolution on [BioNLP'11 Shared Task coreference dataset.](http://2011.bionlp-st.org/home) This pipeline consists of mention detection and mention-referent linking.
  
  Input folder= DATA/BIONLP/XML_DEV
  Output folder= DATA/BIONLP/OUT_DEV

* **splGoldMentions:** this script runs the coreference resolution on structured drug label (SPL) dataset with gold coreferential mentions. Only mention-referent linking is performed
  
  Input folder = DATA/SPL/TEST/XML
  Output folder = DATA/SPL/TEST/OUT_GOLD_EXP
  
* **splGoldEntities:** this script  runs the coreference resolution on structured drug label (SPL) dataset with gold named entities. Both coreferential mention detection and mention-referent linking is performed.

  Input folder: DATA/SPL/TEST/XML 
  Output folder: DATA/SPL/TEST/OUT_GOLD_ENTITY

* **bionlpEvaluate,splGoldEntitiesEvaluate, splGoldMentionsEvaluate:** The evaluation scripts evaluate the output generated by the 3 scripts above. (For example, splGoldMentionsEvaluate script evaluates the results generated using splGoldMentions.) The evaluation script will calculate precision, recall, F1 score. In addition, true positives, false positives and negatives can be printed out. Any evaluation script you generate can be compared to already processed evauation outputs stored in **DATA/Evaluation** to make sure they match(for ensuring the integrity of the distribution).

* **genericPipeLine:** this script is for running a generic pipeline. This script differs from the ones above for input, as text files are used rather than XML files. Input to this script is an input directory with text files and corresponding term annotations in brat standoff format and an output directory, where the results are written in the same standoff format. Note that to be effective, the term-related domain knowledge should have been introduced to the system as properties.

  Input folder: chosen at command line, must contain the text file(.txt) and corresponding term annotations in brat standoff format(.a1 and .a2)
  Output folder: chosen at command line

### Run the script
After downloading the package [here](https://github.com/kilicogluh/Bio-SCoRes), in termnal cd to directory where the package is located.
To run a script, simply type the following:
```
./bin/[script_name]
```
For example, to run the bionlp script
```
./bin/bionlp
```
<a name="desc1.3"></a>
## c. Running raw text files as input for the pipeline
### Use the scripts included in this repository
1) Download the **stanford-corenlp-3.3.1-models.jar** file included in this repository, and place it in the **lib** folder found in the BioScores package
2) Download the scripts included in this repository named **runRawText** folder
3) Place scripts in the same repository as the other scripts(bin)
4) Create a folder called "input" inside **DATA**.  In this folder include all text files you want to process. For each file you want to process, create two additional text files named the same as the original text file, but with ".a1" and ".a2". These files do not need to contain any content. A sample input file has ben provided for this article's method section. 
```
sample.text
sample.a1
sample.a2
```

5) To run the scripts, use the same convention above from the location ~/Bio-SCoRes-master
```
./bin/[script_name]
```
6) The output for each script can be found in **DATA/RawTextOutput**
 
The scripts make use of the XML writer java files provided in the source
code, can be found in the following folders:
   * to process raw text through bionlp pipeline: **src/tasks/coref/bionlp/BioNLPCorefXMLWriter.java**
   * to process raw text through spl pipeline: **src/tasks/coref/spl/SPLToXMLWriter.java**
   * to process raw text through i2b2 pipeline: **src/tasks/coref/i2b2/I2B2ToXMLWriter.java**

### How to write your own script file to run raw texts
You can edit input and output folders and their location by writing your own scripts.  Simply copy one of the provided scripts and edit the folloring locations for input and output folder. Can use any of the XMLWriters listed above for the first arguement.
```
#!/bin/bash

export CLASSPATH=""
for file in `ls dist`
do
  export CLASSPATH=$CLASSPATH:dist/$file
done
for file in `ls lib`
do
  export CLASSPATH=$CLASSPATH:lib/$file
done

java -Xmx1G -Djava.util.logging.config.file=logging.properties tasks.coref....[XML_WRITER] [INPUT_FOLDER] [OUTPUT_FOLDER]
```
<a name="desc1.4"></a>
## d. BRAT
### Standoff Annotation Format
The Bio-Scores pipeline produces .ann files, which are in Standoff Annotation Format. Here is an example of a typical .ann file:
```
R3	Coref Anaphora:T17 Antecedent:T35
R4	Coref Anaphora:T24 Antecedent:T36
T1	Protein 87 92	c-Fos
T2	Protein 455 458	PKC
T3	Protein 599 604	c-Fos
T4	Protein 609 612	Jun
T5	Protein 792 797	c-Fos
T6	Protein 802 805	Jun
T7	Protein 970 975	c-Fos
T8	Protein 990 993	Jun
T9	Protein 1165 1168	PKC
T10	Protein 1271 1274	PKC
T35	Exp 405 436	phorbol 12-myristate 13-acetate
```
You can read about it how to interpret these files [here](https://brat.nlplab.org/standoff.html#:~:text=Annotations%20created%20in%20brat%20are,never%20modified%20by%20the%20tool.&text=Within%20the%20document%2C%20individual%20annotations,of%20text%20through%20character%20offsets.).

These files can be interpreted by BRAT, which is a visulization tool for annotations.  BRAT takes the .ann files outputted in the pipeline and produces a visual representation. 

### Setting up the brat standalone server
1) Download brat [here](https://brat.nlplab.org/index.html).
2) Extract the zip file and cd into the created brat directory
```
cd brat-v1.3_Crunchy_Frog
```
3) Run the installation script and follow the instructions. You will be asked for a username, password and admin email. You will need these later on to log into the standalone brat server.
```
./install.sh
```

### Using the brat standalone server to visualize .ann files
1) brat requires the .txt file and .ann file for each annotation.  As well as 4 **.config** files which control the type, colors, and normalization.  Download the folders rhetorical_moves and coreference and place them under **~/brat-v1.3_Crunchy_Frog/data**
2) From **~/brat-v1.3_Crunchy_Frog**, start the standalone server:
```
python standalone.py
```
3) Follow the address generated through a browser and open the corresponding file in **data**

4) To add any additional files for visualization, brat requires both a raw text file and a .ann file, both with identical names.

5) CTRL-c in terminal to stop the server

### Using the brat standalone server to annotate manually
brat can also be used to annotate text manually
1) To edit in the browser, make sure to login with the username and password from when the server was created.  Otherwise editing abilities
Upload text raw text into the data folder
2) Annotations are straight forward and can be acheived with click and dragging over the text. You can follow a more thorough tutorial about annotating using brat [here](http://weaver.nlplab.org/~brat/demo/latest/index.xhtml#/.tutorials/.-1943678360/news/000-introduction). Additianlly, the manual can be found [here](https://brat.nlplab.org/manual.html).

note: As you annotate, brat will automatically create .ann files automatically(like the ones generated in the pipeline).  These files can be really useful for processing annotations through simple text programs, for purposes of identifying patterns and relationships.

### Configuring brat
Configuration files allow the user to specify the annotations, relationships attributes and event types for your annotations and should be included with the data, within the same folder. If no configuration is provided, the brat server will look for the configuration in the parent folder.  If still no config file is found, the default will be used. 

There are 4 config files:
* **annotantion.conf:** specify entitity, relation and event types as well as their attributes
* **visual.conf:** specify color schemes, font, text-size, and othe visual style elements.
* **tools.conf:** add normalization databases, specify search restrictions, and add built in annotators
* **kb_shortcuts.conf:** specify keyboard shortcuts

For more information on how to create, edit or interpret the existing configuration files, read the [following](https://brat.nlplab.org/configuration.html).

## e. Edit the Pipeline
For a breakdown of the pipeline, and how to build configurations, there is a [google doc](https://docs.google.com/document/d/1M9EJd6a5ILfhRh84E8joikZ4SX4YjdcgtJ86fjLq1Xc/edit?usp=sharing) availaible, where I have summarized the key parts of the pipeline and how to edit the code.

For a better understanding of the structure of the code in the pipeline, take a look at the [UML diagram](https://app.diagrams.net/#G1RE_8oV_YLIL5-iApDI-j_2tjE0FMZ7h1) I've created.

<a name="desc2"></a>
# 2) NER(Named Entity Recognition)
<a name="desc2.1"></a>
## a. PubTator Central
PubTator Central (PTC) is a Web-based system, which provides automatic annotations of biomedical concepts such as genes and mutations. It is powered by TaggerOne, tmVar, GNormPlus, SR4GN and SimConcept.

Annotations are only availaible for **PubMed abstracts** and **PMC full-text articles**.  The article we are looking at is not availaible in the PMC database, only Pubmed.  Thus, only the abstract can be annotated through pubmed. 
 
To find pre-annotated NER for any abstract from pubmed, follow the link below:
 ```
https://www.ncbi.nlm.nih.gov/research/pubtator/index.html?view=docsum&query=[PMID pr PMC ID]
```
or enter ID from this Pubtar [search engine](https://www.ncbi.nlm.nih.gov/research/pubtator/index.html)

<a name="desc2.2"></a>
## b. Pubtator API for processing raw text
To process any raw text, input must be converted to [BioC](http://bioc.sourceforge.net/), pubtator or JSON formats. There is a section below, which provides the APIs to retrieve the  BioC format for an article. Unfortunately, BioC formats are also only availaible for articles in the PMC and Pubmed databases.

After retrieving the BioC format of the article you want to process, use the following steps from terminal:

First submit a request with your file to annotate, make sure you are in the same directory where the file is located:
 ```
curl -X POST --data-binary @[Inputfile] https://www.ncbi.nlm.nih.gov/research/pubtator-api/annotations/annotate/submit/[Bioconcept]
```
A session number will be returned to you in the format of XXXX-XXXX-XXXX-XXXX.

For example, to process the file sample.xml with the bioconcept "Chemical":
 ```
curl -X POST --data-binary @sample.xml https://www.ncbi.nlm.nih.gov/research/pubtator-api/annotations/annotate/submit/Chemical
```

Retrieve the annotated file, by submitting the session ID for your job:
```
curl https://www.ncbi.nlm.nih.gov/research/pubtator-api/annotations/annotate/retrieve/[SessionNumber]
```
Where [SessionNumber] is the number previoulsy returned by your submitted request. e.g., 1441-7295-7121-9907
When submitting this request, the system will return a warning message : [Warning] : The Result is not ready" if the processing is not complete yet.


For more information about formats, visit [here](https://www.ncbi.nlm.nih.gov/research/pubtator/api.html)
<a name="desc2.3"></a>
## c. BioC

[API](https://www.ncbi.nlm.nih.gov/research/bionlp/APIs/BioC-PMC/) for retrieving  PMC abstracts in BioC format:
 ```
 https://www.ncbi.nlm.nih.gov/research/bionlp/RESTful/pmcoa.cgi/BioC_[format]/[ID]/[encoding]
```
[API](https://www.ncbi.nlm.nih.gov/research/bionlp/APIs/BioC-PubMed/) for retrieving  Pubmed articles in BioC format:
 ```
 https://www.ncbi.nlm.nih.gov/research/bionlp/RESTful/pubmed.cgi/BioC_[format]/[PMID]/[encoding]
```

<a name="desc2.4"></a>
## d. MetaMap Lite
Follow this [link](https://metamap.nlm.nih.gov/MetaMapLite.shtml) to access Metamap and select "MetaMap Lite" under **Run Locally**. Metamap requires a username and password for access, can be retrieved from Dr. Mercer.

Input: raw text file

Output: many options, choose brat for standoff annotation format (can pass through brat server to get a visualization of the annotations)


<a name="desc3"></a>
# 1) Graphviz
<a name="desc3.1"></a>
## a. Description and Resources
Graphviz was used to recreate the graph of claim-claim and figure-claim Connections found in " Dimerization Interactions of the b Subunit of the E.Coli ATPase".<br>
Webpage: https://fahreen.github.io/graphvizProject/
<a name="desc3.2"></a>
## b. Dot Language
Graphviz files are written in DOT.  For a simple introduction, refer to this webpage. https://graphviz.org/doc/info/lang.html

<a name="desc3.3"></a>
## c. Running Graphviz
Graphviz is a simple language which can be used to generate graphs which are structured to be more readable through Graphviz's Algorithm.
Many input and output  formats are accepted.  You can read about them here. <br> https://graphviz.org/doc/info/output.html

To download graphviz, for MAC or WINDOWS, you can find the instructions here: https://graphviz.org/download/ <br>

One way of running is specified below.  Many other examples can be found here. https://graphviz.org/doc/info/command.html
 ```
neato -Tsvg input.gv -o output.svg
```
<a name="desc3.4"></a>
## d. Creating the Interactive Webpage
1) Create the graph as an SVG, using neato.
 ```
 neato -Tsvg input.gv -o output.svg
```
2) Insert the svg wiithin an HTML page, by copy pasting everything within the <svg> tag.
3) Inside the HTML file, each node will have a structure similar to the following:
    
     ```
     <!-- 15A -->
     <g id="node1" class="node">
     <title>15A</title>
     <ellipse fill="lightskyblue" stroke="lightskyblue" cx="231.14" cy="-642.29" rx="27" ry="18"/>
     <text text-anchor="middle" x="231.14" y="-638.59" font-family="Times,serif" font-size="14.00">15A</text>
     </g>```

The id can be used in JS functions to target node colours.  For example the following function changes the node outline when the corresponding conclusion is clicked on the conclusion menu bar on the webpage:

 ```
 //15A
function fifteenA(){
  var x = document.getElementById("15A");
  var color = x.style.fill;

  if (x.style.stroke === "yellow"){
    x.style.stroke = color ;
    var k =1;
    var m = 1;
    while (k < 61){
        document.getElementById("node"+ k).style.opacity = "1";
        k++;
    }
    while (m < 85){
        document.getElementById("edge"+ m).style.opacity = "1";
        m++;
  }
  }

    else{
    x.style.stroke = "yellow";
      var i = 1;
      var j =1;
      while (i < 61){
        if (i !== 1 && i!== 25){
          document.getElementById("node"+ i).style.opacity = "0.5";}
        i++;
      }

      while (j < 85){
          if (j !== 42){
            document.getElementById("edge"+ j).style.opacity = "0.25";}
        j++;
      }
  }

  var a1 = document.getElementById("fig3_15Aline");
  var a2 = document.getElementById("fig3_15Aarr");


  if (a1.style.stroke === "red"){
    a1.style.stroke = "black";
    a2.style.stroke = "black";
    a2.style.fill = "black";
  }
  else{
    a1.style.stroke = "red";
    a2.style.stroke = "red";
    a2.style.fill = "red";
}
}
```

4) To link a highlighted PDF, name a PDF file based on each conclusion. Then place it in the Directory "PDF". Highlight the conclusion and premises using the simple highlighting tool in the PDF preview tool on mac. Add notes to the corresponding highlights. Do not press save, this leads to the notes being lost.  Simply close the PDFs after highlighting.  Following this include the following line within the node <g> tag
    
```
onclick="window.open('PDF/15A.pdf', '_blank', 'fullscreen=yes');
```
This is not the ideal way to set this up, and it is something I did on my last week on the project. A future step would be to make this feature more interactive and efficient.
    
    

NOTE:
More detailed information can be found within the comments of the HTML, CSS and Javascript files included in the WEBPAGE directory.
    
   

