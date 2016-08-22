# Cleversafe-Docs
This repo contains the customer and developer documents for IBM Cleversafe Cloud

Many of these documents are intended primarily for use of the IBM Blue Box / Cleversafe operations and deployment teams, and internal support teams. The internal and external facing documents are so noted.

Here are some **Style Conventions** for this Repository, if you are editing these documents:

 * Blue Box (2 words, Both Capitalized)
 * Box Panel (2 words, Both Capitalized)
 * SoftLayer (one word, CamelCase)
 * OpenStack (one word, CamelCase)
 * Vyatta (Capitalized)
 * Ursula (Capitalized)
 * Neutron, Nova, and so forth, all names of OpenStack projects (Capitalized)
 * GitHub (CamelCase)

Note that these conventions might not apply when these names are used in `code snippets`. Thus, any appearance of these terms that does not follow the conventions would only appear in `courier font`, easily identifiable as `code`.

Please avoid using abbreviations such as BP for Box Panel and SL for SoftLayer at any time. 
Similarly, please avoid using BBG as an abbreviation for IBM Blue Box or Blue Box. Thanks for your help!

Please contact Leslie if you need more information about why; there are a lot of reasons, among them, internationalization.

Comments or questions? Contact Leslie Lundquist llundquist@us.ibm.com or @llundquist on Slack.


**Format:**

We need to use "Markdown" format in the documentation, so the documents can be displayed from the interface the Atlas team is setting up. First, just be sure that every file you put into the repository has the extension .MD

See below for information on Markdown:

https://guides.github.com/features/mastering-markdown/

GitHub natively will display Markdown files as WYSIWYG, so you can check some of your formatting.

**Front Matter Required**

To make these articles display correctly using the Atlas UI, there needs to be a section of YML front matter at the beginning of each .MD file. We will do the conversion to publish on the Web using Jekyll.

NOTE with upgrade to Jekyll 3.0, fields are case-sensitive and must be lowercase or camelcase. The 'date' field has been changed to 'dateAdded' field.

**Fields are:**

layout : always set to Page. This tells Jekyll how to render the documentation HTML.

title : sets the header for the documentation page being written. Displayed in all Nav bars, searches, etc.

featured : If this is set to TRUE, the document is visible in the Nav bar. If FALSE, it is hidden under "view more" in the Nav.

weight : determines order in which items are displayed in the Nav. 1 = first. (NOT REQUIRED)

tags : displayed on documentation pages and the search page. Must be enclosed in brackets and comma-delimited [like, this].

dateAdded : Added to the header data next to the page author. No specific format required.

author : Added to the header data. No specific format required.

Also, there is special treatment required for highlighting code blocks so they will line-wrap correctly.

{% highlight bash %}

YOUR CODE BLOCK HERE

{% endhighlight %}

**Sample Document:** 

Here is a template for the required YML headers:

https://github.ibm.com/BlueBoxDocs/IBM-Blue-Box-Docs/blob/master/SampleFile.md


**General Style Formatting Guidelines:**

For now, just use Markdown H1, H2, and so forth.

FIGURE CAPTIONS ARE 12 PT BOLD ALL CAPS

Please make figures no more than 6.5''wide if at all possible.

`Code snippets` should be formatted in `Courier, 12 pt`.


Contact llundquist@us.ibm.com OR @llundquist on Slack with questions and concerns.
