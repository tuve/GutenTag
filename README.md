Introduction

The GutenTag project is project aimed at assisting with computational analysis of the Project Gutenberg text corpus, particularly by those working in the digital humanities. For more information on the goals of the project, please visit the main website (www.projectgutentag.org) or read the paper which introduced it [1].

The current version of GutenTag takes the form of a downloadable tool (though there is a web demo, and there is the possibility future iterations might be primarily or entirely web-based), and assumes that you already have a copy of the Project Gutenberg corpus (2010 image), which is available officially on the Project Gutenberg website, and unofficially here; our version is smaller because we have removed files that are not useful for text analysis, but it is still a fairly large download, over 4 gigabytes. Once you have the corpus (and have unzipped it, if necessary), you can access it using GutenTag, you shouldn't need to modify it yourself.

With our goal of helping the full range of users that might be interested in accessing GutenTag's functionality, there are three distinct ways to run GutenTag, depending on your situation (and the version you have downloaded): you can run an executable (which is specific to the operating system you're using, i.e. Mac or PC, see download page on the main website), run the python script, or import GutenTag into other python code and access its functionality through an API. The first two are functionally equivalent in the sense that both access the GutenTag web interface. The main difference is that the python script requires you have Python (2.7) [2] and NLTK (3.0) [3] installed, whereas the exectuable is standalone and does not require any additional software. We have not built an executable for Linux, so Linux users should use the python script. If you are accessing through an API, you can mostly skip the next section, though even API users may use the web interface for the purposes of creating configuration files which access specific subcorpora or do XML (TEI) tagging.
             
                     
 The GutenTag HTML interface

When you run GutenTag (either by clicking on it or running it in a terminal), a brower window should open automatically. The first time you run GutenTag, it will prompt you for the location of the directory which contains the Project Gutenberg corpus: on a GUI like Windows or Mac OS X, you can usually grap the exact full path from a file finder/explorer window opened to that directory (use copy and paste). Click check to see if you've done this right, at which point it will prompt you to refresh your browser.

The main page of the interface allows you to configure everything about a particular run of GutenTag. By clicking on the first button on the screen, you to load an existing (previously saved) parameter file from a list, which will then populate the HTML form with those previous settings. At the bottom of the screen, there is a corresponding button that allows you to enter a filename to save your parameters when you start your run of GutenTag. If you wish, you can also run GutenTag with the -c command (configure), which will allow you to save the parameter file without actually runing GutenTag. The saved parameter files can be found in the saved_parameters directory.

The large box on the screen allows you to configure a subcorpus that you're interested in, focusing on particular types of texts, time periods, authors, etc.  Note that you must select one or more genres, or you will have no output whatsoever. Most of the other opinions limit the range of texts, and if you leave them blank, there will be no restrictions on your texts. Multiple restrictions within a subcorpus means that a text must satisfy all of them: if you select female authors and then select a birth range between 1800 and 1900, you will get texts written by female authors that were also born between 1800 and 1900. Some of the options available are marked with a * which means that some (many) texts are not marked for these aspects and/or the information was obtained automatically. If you select one of the restrictions for which the annotation is incomplete, it will excude all texts for which there is no information about that aspect.

For texts and authors, you can create a predefined list which is accessible through the interface. In order to define such a list, create a new text file in the "user_lists" directory, which one item on each line. Note that what you enter must match exactly the author/title in the PG database. If you want to check to see exactly what name is used for a particular author/text, you can browse the full lists of each attribute in the "full_lists" directory. 

The within text filer with the subcorpus box allows you to exclude parts of texts which otherwise meets your criteria. Probably the most common use of this will be to exclude material outside the main body of the text (that is, exclude the "front", the "back", and headings), though you can select specific tags to include or delete: for instance, for fiction you can include just narration or just speech, for plays you can include just speech or just stage direction, etc. The default is everything, if you want to select a specific set of tags outside of the preset options, first select one of the presets from the drop down menu and then modify them manually by checking or unchecking the box.  

There are two major modes in GutenTag: export and analysis. export creates a new text corpus designed to your specifications, whereas analysis gets statistics about the tags in your subcorpus directly, without the need to save anything. Both modes allow you to include additional lexical tags which are either displayed in the XML (if in TEI mode, see below) or counted. GutenTag includes three major lexical resources: a subset of the tags from the General Inquirer[4], the ratings from the MRC psycholinguistic database[5], and a "6-style" lexical model created automatically from the Project Gutenberg corpus using automated methods [6,7], as well as polarity ratings [7].Users can also define their lexical tags by putting files into the "user_lexicons" directory, which will be accesible through the interface. The format of the file is a text file with one word/phrase per line, optionally with a tab delimited value for each entry. In analysis mode, the result for discrete tags will be the number of tags divided by the total number of tokens across all texs, while for tags with values the output is the sum of all the values divided by the number of tokens in each subcorpus.                                                                                                                                          

You can choose multiple subcorpora in a single run by clicking the add subcorpora button under the first subcorpus box. There are two uses for this: first, you can use this to create a single corpus defined in incompatible ways (for instance, a corpus with women authors from the 19th century and men author from the 18th). In analysis mode, often you want to be able to compare results directly across subcorpora, and this gives an easy way to do that. However, anything that could be accomplished with multiple subcorpora can also be accomplished by running GutenTag multiple times.

In export mode, the most important option is whether to output in plain text mode (just tokens) or in TEI XML, which displays all the structural tagging for the text as well as any additional tags that have been selected. In both modes, you can choose to include POS tags in the output, and also use the original or lemmatized version of the word. When exporting, you must give the name of the output directory which will be created and filled with text files corresponding to individual texts which match the parameters. For both modes, you can choose to limit each subcorpus to a certain number of texts. With respect to the order in which these texts are chosen, you may decide on random (which will create different results each time, but will not show ordering effects within the PG) or fixed (which will have the same results each time).

When you have entered all your parameters for a run of GutenTag, hit the button on the bottom of the screen (export or analyze, depend on your mode), and GutenTag will begin to scan the corpus. The results screen has a progress bar tracking how far it is through the corpus (or, if you have restricted the number of text, the percentage of required texts that have been retrieved), and the list of texts included so far will be updated as it goes along. This can take a long time, particularly if the number of texts which match your input parameters is large. Be patient. When the run is over, the progress bar will disappear and the results page will be scrollable. If you want to do another run of GutenTag, you can click on the GutenTag icon at the top of the page.


GutenTag API:

To access the API, copy the GutenTag.py script as well as the resources directory into the directory with your other python code. GutenTag is not (yet) a Python package, so you need to preserve the basic directory structure. You can then "import GutenTag" in your code, and create a GT_API object.

gt = GutenTag.GT_API(corpus_path, parameter_path)

the corpus_path is the path to the Project Gutenberg corpus. In order to use the API, you need to have a existing parameter file, which includes all your settings for GutenTag (this is preferable to having an extrodinarily long argument list). We provide a default one in the saved_parameters directory "allEnglish" which would allow you to iterate through all the English texts in the corpus. You can create your own by modifying an existing file (it is in human-readable JSON format) or using the main GutenTag HTML interface with the -c command.

There are three methods available through GT_API:

gt.cycle_through_texts()    

This is a iterator function which will return pairs (2-ples) of GutenTag internal Text objects and corresponding information dictionaries for texts that match the given parameters. A Text object contains a set of ordered set of "tokens" (strings), and a set of "tags" in no particular order. Tags include a index "start" which indicates the starting token of the tag, an "end" index which indicates the end of the tag, the "tag" (the xml tag), a dictionary of "attributes" (key value pairs), and a some other attributes which control how they are displayed in TEI output (in general, these shouldn't be modified).  In general, the tags will encode structural and (if desired) lexical information about the text. The "info" dictionary includes basic information about the text, including author, title, genre, etc.

gt.cycle_through_all_info()

This iterator function cycles through the info dictionaries for all the texts in the corpus It is much quicker because it does not have to analyze the texts, and is useful for investigating basic facts about the corpus.

gt.get_text_TEI_string(text, info)

If the user wishes to output a text/info pair from cycle_through_texts() into the TEI (XML) format, this function can be called. A string corresponding to a TEI version of the text will be returned.



