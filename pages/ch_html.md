---
layout: default
title: C++ Toolkit test
nav: pages/ch_html
---

Chapter 12. HTML
========

Last Update: November 15, 2013.

The HTML API [Library `xhtml`: [include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/html) \\| [src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/html)]
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

Introduction

**This C++ HTML generation API is slowly but surely going out of fashion. Nowadays, it's recommended to use mainstream XML/XSLT approach to prepare HTML pages; in particular, the** [**XmlWrapp API**](ch_xmlwrapp.html).

**NB Don't confuse it with the** [**C++ CGI framework API**](ch_cgi.html) **-- which is alive and well!**

The HTML module can be used to compose and print out a HTML page by using a static HTML template with embedded dynamic fragments. The HTML module provides a rich set of classes to help build the dynamic fragments using HTML tag nodes together with [text nodes](#text-nodes) arranged into a tree-like structure.

This chapter provides reference material for many of the HTML facilities. You can also see the quick reference guide, a note about [using the HTML and CGI classes together](#using-the-html-and-cgi-classes-together) and an additional [class reference document](#class-reference-document). For an overview of the HTML module please refer to the HTML section in the introductory chapter on the C++ Toolkit.

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [NCBI C++ HTML Classes](#ncbi-c++-html-classes)

    -   [Basic Classes](#basic-classes)

        -   [CNCBINode](#cncbinode)

        -   [CHTMLText](#chtmltext)

        -   [CHTMLPlainText](#chtmlplaintext)

        -   [CHTMLNode](#chtmlnode)

        -   [CHTMLElement](#chtmlelement)

        -   [CHTMLOpenElement](#chtmlopenelement)

        -   [CHTMLListElement](#chtmllistelement)

<!-- -->

-   [Specialized Tag Classes used in Forms](#specialized-tag-classes-used-in-forms)

    -   [CHTML\_form: derived from CHTMLElement](#chtmlform-derived-from-chtmlelement)

    -   [CHTML\_input: derived from CHTMLOpenElement](#chtmlinput-derived-from-chtmlopenelement)

    -   [CHTML\_checkbox: derived from CHTML\_input](#chtmlcheckbox-derived-from-chtmlinput)

    -   [CHTML\_hidden: derived from CHTML\_input](#chtmlhidden-derived-from-chtmlinput)

    -   [CHTML\_image: derived from CHTML\_input](#chtmlimage-derived-from-chtmlinput)

    -   [CHTML\_radio: derived from CHTML\_input](#chtmlradio-derived-from-chtmlinput)

    -   [CHTML\_reset: derived from CHTML\_input](#chtmlreset-derived-from-chtmlinput)

    -   [CHTML\_submit: derived from CHTML\_input](#chtmlsubmit-derived-from-chtmlinput)

    -   [CHTML\_text: derived from CHTML\_input](#chtmltext-derived-from-chtmlinput)

    -   [CHTML\_select: derived from CHTMLElement](#chtmlselect-derived-from-chtmlelement)

    -   [CHTML\_option: derived from CHTMLElement](#chtmloption-derived-from-chtmlelement)

    -   [CHTML\_textarea: derived from CHTMLElement](#chtmltextarea-derived-from-chtmlelement)

<!-- -->

-   [Specialized Tag Classes used in Lists](#specialized-tag-classes-used-in-lists)

    -   [CHTML\_dl: derived from CHTMLElement](#chtmldl-derived-from-chtmlelement)

    -   [CHTML\_ol: derived from CHTMLListElement](#chtmlol-derived-from-chtmllistelement)

<!-- -->

-   [Other Specialized Tag Classes](#other-specialized-tag-classes)

    -   [CHTML\_table: derived from CHTMLElement](#chtmltable-derived-from-chtmlelement)

    -   [CHTML\_a: derived from CHTMLElement](#chtmla-derived-from-chtmlelement)

    -   [CHTML\_img: derived from CHTMLOpenElement](#chtmlimg-derived-from-chtmlopenelement)

    -   [CHTML\_font: derived from CHTMLElement](#chtmlfont-derived-from-chtmlelement)

    -   [CHTML\_color: derived from CHTMLElement](#chtmlcolor-derived-from-chtmlelement)

    -   [CHTML\_br: derived from CHTMLOpenElement](#chtmlbr-derived-from-chtmlopenelement)

    -   [CHTML\_basefont: derived from CHTMLElement](#chtmlbasefont-derived-from-chtmlelement)

<!-- -->

-   [Generating Web Pages with the HTML classes](#generating-web-pages-with-the-html-classes)

    -   [The CNCBINode class](#the-cncbinode-class)

    -   [HTML Text nodes: CHTMLText and CHTMLPlainText](#html-text-nodes-chtmltext-and-chtmlplaintext)

    -   [The NCBI Page classes](#the-ncbi-page-classes)

    -   [Using the CHTMLPage class with Template Files](#using-the-chtmlpage-class-with-template-files)

    -   [The CHTMLTagNode class](#the-chtmltagnode-class)

    -   [The CHTMLNode class](#the-chtmlnode-class)

    -   [The CHTMLDualNode class](#the-chtmldualnode-class)

    -   [Using the HTML classes with a CCgiApplication object](#using-the-html-classes-with-a-ccgiapplication-object)

<!-- -->

-   [Supplementary Information](#supplementary-information)

    -   The <span class="nctnt ncbi-type">CNCBINode::TMode</span> class

    -   [Quick Reference Guide](#quick-reference-guide)

**Demo Cases** [[src/html/demo](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/html/demo)]

**Test Cases** [[src/html/test](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/html/test)]

NCBI C++ HTML Classes
---------------------

The NCBI C++ HTML classes are intended for use in CGI programs that generate HTML. By creating a structured method for creating HTML, these classes allow for reuse of HTML generating code and simplifies laborious tasks, such as creating and maintaining tables.

A good resource for the use of HTML is the **HTML Sourcebook** by Ian Graham.

Using these classes, the in-memory representation of an HTML page is of a graph: each element on the page can have other elements as children. For example, in

    <HTML><BODY>hello</BODY></HTML>

the body tag is a child of the html tag and the text "hello" is a child of the body tag. This graph structure allows for the easy addition of components as well as reuse of code among components since they share the same base classes.

A sample program, `htmldemo.cpp`, can be found in `internal/c++/src/html/demo`.

Next, the following topics are discussed:

-   [Basic Classes](#basic-classes)

-   [Specialized Tag Classes used in Forms](#specialized-tag-classes-used-in-forms)

-   [Specialized Tag Classes used in Lists](#specialized-tag-classes-used-in-lists)

-   [Other Specialized Tag Classes](#other-specialized-tag-classes)

### Basic Classes

There are several basic classes for the html library. The most basic class is <span class="nctnt ncbi-class">CNCBINode</span>, which is a node that knows how to contain and manipulate child <span class="nctnt ncbi-class">CNCBINodes</span>. Two main types of classes are derived from <span class="nctnt ncbi-class">CNCBINode</span>, text nodes and tag (or "element") nodes. The text nodes (<span class="nctnt ncbi-class">CHTMLText</span> and <span class="nctnt ncbi-class">CHTMLPlainText</span>) are intended to be used directly by the user, whereas the basic tag nodes (<span class="nctnt ncbi-class">CHTMLNode</span>, <span class="nctnt ncbi-class">CHTMLElement</span>, <span class="nctnt ncbi-class">CHTMLOpenElement</span>, and <span class="nctnt ncbi-class">CHTMLListElement</span>) are base classes for the nodes actually used to construct a page, such as <span class="nctnt ncbi-class">CHTML\_form</span>.

<span class="nctnt ncbi-class">CHTMLText</span> and <span class="nctnt ncbi-class">CHTMLPlainText</span> are both used to insert text into the generated html, with the latter class performing HTML encoding before generation.

<span class="nctnt ncbi-class">CHTMLNode</span> is the base class for <span class="nctnt ncbi-class">CHTMLElement</span> (tags with close tags, like <span class="nctnt ncbi-code">FORM</span>), <span class="nctnt ncbi-class">CHTMLOpenElement</span> (tags without end tags, like <span class="nctnt ncbi-code">BR</span>) and <span class="nctnt ncbi-class">CHTMLListElement</span> (tags used in lists, like <span class="nctnt ncbi-code">OL</span>).

The following basic classes are discussed in more detail, next:

-   [CNCBINode](#cncbinode)

-   [CHTMLText](#chtmltext)

-   [CHTMLPlainText](#chtmlplaintext)

-   [CHTMLNode](#chtmlnode)

-   [CHTMLElement](#chtmlelement)

-   [CHTMLOpenElement](#chtmlopenelement)

-   [CHTMLListElement](#chtmllistelement)

#### CNCBINode

<span class="nctnt ncbi-class">CNCBINode</span> uses the following typedefs:**typedef list\<CNCBINode\*\> TChildList typedef map\<string, string\> TAttributes**

**CNCBINode\* AppendChild(CNCBINode\* child)** Add a <span class="nctnt ncbi-class">CNCBINode\*</span> to the end the list of child nodes. Returns <span class="nctnt ncbi-var">\*this</span> so you can repeat the operation on the same line, e.g. Node-\>AppendChild(new CNCBINode)-\>AppendChild(new CNCBINode).

**CNCBINode\* AppendChild(CNodeRef& ref)** Add a node by reference to the end the list of child nodes. Returns <span class="nctnt ncbi-var">\*this</span> so you can repeat the operation on the same line.

**void RemoveAllChildren(void)** Removes all child nodes.

**TChildList::iterator ChildBegin(void) TChildList::const\_iterator ChildBegin(void) const** Returns the first child.

**TChildList::iterator ChildEnd(void) TChildList::const\_iterator ChildEnd(void) const** Returns the end of the child list (this is **not** the last child).

**TChildList::iterator FindChild(CNCBINode\* child)** Find a particular child, otherwise return 0.

**virtual CNcbiOstream& Print(CNcbiOstream& out)** Create HTML from the node and all its children and send it to out. Returns a reference to out.

**virtual void CreateSubNodes(void)** This function is called during printing when the node has not been initialized. A newly created node is internally marked as not initialized. The intent of this function is for the user to replace it with a function that knows how to create all of the subchildren of the node. The main use of this function is in classes that define whole regions of pages.

**const string& GetName(void) const void SetName(const string& namein)** Get and set the name of the node.

**bool HaveAttribute(const string& name) const** Check for an attribute. Attributes are like the href in \<a href="[http://www.ncbi.nlm.nih.gov](http://www.ncbi.nlm.nih.gov/)"\>

**string GetAttribute(const string& name) const** Return a copy of the attribute's value

**const string\* GetAttributeValue(const string& name) const** Return a pointer to the attribute's value

**void SetAttribute(const string& name, const string& value) void SetAttribute(const string& name) void SetAttribute(const string& name, int value) void SetOptionalAttribute(const string& name, const string& value) void SetOptionalAttribute(const string& name, bool set) void SetAttribute(const char\* name, const string& value) void SetAttribute(const char\* name) void SetAttribute(const char\* name, int value) void SetOptionalAttribute(const char\* name, const string& value) void SetOptionalAttribute(const char\* name, bool set)** Set an attribute. <span class="nctnt ncbi-func">SetOptionalAttribute()</span> only sets the attribute if value contains a string or is true.

#### CHTMLText

**CHTMLText(const string& text)**

This is a text node that can contain html tags, including tags of the form \<@...@\> which are replaced by <span class="nctnt ncbi-class">CNCBINode</span>'s when printing out (this is discussed further in the <span class="nctnt ncbi-class">CHTMLPage</span> documentation).

**const string& GetText(void) const void SetText(const string& text)** Get and set the text in the node.

#### CHTMLPlainText

**CHTMLPlainText(const string& text)**

This node is for text that is to be HTML encoded. For example, characters like "&" are turned into "&amp;"

**const string& GetText(void) const void SetText(const string& text)**

Get and set text in the node.

#### CHTMLNode

<span class="nctnt ncbi-class">CHTMLNode</span> inherits from <span class="nctnt ncbi-class">CNCBINod</span>e is the base class for html tags.

**CHTMLNode\* SetWidth(int width) CHTMLNode\* SetWidth(const string& width) CHTMLNode\* SetHeight(int height) CHTMLNode\* SetHeight(const string& width) CHTMLNode\* SetAlign(const string& align) CHTMLNode\* SetBgColor(const string& color) CHTMLNode\* SetColor(const string& color)** Sets various attributes that are in common for many tags. Avoid setting these on tags that do not support these attributes. Returns \*this so that the functions can be daisy chained:

    CHTML_table * Table = new CHTML_table;
    Table->SetWidth(400)->SetBgColor("#FFFFFF");

**void AppendPlainText(const string &)** Appends a <span class="nctnt ncbi-class">CHTMLPlainText</span> node. A plain text node will be encoded so that it does not contain any html tags (e.g. "\<" becomes "\<").

**void AppendHTMLText(const string &)** Appends a <span class="nctnt ncbi-class">CHTMLTextNode</span>. This type of node can contain HTML tags, i.e. it is not html encoded.

#### CHTMLElement

<span class="nctnt ncbi-class">CHTMLElemen</span>t is the base class for several tags that have the constructors with the common form:**CHTMLElement() CHTMLElement(CNCBINode\* node) CHTMLElement(const string& text)** The second constructor appends node. The third constructor appends <span class="nctnt ncbi-func">CHTMLText(const string& text)</span>.

The tags derived from this class include: <span class="nctnt ncbi-class">CHTML\_html, CHTML\_head, CHTML\_body, CHTML\_base, CHTML\_isindex, CHTML\_link, CHTML\_meta, CHTML\_script, CHTML\_style, CHTML\_title, CHTML\_address, CHTML\_blockquote, CHTML\_center, CHTML\_div, CHTML\_h1, CHTML\_h2, CHTML\_h3, CHTML\_h4, CHTML\_h5, CHTML\_h6, CHTML\_hr, CHTML\_p, CHTML\_pre, CHTML\_dt, CHTML\_dd, CHTML\_li, CHTML\_caption, CHTML\_col, CHTML\_colgroup, CHTML\_thead, CHTML\_tbody, CHTML\_tfoot, CHTML\_tr, CHTML\_th, CHTML\_td, CHTML\_applet, CHTML\_param, CHTML\_cite, CHTML\_code, CHTML\_dfn, CHTML\_em, CHTML\_kbd, CHTML\_samp, CHTML\_strike, CHTML\_strong, CHTML\_var, CHTML\_b, CHTML\_big, CHTML\_i, CHTML\_s, CHTML\_small, CHTML\_sub, CHTML\_sub, CHTML\_sup, CHTML\_tt, CHTML\_u, CHTML\_blink, CHTML\_map, CHTML\_area</span>

#### CHTMLOpenElement

This is used for tags that do not have a close tag (like <span class="nctnt ncbi-code">img</span>). The constructors are of the same form as <span class="nctnt ncbi-class">CHTMLElement</span>. The tags derived from this class include: <span class="nctnt ncbi-class">CHTML\_pnop</span> (paragraph tag without a close tag)

#### CHTMLListElement

These are elements used in a list.

**CHTMLListElement(void) CHTMLListElement(bool compact) CHTMLListElement(const string& type) CHTMLListElement(const string& type, bool compact)** Construct the ListElement with the given attibutes: <span class="nctnt ncbi-code">TYPE</span> and <span class="nctnt ncbi-code">COMPACT</span>. Both attributes affect the way the ListElement is displayed.

**CHTMLListElement\* AppendItem(const string& item) CHTMLListElement\* AppendItem(CNCBINode\* item)** These functions add <span class="nctnt ncbi-class">CHTMLText</span> and <span class="nctnt ncbi-class">CNCBINode</span> items as children of the <span class="nctnt ncbi-class">CHTMLListElement</span>. The tags derived from this class include: <span class="nctnt ncbi-class">CHTML\_ul, CHTML\_dir, CHTML\_menu</span>.

### Specialized Tag Classes used in Forms

The rest of the sections deal with tag classes that have additional members or member functions that make the tags easier to use. In addition there are helper classes, such as <span class="nctnt ncbi-class">CHTML\_checkbox</span>, that are easier to use instances of HTML tags.

The following specialized tag classes used in forms are discussed, next:

-   [CHTML\_form: derived from CHTMLElement](#chtmlform-derived-from-chtmlelement)

-   [CHTML\_input: derived from CHTMLOpenElement](#chtmlinput-derived-from-chtmlopenelement)

-   [CHTML\_checkbox: derived from CHTML\_input](#chtmlcheckbox-derived-from-chtmlinput)

-   [CHTML\_hidden: derived from CHTML\_input](#chtmlhidden-derived-from-chtmlinput)

-   [CHTML\_image: derived from CHTML\_input](#chtmlimage-derived-from-chtmlinput)

-   [CHTML\_radio: derived from CHTML\_input](#chtmlradio-derived-from-chtmlinput)

-   [CHTML\_reset: derived from CHTML\_input](#chtmlreset-derived-from-chtmlinput)

-   [CHTML\_submit: derived from CHTML\_input](#chtmlsubmit-derived-from-chtmlinput)

-   [CHTML\_text: derived from CHTML\_input](#chtmltext-derived-from-chtmlinput)

-   [CHTML\_select: derived from CHTMLElement](#chtmlselect-derived-from-chtmlelement)

-   [CHTML\_option: derived from CHTMLElement](#chtmloption-derived-from-chtmlelement)

-   [CHTML\_textarea: derived from CHTMLElement](#chtmltextarea-derived-from-chtmlelement)

#### CHTML\_form: derived from CHTMLElement

**CHTML\_form(const string& action = NcbiEmptyString, const string& method = NcbiEmptyString, const string& enctype = NcbiEmptyString)** Add an HTML form tag with the given attributes. <span class="nctnt ncbi-class">NCBIEmptyString</span> is simply a null string.

**void AddHidden(const string& name, const string& value)** Add a hidden value to the form.

#### CHTML\_input: derived from CHTMLOpenElement

**CHTML\_input(const string& type, const string& name)** Create a input tag of the given type and name. Several of the following classes are specialized versions of the input tag, for example, <span class="nctnt ncbi-class">CHTML\_checkbox</span>.

#### CHTML\_checkbox: derived from CHTML\_input

**CHTML\_checkbox(const string& name) CHTML\_checkbox(const string& name, bool checked, const string& description = NcbiEmptyString) CHTML\_checkbox(const string& name, const string& value) CHTML\_checkbox(const string& name, const string& value, bool checked, const string& description = NcbiEmptyString)** Create a checkbox with the given attributes. This is an input tag with <span class="nctnt ncbi-code">type</span> = `"checkbox"`.

#### CHTML\_hidden: derived from CHTML\_input

**CHTML\_hidden(const string& name, const string& value)** Create a hidden value with the given attributes. This is an input tag with <span class="nctnt ncbi-code">type</span> = `"hidden"`.

#### CHTML\_image: derived from CHTML\_input

**CHTML\_image(const string& name, const string& src)** Create an image submit input tag. This is an input tag with <span class="nctnt ncbi-code">type</span> = `"image"`.

#### CHTML\_radio: derived from CHTML\_input

**CHTML\_radio(const string& name, const string& value) CHTML\_radio(const string& name, const string& value, bool checked, const string& description = NcbiEmptyString)** Creates a radio button. Radio buttons are input tags with <span class="nctnt ncbi-code">type</span> = `"radio button"`.

#### CHTML\_reset: derived from CHTML\_input

**CHTML\_reset(const string& label = NcbiEmptyString)** Create a reset button. This is an input tag with <span class="nctnt ncbi-code">type</span> = `"reset"`.

#### CHTML\_submit: derived from CHTML\_input

**CHTML\_submit(const string& name) CHTML\_submit(const string& name, const string& label)** Create a submit button. This is an input tag with <span class="nctnt ncbi-code">type</span> = `"submit"`.

#### CHTML\_text: derived from CHTML\_input

**CHTML\_text(const string& name, const string& value = NcbiEmptyString) CHTML\_text(const string& name, int size, const string& value = NcbiEmptyString) CHTML\_text(const string& name, int size, int maxlength, const string& value = NcbiEmptyString)** Create a text box. This is an input tag with <span class="nctnt ncbi-code">type</span> = `"text"`.

#### CHTML\_select: derived from CHTMLElement

**CHTML\_select(const string& name, bool multiple = false) CHTML\_select(const string& name, int size, bool multiple = false)** Create a selection tag used for drop-downs and selection boxes.

**CHTML\_select\* AppendOption(const string& option, bool selected = false) CHTML\_select\* AppendOption(const string& option, const string& value, bool selected = false)** Add an entry to the selection box by using the option tag. Returns <span class="nctnt ncbi-var">\*this</span> to allow you to daisy-chain calls to <span class="nctnt ncbi-func">AppendOption()</span>.

#### CHTML\_option: derived from CHTMLElement

**CHTML\_option(const string& content, bool selected = false) CHTML\_option(const string& content, const string& value, bool selected = false)** The option tag used inside of select elements. See <span class="nctnt ncbi-class">CHTML\_select</span> for an easy way to add option.

#### CHTML\_textarea: derived from CHTMLElement

**CHTML\_textarea(const string& name, int cols, int rows) CHTML\_textarea(const string& name, int cols, int rows, const string& value)**

Create a textarea tag inside of a form.

### Specialized Tag Classes used in Lists

These are specialized tag classes used in lists. See "Basic Classes" for non-specialized tag classes used in list.

The following specialized tag classes used in lists are discussed, next:

-   [CHTML\_dl: derived from CHTMLElement](#chtmldl-derived-from-chtmlelement)

-   [CHTML\_ol: derived from CHTMLListElement](#chtmlol-derived-from-chtmllistelement)

#### CHTML\_dl: derived from CHTMLElement

**CHTML\_dl(bool compact = false)** Create a <span class="nctnt ncbi-code">dl</span> tag.

**CHTML\_dl\* AppendTerm(const string& term, CNCBINode\* definition = 0) CHTML\_dl\* AppendTerm(const string& term, const string& definition) CHTML\_dl\* AppendTerm(CNCBINode\* term, CNCBINode\* definition = 0) CHTML\_dl\* AppendTerm(CNCBINode\* term, const string& definition)** Append a term and definition to the list by using <span class="nctnt ncbi-code">DD</span> and <span class="nctnt ncbi-code">DT</span> tags.

#### CHTML\_ol: derived from CHTMLListElement

**CHTML\_ol(bool compact = false) CHTML\_ol(const string& type, bool compact = false) CHTML\_ol(int start, bool compact = false) CHTML\_ol(int start, const string& type, bool compact = false)** The last two constructors let you specify the starting number for the list.

### Other Specialized Tag Classes

These tag classes that have additional members or member functions that make the tags easier to use. The following classes are discussed next:

-   [CHTML\_table: derived from CHTMLElement](#chtmltable-derived-from-chtmlelement)

-   [CHTML\_a: derived from CHTMLElement](#chtmla-derived-from-chtmlelement)

-   [CHTML\_img: derived from CHTMLOpenElement](#chtmlimg-derived-from-chtmlopenelement)

-   [CHTML\_font: derived from CHTMLElement](#chtmlfont-derived-from-chtmlelement)

-   [CHTML\_color: derived from CHTMLElement](#chtmlcolor-derived-from-chtmlelement)

-   [CHTML\_br: derived from CHTMLOpenElement](#chtmlbr-derived-from-chtmlopenelement)

-   [CHTML\_basefont: derived from CHTMLElement](#chtmlbasefont-derived-from-chtmlelement)

#### CHTML\_table: derived from CHTMLElement

**CNCBINode\* Cell(int row, int column)** This function can be used to specify the size of the table or return a pointer to a particular cell in the table. Throws a runtime\_error exception when the children of the table are not <span class="nctnt ncbi-code">TR</span> or the children of each <span class="nctnt ncbi-code">TR</span> is not <span class="nctnt ncbi-code">TH</span> or <span class="nctnt ncbi-code">TD</span> or there are more columns than should be.

**int CalculateNumberOfColumns(void) const int CalculateNumberOfRows(void) const** Returns number of columns and number of rows in the table.

**CNCBINode\* InsertAt(int row, int column, CNCBINode\* node) CNCBINode\* InsertTextAt(int row, int column, const string& text)** Inserts a node or text in the table. Grows the table if the specified cell is outside the table. Uses <span class="nctnt ncbi-func">Cell()</span> so can throw the same exceptions.

**void ColumnWidth(CHTML\_table\*, int column, const string & width)** Set the width of a particular column.

**CHTML\_table\* SetCellSpacing(int spacing) CHTML\_table\* SetCellPadding(int padding)** Set the cellspacing or cellpadding attributes.

#### CHTML\_a: derived from CHTMLElement

**CHTML\_a(const string& href, const string& text) CHTML\_a(const string& href, CNCBINode\* node)** Creates a hyperlink that contains the given text or node.

#### CHTML\_img: derived from CHTMLOpenElement

**CHTML\_img(const string& url) CHTML\_img(const string& url, int width, int height)** Creates an image tag with the given attributes.

#### CHTML\_font: derived from CHTMLElement

**CHTML\_font(void) CHTML\_font(int size, CNCBINode\* node = 0) CHTML\_font(int size, const string& text) CHTML\_font(int size, bool absolute, CNCBINode\* node = 0) CHTML\_font(int size, bool absolute, const string& text) CHTML\_font(const string& typeface, CNCBINode\* node = 0) CHTML\_font(const string& typeface, const string& text) CHTML\_font(const string& typeface, int size, CNCBINode\* node = 0) CHTML\_font(const string& typeface, int size, const string& text) CHTML\_font(const string& typeface, int size, bool absolute, CNCBINode\* node = 0) CHTML\_font(const string& typeface, int size, bool absolute, const string& text)** Create a font tag with the given attributes. Appends the given text or node. Note that it is cleaner and more reusable to use a stylesheet than to use the font tag.

**void SetRelativeSize(int size)** Set the size of the font tag.

#### CHTML\_color: derived from CHTMLElement

**CHTML\_color(const string& color, CNCBINode\* node = 0) CHTML\_color(const string& color, const string& text)** Create a font tag with the given color and append either node or text.

#### CHTML\_br: derived from CHTMLOpenElement

**CHTML\_br(void) CHTML\_br(int number)** The last constructor lets you insert multiple <span class="nctnt ncbi-code">BR</span> tags.

#### CHTML\_basefont: derived from CHTMLElement

**CHTML\_basefont(int size) CHTML\_basefont(const string& typeface) CHTML\_basefont(const string& typeface, int size)** Set the basefont for the page with the given attributes.

Generating Web Pages with the HTML classes
------------------------------------------

Web applications involving interactions with a client via a complex HTML interface can be difficult to understand and maintain. The NCBI C++ Toolkit classes decouple the complexity of interacting with a CGI client from the complexity of generating HTML output by defining separate class hierarchies for these activities. In fact, one useful application of the HTML classes is to generate web pages "offline".

The chapter on [Developing CGI Applications](ch_cgi.html#ch_cgi.cg_develop_apps) discussed only the activities involved in processing the client's request and generating a response. This section introduces the C++ Toolkit components that support the creation of HTML pages, and concludes with a brief consideration of how the HTML classes can be used in consort with a running <span class="nctnt ncbi-class">CCgiApplication</span>. Further discussion of combining a CGI application with the HTML classes can be found in the section on [An example web-based CGI application](ch_cgi.html#ch_cgi.html). See also [NCBI C++ HTML Classes](#ncbi-c++-html-classes) in the Reference Manual.

The following topics are discussed in this section:

-   [The CNCBINode class](#the-cncbinode-class)

-   [HTML Text nodes: CHTMLText and CHTMLPlainText](#html-text-nodes-chtmltext-and-chtmlplaintext)

-   [The NCBI Page classes](#the-ncbi-page-classes)

-   [Using the CHTMLPage class with Template Files](#using-the-chtmlpage-class-with-template-files)

-   [The CHTMLTagNode class](#the-chtmltagnode-class)

-   [The CHTMLNode class](#the-chtmlnode-class)

-   [The CHTMLDualNode class](#the-chtmldualnode-class)

-   [Using the HTML classes with a CCgiApplication object](#using-the-html-classes-with-a-ccgiapplication-object)

### The <span class="nctnt ncbi-class">CNCBINode</span> ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CNCBINode)) class

All of the HTML classes are derived from the <span class="nctnt ncbi-class">CNCBINode</span> class, which in turn, is derived from the [CObject](ch_core.html#ch_core.CObject) class. Much of the functionality of the many derived subclasses is implemented by the <span class="nctnt ncbi-class">CNCBINode</span> base class. The <span class="nctnt ncbi-class">CNCBINode</span> class has just three data members:

-   <span class="nctnt ncbi-var">m\_Name</span> - a <span class="nctnt ncbi-type">string</span>, used to identify the type of node or to store text data

-   <span class="nctnt ncbi-var">m\_Attributes</span> - a <span class="nctnt ncbi-type">map\<string, string\></span> of properties for this node

-   <span class="nctnt ncbi-var">m\_Children</span> - a list of subnodes embedded (at run-time) in this node

The <span class="nctnt ncbi-var">m\_Name</span> data member is used differently depending on the type of node. For HTML [text](#text) nodes, <span class="nctnt ncbi-var">m\_Name</span> stores the actual body of text. For [CHTMLElement](#chtmlelement) objects, <span class="nctnt ncbi-var">m\_Name</span> stores the HTML tagname that will be used in generating HTML formatted output.

The <span class="nctnt ncbi-var">m\_Attributes</span> data member provides for the encoding of specific features to be associated with the node, such as background color for a web page. A group of "Get/SetAttribute" member functions are provided for access and modification of the node's attributes. All of the "SetAttribute" methods return <span class="nctnt ncbi-var">this</span> - a pointer to the HTML node being operated on, and so, can be daisy-chained, as in:

    table->SetCellSpacing(0)->SetBgColor("CCCCCC");

Care must be taken however, in the order of invocations, as the object type returned by each operation is determined by the class in which the method is defined. In the above example, <span class="nctnt ncbi-var">table</span> is an instance of <span class="nctnt ncbi-class">CHTML\_table</span>, which is a subclass of <span class="nctnt ncbi-class">CNCBINode</span> - where <span class="nctnt ncbi-func">SetBgColor()</span> is defined. The above expression then, effectively executes:

    table->SetCellSpacing(0);
    table->SetBgColor("CCCCCC");

In contrast, the expression:

    table->SetBgColor("CCCCCC")->SetCellSpacing(0);

would fail to compile, as it would effectively execute:

    table->SetBgColor("CCCCCC");
    (CNCBINode*)table->SetCellSpacing(0);

since the method <span class="nctnt ncbi-func">SetCellSpacing()</span> is undefined for <span class="nctnt ncbi-func">CNCBINode()</span> objects.

The <span class="nctnt ncbi-var">m\_Children</span> data member of <span class="nctnt ncbi-class">CNCBINode</span> stores a dynamically allocated list of <span class="nctnt ncbi-class">CNCBINode</span> subcomponents of the node. In general, the in memory representation of each node is a graph of <span class="nctnt ncbi-class">CNCBINode</span> objects (or subclasses thereof), where each object may in turn contain additional <span class="nctnt ncbi-class">CNCBINode</span> children. For example, an unordered list is represented as a <span class="nctnt ncbi-class">CHTML\_ul</span> (<span class="nctnt ncbi-code">\<ul\></span>) element containing <span class="nctnt ncbi-class">CHTML\_li</span> (<span class="nctnt ncbi-code">\<li\></span>) subcomponents.

A number of member functions are provided to operate on <span class="nctnt ncbi-var">m\_Children</span>. These include methods to access, add, and remove children, along with a pair of begin/end iterators ([ChildBegin()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/search?string=ChildBegin) and [ChildEnd()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/search?string=ChildEnd)), and a function to dereference these iterators (<span class="nctnt ncbi-code">Node(i)</span>).

Depending on flags set at compile time, <span class="nctnt ncbi-var">m\_Children</span> is represented as either a list of <span class="nctnt ncbi-class">CNodeRef</span> objects, or a list of <span class="nctnt ncbi-type">auto\_ptr\<CNodeRef\></span>, where [CNodeRef](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CNodeRef) is a typedef for <span class="nctnt ncbi-type">CRef\<CNCBINode\></span>. This distinction is transparent to the user however, and the important point is that the deallocation of all dynamically embedded child nodes is handled automatically by the containing class.

[CNCBINode::Print()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=Print) recursively generates the HTML text for the node and all of its children, and outputs the result to a specified output stream. The <span class="nctnt ncbi-func">Print()</span> function takes two arguments: (1) an output stream, and (2) a <span class="nctnt ncbi-type">CNCBINode::TMode</span> object, where [TMode](#tmode) is an internal class defined inside the <span class="nctnt ncbi-class">CNCBINode</span> class. The <span class="nctnt ncbi-type">TMode</span> object is used by the print function to determine what type of encoding takes place on the output, and in some cases, to locate the containing parent node.

Many of the <span class="nctnt ncbi-class">CNCBINode</span> objects do not actually allocate their embedded subnodes until the <span class="nctnt ncbi-func">Print()</span> method is invoked. Instead, a kind of lazy evaluation is used, and the information required to install these nodes to <span class="nctnt ncbi-var">m\_Children</span> is used by the <span class="nctnt ncbi-func">CreateSubNodes()</span> method only when output has been requested (see discussion [below](#below)).

A slice of the NCBI C++ Toolkit class hierarchy rooted at the [CNCBINode](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CNCBINode) class includes the following directly derived subclasses:

-   CNCBINode:

    -   <span class="nctnt ncbi-class">CSmallPagerBox</span>

    -   <span class="nctnt ncbi-class">CSelection</span>

    -   <span class="nctnt ncbi-class">CPagerBox</span>

    -   <span class="nctnt ncbi-class">CPager</span>

    -   [CHTMLText](#chtmltext)

    -   <span class="nctnt ncbi-class">CHTMLTagNode</span>

    -   [CHTMLPlainText](#chtmlplaintext)

    -   [CHTMLNode](#chtmlnode)

    -   [CHTMLDualNode](#chtmldualnode)

    -   [CHTMLBasicPage](#chtmlbasicpage)

    -   <span class="nctnt ncbi-class">CButtonList</span>

Many of these subclasses make little sense out of context, as they are designed for use as subcomponents of, for example, a <span class="nctnt ncbi-class">CHTMLPage</span>. Exceptions to this are the text nodes, described next.

### HTML Text nodes: <span class="nctnt ncbi-class">CHTMLText</span> ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLText.html)) and <span class="nctnt ncbi-class">CHTMLPlainText</span> ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPlainText))

The <span class="nctnt ncbi-class">CHTMLText</span> class uses the <span class="nctnt ncbi-var">m\_Name</span> data member (inherited from <span class="nctnt ncbi-class">CNCBINode</span>) to store a text string of arbitrary length. No new data members are introduced, but two new member functions are defined. [SetText()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetText) resets <span class="nctnt ncbi-var">m\_Name</span> to a new string, and [GetText()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=GetText) returns the value currently stored in <span class="nctnt ncbi-var">m\_Name</span>. With the exception of specially `tagged` sections (described below), all text occurring in a <span class="nctnt ncbi-class">CHTMLText</span> node is sent directly to the output without further encoding.

The [CHTMLPlainText](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPlainText) class is provided for text that may require further encoding. In addition to the <span class="nctnt ncbi-func">SetText()</span> and <span class="nctnt ncbi-func">GetText()</span> member functions described for the <span class="nctnt ncbi-class">CHTMLText</span> class, one new data member is introduced. <span class="nctnt ncbi-var">m\_NoEncode</span> is a Boolean variable that designates whether or not the text should be further encoded. <span class="nctnt ncbi-func">NoEncode()</span> and <span class="nctnt ncbi-func">SetNoEncode()</span> allow for access and modification of this private data member. For example:

    (new CHTMLText("<br> testing BR <br>"))->Print(cout);

will generate the output:

    testing BR

whereas:

    (new CHTMLPlainText("<br> testing BR <br>"))->Print(cout);

will generate:

    <br> testing BR <br>

The text in the <span class="nctnt ncbi-class">CHTMLText</span> node is output verbatim, and the web browser interprets the <span class="nctnt ncbi-code">\<br\></span> tags as line breaks. In contrast, the <span class="nctnt ncbi-class">CHTMLPlainText</span> node effectively "insulates" its content from the browser's interpretation by encoding the <span class="nctnt ncbi-code">\<br\></span> tags as "\<br&gt;".

<span class="nctnt ncbi-class">CHTMLText</span> nodes also play a special role in the implementation of page nodes that work with template files. A `tagname` in the text is delimited by "\<@" and "@\>", as in: <span class="nctnt ncbi-code">\<@tagname@\></span>. This device is used for example, when working with [template files](#template-files), to allow additional nodes to be inserted in a pre-formatted web page. The <span class="nctnt ncbi-func">CHTMLText::PrintBegin()</span> method is specialized to skip over the tag names and their delimiters, outputting only the text generated by the nodes that should be inserted in that tagged section. Further discussion of this feature is deferred until the section on the [NCBI page classes](#ncbi-page-classes), which contain a <span class="nctnt ncbi-type">TTagMap</span>.

### The NCBI Page classes

The page classes serve as generalized containers for collections of other HTML components, which are mapped to the page by a <span class="nctnt ncbi-var">tagmap</span>. In general, subcomponents are added to a page using the [AddTagMap()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=AddTagMap) method (described below), instead of the <span class="nctnt ncbi-func">AppendChild()</span> method. The page classes define the following subtree in the C++ Toolkit class hierarchy:

-   [CHTMLBasicPage](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLBasicPage)

    -   [CHTMLPage](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPage)

In addition to the data members inherited from [CNCBINode](#cncbinode), three new private data members are defined in the <span class="nctnt ncbi-class">CHTMLBasicPage</span> class.

-   <span class="nctnt ncbi-var">m\_CgiApplication</span> - a pointer to the <span class="nctnt ncbi-class">CCgiApplication</span>

-   <span class="nctnt ncbi-var">m\_Style</span> - an integer flag indicating subcomponents to display/suppress (e.g., Title)

-   <span class="nctnt ncbi-var">m\_TagMap</span> (see discussion)

In effect, <span class="nctnt ncbi-var">m\_TagMap</span> is used to map strings to tagged subcomponents of the page - some of which may not have been instantiated yet. Specifically, <span class="nctnt ncbi-var">m\_TagMap</span> is defined as a <span class="nctnt ncbi-type">TTagMap</span> variable, which has the following type definition:

    typedef map<string, BaseTagMapper*> TTagMap;

Here, [BaseTagMapper](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=BaseTagMapper) is a base class for a set of functor-like structs. Each of the derived subclasses of <span class="nctnt ncbi-class">BaseTagMapper</span> has a single data member (e.g. <span class="nctnt ncbi-var">m\_Node, m\_Function</span> or <span class="nctnt ncbi-var">m\_Method</span>), which points to either a <span class="nctnt ncbi-class">CNCBINode</span>, or a function that returns a pointer to a <span class="nctnt ncbi-class">CNCBINode</span>. The <span class="nctnt ncbi-class">BaseTagMapper</span> class also has a single member function, [MapTag()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=MapTag), which knows how to "invoke" its data member.

The simplest subclass of <span class="nctnt ncbi-class">BaseTagMapper</span> is the <span class="nctnt ncbi-class">ReadyTagMapper</span> class whose sole data member, <span class="nctnt ncbi-var">m\_Node</span>, is a [CRef](ch_core.html#ch_core.CRef) pointer to a <span class="nctnt ncbi-class">CNCBINode</span>. In this case the <span class="nctnt ncbi-func">MapTag()</span> function simply returns <span class="nctnt ncbi-code">&\*m\_Node</span>. Several different types of tagmappers are derived from the <span class="nctnt ncbi-class">BaseTagMapper</span> class in [nodemap.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/html/nodemap.hpp). Each of these subclasses specializes a different type of data member, which may be a pointer to a free function, a pointer to a member function, or a pointer to an object, as in the case of the <span class="nctnt ncbi-class">ReadyTagMapper</span>. The action taken by the tagmapper's <span class="nctnt ncbi-func">MapTag()</span> method in order to return a pointer to a <span class="nctnt ncbi-class">CNCBINode</span> is implemented accordingly.

The <span class="nctnt ncbi-class">CHTMLBasicPage</span> class also has a member function named <span class="nctnt ncbi-func">MapTag()</span>, which is used in turn, to invoke a tagmapper's <span class="nctnt ncbi-func">MapTag()</span> method. Specifically, <span class="nctnt ncbi-code">CHTMLBasicPage::MapTag(tagname)</span> first locates the installed tagmapper associated with tagname, <span class="nctnt ncbi-code">m\_TagMap[tagname]</span>. If an entry is found, that tagmapper's <span class="nctnt ncbi-func">MapTag()</span> member function is then invoked, which finally returns a pointer to a <span class="nctnt ncbi-class">CNCBINode</span>.

A second member function, <span class="nctnt ncbi-func">CHTMLBasicPage::</span>[AddTagMap(str, obj)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=AddTagMap), provides for the insertion of a new tag string and its associated tagmapper struct to <span class="nctnt ncbi-var">m\_TagMap</span>. Depending on the object type of the second argument, a type-specific implementation of an overloaded helper function, [CreateTagMapper()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CreateTagMapper), can be used to install the desired tagmapper.

In order for a new mapping to have any effect however, the tag must also occur in one of the nodes installed as a child of the page. This is because the <span class="nctnt ncbi-func">Print()</span> methods for the page nodes do virtually nothing except invoke the <span class="nctnt ncbi-func">Print()</span> methods for <span class="nctnt ncbi-var">m\_Children</span>. The <span class="nctnt ncbi-var">m\_TagMap</span> data member, along with all of its supporting methods, is required for the usage of template files, as described in the next section.

The primary purpose of the <span class="nctnt ncbi-class">CHTMLBasicPage</span> is as a base class whose features are inherited by the <span class="nctnt ncbi-class">CHTMLPage</span> class - it is not intended for direct usage. Important inherited features include its three data members: <span class="nctnt ncbi-var">m\_CgiApplication</span>, <span class="nctnt ncbi-var">m\_Style</span>, and <span class="nctnt ncbi-var">m\_TagMap</span>, and its member functions: <span class="nctnt ncbi-func">Get/SetApplication(), Get/SetStyle(), MapTag()</span>, and <span class="nctnt ncbi-func">AddTagMap()</span>. Several of the more advanced HTML components generate their content via access of the running CGI application. For example, see the description of a <span class="nctnt ncbi-class">CSelection</span> node. It is not strictly necessary to specify a CGI application when instantiating a page object however, and constructors are available that do not require an application argument.

### Using the <span class="nctnt ncbi-class">CHTMLPage</span> class with Template Files

The [CHTMLPage](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPage) class is derived from the <span class="nctnt ncbi-class">CHTMLBasicPage</span>. In combination with the appropriate template file, this class can be used to generate the standard NCBI web page, which includes:

-   the NCBI logo

-   a hook for the application-specific logo

-   a top menubar of links to several databases served by the <span class="nctnt ncbi-app">query</span> program

-   a links sidebar for application-specific links to relevant sites

-   a <span class="nctnt ncbi-var">VIEW</span> tag for the application's web interface

-   a bottom menubar for help links, disclaimers, etc.

The template file is a simple HTML text file with one extension -- the use of named tags (<span class="nctnt ncbi-code">\<@tagname@\></span>) which allow the insertion of new HTML blocks into a pre-formatted page. The standard NCBI page template file contains one such tag, <span class="nctnt ncbi-var">VIEW</span>.

The <span class="nctnt ncbi-class">CHTMLPage</span> class introduces two new data members: <span class="nctnt ncbi-var">m\_Title</span> (<span class="nctnt ncbi-type">string</span>), which specifies the title for the page, and <span class="nctnt ncbi-var">m\_TemplateFile</span> (<span class="nctnt ncbi-type">string</span>), which specifies a template file to load. Two constructors are available, and both accept <span class="nctnt ncbi-type">string</span> arguments that initialize these two data members. The first takes just the title name and template file name, with both arguments being optional. The other constructor takes a pointer to a [CCgiApplication](ch_cgi.html#ch_cgi.cgi_app_class) and a style (type <span class="nctnt ncbi-type">int</span>), along with the title and template\_file names. All but the first argument are optional for the second constructor. The member functions, <span class="nctnt ncbi-func">SetTitle()</span> and <span class="nctnt ncbi-func">SetTemplateFile()</span>, allow these data members to be reset after the page has been initialized.

Five additional member functions support the usage of template files and tagnodes as follows:

-   [CreateTemplate()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CreateTemplate) reads the contents of file <span class="nctnt ncbi-var">m\_TemplateFile</span> into a <span class="nctnt ncbi-class">CHTMLText</span> node, and returns a pointer to that node.

-   [CreateSubNodes()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CreateSubNodes) executes <span class="nctnt ncbi-code">AppendChild(CreateTemplate())</span>, and is called at the top of <span class="nctnt ncbi-func">Print()</span> when <span class="nctnt ncbi-var">m\_Children</span> is empty. Thus, the contents of the template file are read into the <span class="nctnt ncbi-var">m\_Name</span> data member of a <span class="nctnt ncbi-class">CHTMLText</span> node, and that node is then installed as a child in the page's <span class="nctnt ncbi-var">m\_Children</span>.

-   [CreateTitle()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CreateTitle) returns <span class="nctnt ncbi-code">new CHTMLText(m\_Title)</span>.

-   [CreateView()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CreateView) is effectively a virtual function that must be redefined by the application. The <span class="nctnt ncbi-class">CHTMLPage</span> class definition returns a null pointer (0).

-   [Init()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=Init) is called by all of the <span class="nctnt ncbi-class">CHTMLPage</span> constructors, and initializes <span class="nctnt ncbi-var">m\_TagMap</span> as follows:<br/><span class="nctnt ncbi-code">void CHTMLPage::Init(void)</span><br/><span class="nctnt ncbi-code">{</span><br/><span class="nctnt ncbi-code"> AddTagMap("TITLE", CreateTagMapper(this, &CHTMLPage::CreateTitle));</span><br/><span class="nctnt ncbi-code"> AddTagMap("VIEW", CreateTagMapper(this, &CHTMLPage::CreateView));</span><br/><span class="nctnt ncbi-code">}</span><br/>As described in the preceding section, [CreateTagMapper()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CreateTagMapper) is an overloaded function that creates a tagmapper struct. In this case, <span class="nctnt ncbi-func">CreateTitle()</span> and <span class="nctnt ncbi-func">CreateView()</span> will be installed as the <span class="nctnt ncbi-var">m\_Method</span> data members in the resulting tagmappers. In general, the type of struct created by <span class="nctnt ncbi-func">CreateTagMapper</span> depends on the argument types to that function. In its usage here, <span class="nctnt ncbi-func">CreateTagMapper</span> is a template function, whose arguments are a pointer to an object and a pointer to a class method:<br/><span class="nctnt ncbi-code">template\<class C\></span><br/><span class="nctnt ncbi-code">BaseTagMapper\* CreateTagMapper(const C\*, CNCBINode\* (C::\*method)(void)) {</span><br/><span class="nctnt ncbi-code"> return new TagMapper\<C\>(method);</span><br/><span class="nctnt ncbi-code">}</span><br/>The value returned is itself a template object, whose constructor expects a pointer to a method (which will be used as a callback to create an object of type C). Here, <span class="nctnt ncbi-func">AddTagMap()</span> installs <span class="nctnt ncbi-func">CreateTitle()</span> and <span class="nctnt ncbi-func">CreateView()</span> as the data member for the tagmapper associated with tag "TITLE" and tag "VIEW", respectively.

An example using the NCBI standard template file should help make these concepts more concrete. The following code excerpt uses the standard NCBI template and inserts a text node at the <span class="nctnt ncbi-var">VIEW</span> tag position:

    #include <html/html.hpp>
    #include <html/page.hpp>
    USING_NCBI_SCOPE;
    int main()
    {
        try {
            CHTMLPage *Page = new CHTMLPage("A CHTMLPage!", "ncbi_page.html");
            Page->AddTagMap( "VIEW", 
      new CHTMLText("Insert this string at VIEW tag"));
            Page->Print(cout);
            cout.flush();
            return 0;
        }
        catch (exception& exc) {
            NcbiCerr << "\n" << exc.what() << NcbiEndl;
        }
        return 1;
    }

The name of the template file is stored in <span class="nctnt ncbi-var">m\_TemplateFile</span>, and no further action on that file will be taken until <span class="nctnt ncbi-code">Page-\>Print(cout)</span> is executed. The call to <span class="nctnt ncbi-func">AddTagMap()</span> is in a sense then, a forward reference to a tag that we know is contained in the template. Thus, although a new <span class="nctnt ncbi-class">CHTMLText</span> node is instantiated in this statement, it is not appended to the page as a child, but is instead "mapped" to the page's <span class="nctnt ncbi-var">m\_TagMap</span> where it is indexed by "VIEW".

The contents of the template file will not be read until <span class="nctnt ncbi-func">Print()</span> is invoked. At that time, the text in the template file will be stored in a [CHTMLText](#chtmltext) node, and when that node is in turn printed, any tag node substitutions will then be made. More generally, nodes are not added to the page's <span class="nctnt ncbi-var">m\_Children</span> graph until <span class="nctnt ncbi-func">Print()</span> is executed. At that time, <span class="nctnt ncbi-func">CreateSubNodes()</span> is invoked if <span class="nctnt ncbi-var">m\_Children</span> is empty. Finally, the actual mapping of a tag (embedded in the template) to the associated <span class="nctnt ncbi-class">TagMapper</span> in <span class="nctnt ncbi-var">m\_TagMap</span>, is executed by <span class="nctnt ncbi-func">CHTMLText::PrintBegin()</span>.

The <span class="nctnt ncbi-class">CHTMLPage</span> class, in combination with a template file, provides a very powerful and general method for generating a "boiler-plate" web page which can be adapted to application-specific needs using the <span class="nctnt ncbi-func">CHTMLPage::AddTagMap()</span> method. When needed, The user can edit the template file to insert additional <span class="nctnt ncbi-code">\<@tagname@\></span> tags. The <span class="nctnt ncbi-func">AddTagMap()</span> method is defined **only** for page objects however, as they are the only class having a <span class="nctnt ncbi-var">m\_TagMap</span> data member.

Before continuing to a general discussion of `tagnodes`, let's review how the page classes work in combination with a template file:

-   A page is first created with a title string and a template file name. These arguments are stored directly in the page's data members, <span class="nctnt ncbi-var">m\_Title</span> and <span class="nctnt ncbi-var">m\_TemplateFile</span>.

-   The page's <span class="nctnt ncbi-func">Init()</span> method is then called to establish tagmap entries for "TITLE" and "VIEW" in <span class="nctnt ncbi-var">m\_TagMap</span>.

-   Additional HTML nodes which should be added to this page are inserted using the page's <span class="nctnt ncbi-func">AddTagMap(tagname, \*node)</span> method, where the string <span class="nctnt ncbi-var">tagname</span> appears in the template as <span class="nctnt ncbi-code">"\<@tagname@\>"</span>. Typically, a CGI application defines a custom implementation of the <span class="nctnt ncbi-func">CreateView()</span> method, and installs it using <span class="nctnt ncbi-code">AddTagMap("VIEW", CreateView())</span>.

-   When the page's <span class="nctnt ncbi-func">Print()</span> method is called, it first checks to see if the page has any child nodes, and if so, assumes there is no template loaded, and simply calls <span class="nctnt ncbi-func">PrintChildren()</span>. If there are no children however, <span class="nctnt ncbi-code">page-\>CreateSubNodes()</span> is called, which in turn calls the <span class="nctnt ncbi-func">CreateTemplate()</span> method. This method simply reads the contents of the template file and stores it directly in a <span class="nctnt ncbi-class">CHTMLText</span> node, which is installed as the only child of the parent page.

-   The page's <span class="nctnt ncbi-func">Print()</span> method then calls <span class="nctnt ncbi-func">PrintChildren()</span>, which (eventually) causes <span class="nctnt ncbi-func">CHTMLText::PrintBegin()</span> to be executed. This method in turn, encodes special handling of <span class="nctnt ncbi-code">"\<@tagname@\>"</span> strings. In effect, it repeatedly outputs all text up to the first "@" character; extracts the <span class="nctnt ncbi-var">tagname</span> from the text; searches the parent page's m\_TagMap to find the <span class="nctnt ncbi-class">TagMapper</span> for that `tagname`, and finally, calls <span class="nctnt ncbi-func">Print()</span> on the HTML node returned by the <span class="nctnt ncbi-class">TagMapper</span>. <span class="nctnt ncbi-func">CHTMLText::PrintBegin()</span> continues in this fashion until the end of its text is reached.

NOTE: appending any child nodes directly to the page prior to calling the <span class="nctnt ncbi-func">Print()</span> method will make the template effectively inaccessible, since <span class="nctnt ncbi-var">m\_Children()</span> will not be empty. For this reason, the user is advised to use <span class="nctnt ncbi-func">AddTagNode()</span> rather than <span class="nctnt ncbi-func">AppendChild()</span> when adding subcomponents.

### The <span class="nctnt ncbi-class">CHTMLTagNode</span> ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLTagNode)) class

The objects and methods described to this point provide no mechanisms for dynamically adding tagged nodes. As mentioned, the user is free to edit the template file to contain additional <span class="nctnt ncbi-code">\<@tag@\></span> names, and <span class="nctnt ncbi-func">AddTagMap()</span> can then be used to associate tagmappers with these new tags. This however, requires that one know ahead of time how many tagged nodes will be used. The problem specifically arises in the usage of template files, as it is not possible to add child nodes directly to the page without overriding the the template file.

The <span class="nctnt ncbi-class">CHTMLTagNode</span> class addresses this issue. Derived directly from <span class="nctnt ncbi-class">CNCBINode</span>, the class's constructor takes a single (<span class="nctnt ncbi-type">string</span> or <span class="nctnt ncbi-type">char\*</span>) argument, <span class="nctnt ncbi-var">tagname</span>, which is stored as <span class="nctnt ncbi-var">m\_Name</span>. The [CHTMLTagNode::PrintChildren()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=PrintChildren) method is specialized to handle tags, and makes a call to [MapTagAll](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=MapTagAll)<span class="nctnt ncbi-code">(GetName(), mode)</span>. Here, <span class="nctnt ncbi-func">GetName()</span> returns the <span class="nctnt ncbi-var">m\_Name</span> of the <span class="nctnt ncbi-class">CHTMLTagNode</span>, and <span class="nctnt ncbi-var">mode</span> is the [TMode](#tmode) argument that was passed in to <span class="nctnt ncbi-func">PrintChildren()</span>. In addition to an enumeration variable specifying the mode of output, a <span class="nctnt ncbi-type">TMode</span> object has a pointer to the parent node that invoked <span class="nctnt ncbi-func">PrintChildren()</span>. This pointer is used by <span class="nctnt ncbi-func">MapTagAll()</span>, to locate a parent node whose <span class="nctnt ncbi-var">m\_TagMap</span> has an installed `tagmapper` for the tagname. The <span class="nctnt ncbi-type">TMode</span> object's parent pointer essentially implements a stack which can be used to retrace the dynamic chain of <span class="nctnt ncbi-func">PrintChildren()</span> invocations, until either a match is found or the end of the call stack is reached. When a match is found, the associated `tagmapper`'s <span class="nctnt ncbi-func">MapTag()</span> method is invoked, and <span class="nctnt ncbi-func">Print()</span> is applied to the node returned by this function.

The following example uses an auxillary <span class="nctnt ncbi-code">CNCBINode(tagHolder)</span> to install additional <span class="nctnt ncbi-class">CHTMLTagNode</span> objects. The tags themselves however, are installed in the containing page's <span class="nctnt ncbi-var">m\_TagMap</span>, where they will be retrieved by the <span class="nctnt ncbi-func">MapTagAll()</span> function, when <span class="nctnt ncbi-func">PrintChildren()</span> is called for the auxillary node. That node in turn, is mapped to the page's <span class="nctnt ncbi-var">VIEW</span> tag. When the parent page is "printed", <span class="nctnt ncbi-func">CreateSubNodes()</span> will create a <span class="nctnt ncbi-class">CHTMLText</span> node. The text node will hold the contents of the template file and be appended as a child to the page. When <span class="nctnt ncbi-func">PrintBegin()</span> is later invoked for the text node, <span class="nctnt ncbi-func">MapTagAll()</span> associates the <span class="nctnt ncbi-var">VIEW</span> string with the <span class="nctnt ncbi-class">CNCBINode</span>, and in turn, calls <span class="nctnt ncbi-func">Print()</span> on that node.

    #include <html/html.hpp>
    #include <html/page.hpp>
    USING_NCBI_SCOPE;
    int main()
    {
        try {
            CHTMLPage *Page = new CHTMLPage("myTitle", "ncbi_page.html");
            CNCBINode *tagHolder = new CNCBINode();
            Page->AddTagMap( "VIEW", tagHolder);
            tagHolder->AppendChild(new CHTMLTagNode("TAG1"));
            tagHolder->AppendChild(new CHTML_br());
            tagHolder->AppendChild(new CHTMLTagNode("TAG2"));
            Page->AddTagMap( "TAG1", 
      new CHTMLText("Insert this string at TAG1"));
            Page->AddTagMap( "TAG2", 
      new CHTMLText("Insert another string at TAG2"));
            Page->Print(cout);
            cout.flush();
            return 0;
        }
        catch (exception& exc) {
            NcbiCerr << "\n" << exc.what() << NcbiEndl;
        }
        return 1;
    }

### The <span class="nctnt ncbi-class">CHTMLNode</span> ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLNode)) class

<span class="nctnt ncbi-class">CHTMLNode</span> is derived directly from the <span class="nctnt ncbi-class">CNCBINode</span> class, and provides the base class for all elements requiring HTML tags (e.g., <span class="nctnt ncbi-code">\<ul\>,\<br\>, \<img\>, \<table\></span>, etc.). The class interface includes several constructors, all of which expect the first argument to specify the HTML tagname for the node. This argument is used by the constructor to set the <span class="nctnt ncbi-var">m\_Name</span> data member. The optional second argument may be either a text string, which will be appended to the node using <span class="nctnt ncbi-func">AppendPlainText()</span>, or a <span class="nctnt ncbi-class">CNCBINode</span>, which will be appended using <span class="nctnt ncbi-func">AppendChild()</span>.

A uniform system of class names is applied; each subclass derived from the <span class="nctnt ncbi-class">CHTMLNode</span> base class is named <span class="nctnt ncbi-class">CHTML\_[tag]</span>, where <span class="nctnt ncbi-var">[tag]</span> is the HTML tag in lowercase, and is always preceded by an underscore. The NCBI C++ Toolkit hierarchy defines roughly 40 subclasses of <span class="nctnt ncbi-class">CHTMLNode</span> - all of which are defined in the Quick Reference Guide at the end of this section. The constructors for "empty" elements, such as <span class="nctnt ncbi-class">CHTML\_br</span>, which have no assigned values, are simply invoked as <span class="nctnt ncbi-class">CHTML\_br()</span>. The Quick Reference Guide provides brief explanations of each class, along with descriptions of the class constructors.

In addition to the subclasses explicitly defined in the hierarchy, a large number of lightweight subclasses of <span class="nctnt ncbi-class">CHTMLNode</span> are defined by the preprocessor macro [DECLARE\_HTML\_ELEMENT](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=DECLARE_HTML_ELEMENT)<span class="nctnt ncbi-code">(Tag, Parent)</span> defined in `html.hpp`. All of these elements have the same interface as other <span class="nctnt ncbi-class">CHTMLNode</span> classes however, and the distinction is invisible to the user.

A rich interface of settable attributes is defined in the base class, and is applicable to all of the derived subclasses, including those implemented by the preprocessor macros. Settable attributes include: <span class="nctnt ncbi-code">class, style, id, width, height, size, alignment, color, title, accesskey,</span> and <span class="nctnt ncbi-code">name</span>. All of the <span class="nctnt ncbi-func">SetXxx()</span> functions which set these attributes return a <span class="nctnt ncbi-var">this</span> pointer, cast as <span class="nctnt ncbi-type">CHTMLNode\*</span>.

### The <span class="nctnt ncbi-class">CHTMLDualNode</span> ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLDualNode)) class

<span class="nctnt ncbi-class">CHTMLDualNode</span> is derived directly from the <span class="nctnt ncbi-class">CNCBINode</span> class, and provides the base class for all elements requiring different means for displaying data in [eHTML](#ehtml) and [ePlainText](#eplaintext) modes.

This class interface includes several constructors. The second argument in these constructors specifies the alternative text to be displayed in <span class="nctnt ncbi-var">ePlainText</span> mode. The first argument of these constructors expects HTML text or pointer to an object of (or inherited from) CNCBINode class. It will be appended to the node using <span class="nctnt ncbi-func">AppendChild()</span> method, and printed out in <span class="nctnt ncbi-var">eHTML</span> mode. For example:

    (new CHTMLDualNode(new CHTML_p("text"),"\nTEXT \n"))->Print(cout);

will generate the output:

    <p>text</p>

whereas:

    (new CHTMLDualNode(new CHTML_p("text"),"\n TEXT \n"))
    ->Print(cout, CNCBINode::ePlainText);

will generate:

    \n TEXT \n

### Using the HTML classes with a <span class="nctnt ncbi-class">CCgiApplication</span> object

The previous chapter described the NCBI C++ Toolkit's [CGI](ch_cgi.html#ch_cgi.cg_develop_apps) classes, with an emphasis on their independence from the HTML classes. In practice however, a real application must employ both types of objects, and they must communicate with one another. The only explicit connection between the CGI and HTML components is in the HTML page classes, whose constructors accept a [CCgiApplication](ch_cgi.html#ch_cgi.cgi_app_class) as an input parameter. The open-ended definition of the page's <span class="nctnt ncbi-var">m\_TagMap</span> data member also allows the user to install `tagmapper` functions that are under control of the application, thus providing an "output port" for the application. In particular, an application-specific <span class="nctnt ncbi-func">CreateView()</span> method can easily be installed as the function to be associated with a page's <span class="nctnt ncbi-var">VIEW</span> tag. The [CGI sample program](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/cgi) provides a simple example of using these classes in coordination with each other.

Supplementary Information
-------------------------

The following topics are discussed in this section:

-   [The CNCBINode::TMode class](#the-cncbinodetmode-class)

-   [Quick Reference Guide](#quick-reference-guide)

### The <span class="nctnt ncbi-type">CNCBINode::TMode</span> class

<span class="nctnt ncbi-type">TMode</span> is an internal class defined inside the <span class="nctnt ncbi-class">CNCBINode</span> class. The <span class="nctnt ncbi-type">TMode</span> class has three data members defined:

-   <span class="nctnt ncbi-code">EMode m\_Mode </span>- an enumeration variable specifying <span class="nctnt ncbi-var">eHTML</span> (0) or <span class="nctnt ncbi-var">ePlainText</span> (1) output encoding

-   <span class="nctnt ncbi-code">CNCBINode\* m\_Node </span>- a pointer to the <span class="nctnt ncbi-class">CNCBINode</span> associated with this <span class="nctnt ncbi-type">TMode</span> object

-   <span class="nctnt ncbi-code">TMode\* m\_Previous </span>- a pointer to the <span class="nctnt ncbi-type">TMode</span> associated with the parent of <span class="nctnt ncbi-var">m\_Node</span>

<span class="nctnt ncbi-func">Print()</span> is implemented as a recursive function that allows the child node to dynamically "inherit" its mode of output from the parent node which contains it. <span class="nctnt ncbi-func">Print()</span> outputs the current node using <span class="nctnt ncbi-func">PrintBegin()</span>, recursively prints the child nodes using <span class="nctnt ncbi-func">PrintChildren()</span>, and concludes with a call to <span class="nctnt ncbi-func">PrintEnd()</span>. <span class="nctnt ncbi-type">TMode</span> objects are created dynamically as needed, inside the <span class="nctnt ncbi-func">Print()</span> function. The first call to <span class="nctnt ncbi-func">Print()</span> from say, a root `Page` node, generally specifies the output stream only, and uses a default <span class="nctnt ncbi-var">eHTML</span> enumeration value to initialize a <span class="nctnt ncbi-type">TMode</span> object. The <span class="nctnt ncbi-type">TMode</span> constructor in this case is:

    TMode(EMode m = eHTML): m_Mode(m), m_Node(0), m_Previous(0) {}

The call to <span class="nctnt ncbi-func">Print()</span> with no <span class="nctnt ncbi-type">TMode</span> argument automatically calls this default constructor to create a <span class="nctnt ncbi-type">TMode</span> object which will then be substituted for the formal parameter <span class="nctnt ncbi-var">prev</span> inside tbe <span class="nctnt ncbi-func">Print()</span> method. One way to think of this is that the initial print call - which will ultimately be propagated to all of the child nodes - is initiated with a "null parent" <span class="nctnt ncbi-type">TMode</span> object that only specifies the mode of output.

    CNcbiOstream& CNCBINode::Print(CNcbiOstream& os, TMode prev)
    {
        // ...

        TMode mode(&prev, this);

        PrintBegin(os, mode);
        try {
            PrintChildren(out, mode);
        }
        catch (...) {
            // ...
        }
        PrintEnd(os, mode); }

In the first top-level call to <span class="nctnt ncbi-func">Print()</span>, <span class="nctnt ncbi-var">prev</span> is the default <span class="nctnt ncbi-type">TMode</span> object described above, with `NULL` values for <span class="nctnt ncbi-var">m\_Previous</span> and <span class="nctnt ncbi-var">m\_Node</span>. In the body of the <span class="nctnt ncbi-func">Print()</span> method however, a new <span class="nctnt ncbi-type">TMode</span> is created for subsequent recursion, with the following constructor used to create the new <span class="nctnt ncbi-type">TMode</span> at that level:

    TMode(const TMode* M, CNCBINode* N) : m_Mode(M->m_Mode),m_Node(N),
    m_Previous(M) {}

where <span class="nctnt ncbi-var">M</span> is the <span class="nctnt ncbi-type">TMode</span> input parameter, and <span class="nctnt ncbi-var">N</span> is the current node.

Thus, the output encoding specified at the top level is propagated to the <span class="nctnt ncbi-func">PrintXxx()</span> methods of all the child nodes embedded in the parent. The <span class="nctnt ncbi-func">CNCBINode::PrintXxx()</span> methods essentially do nothing;<span class="nctnt ncbi-func">PrintBegin()</span> and <span class="nctnt ncbi-func">PrintEnd()</span> simply return 0, and <span class="nctnt ncbi-func">PrintChildren()</span> just calls <span class="nctnt ncbi-func">Print()</span> on each child. Thus, the actual printing is implemented by the [PrintBegin()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=PrintBegin) and [PrintEnd()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=PrintEnd) metwebpgs.html\_CHTMLBasicPaghods that are specialized by the child objects.

As the foregoing discussion implies, a generic <span class="nctnt ncbi-class">CNCBINode</span> which has no children explicitly installed will generate no output. For example, a [CHTMLPage](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPage) object which has been initialized by loading a [template](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CreateTemplate) file has no children until they are explicitly created. In this case, the <span class="nctnt ncbi-func">Print()</span> method will first call [CreateSubNodes()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CreateSubNodes) before executing <span class="nctnt ncbi-func">PrintChildren()</span>. The use of template files, and the associated set of `TagMap` functions are discussed in the section on the NCBI [Page](#page) classes.

### Quick Reference Guide

The following is a quick reference guide to the HTML and related classes:

-   <span class="nctnt ncbi-class">CNCBINode</span>

    -   <span class="nctnt ncbi-class">CButtonList</span>

    -   <span class="nctnt ncbi-class">CHTMLBasicPage</span>

        -   <span class="nctnt ncbi-class">CHTMLPage</span>

<!-- -->

-   <span class="nctnt ncbi-class">CHTMLNode</span>

    -   <span class="nctnt ncbi-class">CHTMLComment</span>

    -   <span class="nctnt ncbi-class">CHTMLOpenElement</span>

        -   <span class="nctnt ncbi-class">CHTML\_br</span>

        -   <span class="nctnt ncbi-class">CHTML\_hr</span>

        -   <span class="nctnt ncbi-class">CHTML\_img</span>

        -   <span class="nctnt ncbi-class">CHTML\_input</span>

            -   <span class="nctnt ncbi-class">CHTML\_checkbox</span>

            -   <span class="nctnt ncbi-class">CHTML\_file</span>

            -   <span class="nctnt ncbi-class">CHTML\_hidden</span>

            -   <span class="nctnt ncbi-class">CHTML\_image</span>

            -   <span class="nctnt ncbi-class">CHTML\_radio</span>

            -   <span class="nctnt ncbi-class">CHTML\_reset</span>

            -   <span class="nctnt ncbi-class">CHTML\_submit</span>

            -   <span class="nctnt ncbi-class">CHTML\_text</span>

<!-- -->

-   [CHTMLElement](#chtmlelement)

    -   <span class="nctnt ncbi-class">CHTML\_a</span>

    -   <span class="nctnt ncbi-class">CHTML\_basefont CHTML\_button</span>

    -   <span class="nctnt ncbi-class">CHTML\_dl</span>

    -   <span class="nctnt ncbi-class">CHTML\_fieldset</span>

    -   <span class="nctnt ncbi-class">CHTML\_font</span>

        -   <span class="nctnt ncbi-class">CHTML\_color</span>

<!-- -->

-   <span class="nctnt ncbi-class">CHTML\_form</span>

-   <span class="nctnt ncbi-class">CHTML\_label</span>

-   <span class="nctnt ncbi-class">CHTML\_legend</span>

-   <span class="nctnt ncbi-class">CHTML\_option</span>

-   <span class="nctnt ncbi-class">CHTML\_select</span>

-   <span class="nctnt ncbi-class">CHTML\_table</span>

    -   <span class="nctnt ncbi-class">CPageList</span>

    -   <span class="nctnt ncbi-class">CPagerView</span>

    -   <span class="nctnt ncbi-class">CQueryBox</span>

<!-- -->

-   <span class="nctnt ncbi-class">CHTML\_tc</span>

-   <span class="nctnt ncbi-class">CHTML\_textarea</span>

-   <span class="nctnt ncbi-class">CHTML\_tr</span>

-   <span class="nctnt ncbi-class">CHTMLListElement</span>

    -   <span class="nctnt ncbi-class">CHTML\_dir</span>

    -   <span class="nctnt ncbi-class">CHTML\_menu</span>

    -   <span class="nctnt ncbi-class">CHTML\_ol</span>

    -   <span class="nctnt ncbi-class">CHTML\_ul</span>

<!-- -->

-   <span class="nctnt ncbi-class">CHTMLPlainText</span>

-   <span class="nctnt ncbi-class">CHTMLTagNode</span>

-   <span class="nctnt ncbi-class">CHTMLDualNode</span>

    -   <span class="nctnt ncbi-class">CHTMLSpecialChar</span>

<!-- -->

-   <span class="nctnt ncbi-class">CHTMLText</span>

-   <span class="nctnt ncbi-class">CPager</span>

-   <span class="nctnt ncbi-class">CPagerBox</span>

-   <span class="nctnt ncbi-class">CSelection</span>

-   <span class="nctnt ncbi-class">CSmallPagerBox</span>

<!-- -->

-   [CButtonList](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CButtonList) (Custom feature not for general use.) Derived from <span class="nctnt ncbi-class">CNCBINode</span>; defined in [components.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CButtonList). An HTML select button with a drop down list; used in <span class="nctnt ncbi-class">CPagerBox</span>. The constructor takes no arguments, and child nodes (options) are added using method [CbuttonList::CreateSubNodes()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CreateSubNodes)

-   [CHTML\_a](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_a) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_a) - an HTML anchor element, as used in *\<a href="..."\>*. The constructor takes the URL string as the argument, and optionally, a <span class="nctnt ncbi-class">CNCBINode</span> to be appended as a child node. The label inserted before the closing tag (\</a\>) can thus be specified by providing a <span class="nctnt ncbi-class">CHTMLText</span> node to the constructor, or by using the <span class="nctnt ncbi-func">AppendChild()</span> after the anchor has been created.

-   [CHTML\_basefont](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_basefont) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_basefont) - an HTML basefont element used to define the font size and/or typeface for text embedded in this node by <span class="nctnt ncbi-func">AppendChild()</span>. The constructor expects one to two arguments specifying size, typeface, or both.

-   [CHTML\_br](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_br) Derived from <span class="nctnt ncbi-class">CHTMLOpenElement</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_br) - the HTML component used to insert line breaks. The constructor takes no arguments.

-   [CHTML\_checkbox](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_checkbox) Derived from <span class="nctnt ncbi-class">CHTML\_input</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_checkbox) - can only be used inside a <span class="nctnt ncbi-class">CHTML\_form</span>; the HTML component for a checkbox. The constructor takes up to four arguments specifying the name (<span class="nctnt ncbi-type">string</span>), value (<span class="nctnt ncbi-type">string</span>), state (<span class="nctnt ncbi-type">bool</span>), and description (<span class="nctnt ncbi-type">string</span>) for the node.

-   [CHTML\_color](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_color) Derived from <span class="nctnt ncbi-class">CHTML\_font</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_color) - an HTML font color element. Two constructors are available, and both expect string color as the first argument. If no other argument is provided, a `NULL` CNCBINode is assumed for the second argument, and text can be added to the node using <span class="nctnt ncbi-func">AppendChild()</span>. An alternative constructor accepts a simple string text argument.

-   [CHTML\_dir](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_dir) Derived from <span class="nctnt ncbi-class">CHTMLListElement</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_dir) - the HTML component used to insert a <span class="nctnt ncbi-code">dir</span> list. The constructor takes zero to two arguments; if no arguments are provided, the <span class="nctnt ncbi-code">compact</span> attribute is by default false, and the <span class="nctnt ncbi-code">type</span> attribute is left to the browser. <span class="nctnt ncbi-code">CHTML\_dir("square", true)</span> will create a compact <span class="nctnt ncbi-code">dir</span> element with square icons. Items can be added to the list using <span class="nctnt ncbi-code">AppendChild(new CHTMLText("\<li\>...")</span>.

-   [CHTML\_dl](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_dl) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_dl) - an HTML glossary list. The constructor takes a single <span class="nctnt ncbi-type">bool</span> argument; if no arguments are provided, the <span class="nctnt ncbi-code">compact</span> attribute is by default false. Terms are added to the list using [AppendTerm()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=AppendTerm).

-   [CHTML\_fieldset](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_fieldset) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_fieldset) - an element that groups related form controls (such as checkboxes, radio buttons, etc.) together to define a `form control group`. The constructors take at most 1 argument, which may be either a <span class="nctnt ncbi-type">string</span> or a <span class="nctnt ncbi-class">CHTML\_legend</span> node. If the argument is a <span class="nctnt ncbi-type">string</span>, then it is used to create a <span class="nctnt ncbi-class">CHTML\_legend</span> node for the <span class="nctnt ncbi-code">fieldset</span>. The individual form controls to be included in the group are specified using the <span class="nctnt ncbi-func">AppendChild()</span> method.

-   [CHTML\_file](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_file) Derived from <span class="nctnt ncbi-class">CHTML\_input</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_file) - used only inside a <span class="nctnt ncbi-class">CHTML\_form</span> - a <span class="nctnt ncbi-code">form</span> input type to create a file widget for selecting files to be sent to the server. The constructor takes a <span class="nctnt ncbi-type">string</span> name and an optional <span class="nctnt ncbi-type">string</span> value.

-   [CHTML\_font](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_font) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_font) - an HTML font element. The constructor takes up to four arguments. The first three arguments specify the font typeface and size, along with a Boolean value indicating whether the given font size is absolute or relative. The last argument is either a <span class="nctnt ncbi-type">string</span> or a <span class="nctnt ncbi-class">CNCBINode</span> containing text. Additional text should be added using the <span class="nctnt ncbi-func">AppendChild()</span> method.

-   [CHTML\_form](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_form) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_form) - an HTML <span class="nctnt ncbi-code">form</span> node with two constructors. The first takes the URL <span class="nctnt ncbi-type">string</span> (for submission of form data) and method (<span class="nctnt ncbi-var">CHTML::eGet</span> or <span class="nctnt ncbi-var">CHTML::ePost</span>), and the <span class="nctnt ncbi-func">AppendChild()</span> method is used to add nodes. The second constructor takes three arguments, specifying the URL, an HTML node to append to the form, and the enumereated get/post method.

-   [CHTML\_hidden](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_hidden) Derived from <span class="nctnt ncbi-class">CHTML\_input</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_hidden) - used only inside a <span class="nctnt ncbi-class">CHTML\_form</span> - the HTML node for adding <span class="nctnt ncbi-code">hidden</span> key/value pairs to the data that will be submitted by an <span class="nctnt ncbi-class">CHTML\_form</span>. The constructor takes a name <span class="nctnt ncbi-type">string</span> and a value, where the latter may be either a <span class="nctnt ncbi-type">string</span> or an <span class="nctnt ncbi-type">int</span>.

-   [CHTML\_hr](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_hr) Derived from <span class="nctnt ncbi-class">CHTMLOpenElement</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_hr) - the HTML component used to insert a horizontal rule. The constructor takes up to three arguments, specifying the size, width and shading to be used in the display.

-   [CHTML\_image](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_image) Derived from <span class="nctnt ncbi-class">CHTML\_input</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_image) - used only inside a <span class="nctnt ncbi-class">CHTML\_form</span> - the HTML component used to add an inline active image to an HTML <span class="nctnt ncbi-code">form</span>. Clicking on the image submits the form data to the <span class="nctnt ncbi-class">CHTML\_form</span>'s URL. The constructor takes three arguments, specifying the name of the node, the URL string for the image file, and a Boolean value (optional) indicating whether or not the displayed image should have a border.

-   [CHTML\_img](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_img) Derived from <span class="nctnt ncbi-class">CHTMLOpenElement</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_img) - an HTML <span class="nctnt ncbi-code">img</span> component for adding an inline image to a web page. The constructor takes a single URL <span class="nctnt ncbi-type">string</span> argument for the image's <span class="nctnt ncbi-code">src</span>. The alternative constructor also accepts two integer arguments specifying the width and height of the displayed image.

-   [CHTML\_input](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_input) Derived from <span class="nctnt ncbi-class">CHTMLOpenElement</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_input) - the base class for all HTML input elements to be added to a <span class="nctnt ncbi-class">CHTML\_form</span>. The constructor takes a (<span class="nctnt ncbi-type">char\*</span>) input type and a (<span class="nctnt ncbi-type">string</span>) name. The constructor for each of the subclasses has a static member <span class="nctnt ncbi-var">sm\_InputType</span> which is passed as the first argument to the <span class="nctnt ncbi-class">CParent</span>'s (<span class="nctnt ncbi-class">CHTML\_input</span>) constructor.

-   [CHTML\_label](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_label) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_label) - associates a label with a form control. The constructors take a <span class="nctnt ncbi-type">string</span> argument which specifies the text for the label, and optionally, a second <span class="nctnt ncbi-type">string</span> argument specifying the <span class="nctnt ncbi-code">FOR</span> attribute. The <span class="nctnt ncbi-code">FOR</span> attribute explicitly identifies the form control to associate with this label.

-   [CHTML\_legend](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_legend) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_legend) - defines a caption for a <span class="nctnt ncbi-class">CHTML\_fieldset</span> element. The constructors take a single argument which may be either a <span class="nctnt ncbi-type">string</span> or a <span class="nctnt ncbi-class">CHTMLNode</span>.

-   [CHTML\_menu](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_menu) Derived from <span class="nctnt ncbi-class">CHTMLListElement</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_menu) - the HTML component used to insert a <span class="nctnt ncbi-code">menu</span> list. The constructor takes zero to two arguments; if no arguments are provided, the <span class="nctnt ncbi-code">compact</span> attribute is by default false, and the <span class="nctnt ncbi-code">type</span> attribute is left to the browser. <span class="nctnt ncbi-code">CHTML\_menu("square", true)</span> will create a compact <span class="nctnt ncbi-code">menu</span> element with square icons. Items can be added to the list using <span class="nctnt ncbi-code">AppendChild(new CHTMLText("\<li\>...")</span>.

-   [CHTML\_ol](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_ol) Derived from <span class="nctnt ncbi-class">CHTMLListElement</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_ol) - the HTML component used to insert an enumerated list. The constructor takes up to three arguments, specifying the starting number, the <span class="nctnt ncbi-code">type</span> of enumeration (Arabic, Roman Numeral etc.), and a Boolean argument specifying whether or not the display should be compact. Items can be added to the list using <span class="nctnt ncbi-code">AppendChild(new CHTMLText("\<li\>...")</span>.

-   [CHTML\_option](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_option) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_option) - an HTML <span class="nctnt ncbi-code">option</span> associated with a <span class="nctnt ncbi-class">CHTML\_select</span> component. The constructor takes a value (<span class="nctnt ncbi-type">string</span>), a label (<span class="nctnt ncbi-type">string</span> or <span class="nctnt ncbi-type">char\*</span>), and a Boolean indicating whether or not the option is by default selected. The last two arguments are optional, and by default the option is not selected.

-   [CHTML\_radio](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_radio) Derived from <span class="nctnt ncbi-class">CHTML\_input</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_radio) - can only be used inside a <span class="nctnt ncbi-class">CHTML\_form</span>; the HTML component for a radio button. The constructor takes up to four arguments specifying the name (<span class="nctnt ncbi-type">string</span>), value (<span class="nctnt ncbi-type">string</span>), state (<span class="nctnt ncbi-type">bool</span>), and description (<span class="nctnt ncbi-type">string</span>) for the node.

-   [CHTML\_reset](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_reset) Derived from <span class="nctnt ncbi-class">CHTML\_input</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_reset) - can only be used inside a <span class="nctnt ncbi-class">CHTML\_form</span>; the HTML component for a reset button. The constructor takes a single optional argument specifying the button's label.

-   [CHTML\_select](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_select) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_select) - an HTML <span class="nctnt ncbi-code">select</span> component. The constructor takes up to three arguments, specifying the name (<span class="nctnt ncbi-type">string</span>) and size (<span class="nctnt ncbi-type">int</span>) of the selection box, along with a Boolean specifying whether or not multiple selections are allowed (default is false). Select options should be added using the <span class="nctnt ncbi-func">AppendOption()</span> method.

-   [CHTML\_submit](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_submit) Derived from <span class="nctnt ncbi-class">CHTML\_input</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_submit) - can only be used inside a <span class="nctnt ncbi-class">CHTML\_form</span>; the HTML component for a submit button. The constructor takes two <span class="nctnt ncbi-type">string</span> arguments specifying the button's name and label (optional). When selected, this causes the data selections in the including <span class="nctnt ncbi-code">form</span> to be sent to the <span class="nctnt ncbi-code">form</span>'s URL.

-   [CHTML\_table](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_table) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_table) - an HTML table element. The constructor takes no arguments, but many member functions are provided to get/set attributes of the table. Because each of the "set attribute" methods returns <span class="nctnt ncbi-var">this</span>, the invocations can be strung together in a single statement.<br/>Use [InsertAt](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/search?string=InsertAt)<span class="nctnt ncbi-code">(row, col, contents)</span> to add <span class="nctnt ncbi-var">contents</span> to table cell <span class="nctnt ncbi-var">row, col</span>. To add contents to the next available cell, use <span class="nctnt ncbi-code">AppendChild (new</span><br/><span class="nctnt ncbi-code"> \<listref rid="webpgs.html\_CHTML\_tc" RBID="webpgs.html\_CHTML\_tc"\> </span><span class="nctnt ncbi-class">CHTML\_tc</span><span class="nctnt ncbi-code"> \</listref\></span><br/><span class="nctnt ncbi-code">(tag, contents))</span>, where <span class="nctnt ncbi-var">tag</span> is type <span class="nctnt ncbi-type">char\*</span> and <span class="nctnt ncbi-var">contents</span> is type <span class="nctnt ncbi-type">char\*, string</span> or <span class="nctnt ncbi-type">CNCBINode\*</span>.

-   [CHTML\_tc](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_tc) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_tc) - an HTML table cell element. All of the constructors expect the first argument to be a <span class="nctnt ncbi-code">char\* tagname</span>. The second argument, if present, may be text (<span class="nctnt ncbi-type">char\*</span> or <span class="nctnt ncbi-type">string</span>) or a pointer to a <span class="nctnt ncbi-class">CNCBINode</span>.

-   [CHTML\_text](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_text) Derived from <span class="nctnt ncbi-class">CHTML\_input</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_text) - can only be used inside a <span class="nctnt ncbi-class">CHTML\_form</span>; the HTML component for a text box inside a form. The constructor takes up to four arguments: name (<span class="nctnt ncbi-type">string</span>), size (<span class="nctnt ncbi-type">int</span>), maxlength (<span class="nctnt ncbi-type">int</span>), and value (<span class="nctnt ncbi-type">string</span>). Only the first argument is required.

-   [CHTML\_textarea](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_textarea) Derived from <span class="nctnt ncbi-class">CHTML\_input</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_textarea) - can only be used inside a <span class="nctnt ncbi-class">CHTML\_form</span>; the HTML component for a textarea inside a form. The constructor takes up to four arguments: name (<span class="nctnt ncbi-type">string</span>), cols (<span class="nctnt ncbi-type">int</span>), rows (<span class="nctnt ncbi-type">int</span>), and value (<span class="nctnt ncbi-type">string</span>). Only the last argument is optional.

-   [CHTML\_tr](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_tr) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_tr) - an HTML table row element. The constructors take a single argument, which may be either a <span class="nctnt ncbi-type">string</span> or a pointer to a <span class="nctnt ncbi-class">CNCBINode</span>.

-   [CHTML\_ul](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_ul) Derived from <span class="nctnt ncbi-class">CHTMLListElement</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTML_ul) - the HTML component used to insert an unordered list. The constructor takes zero to two arguments; if no arguments are provided, the <span class="nctnt ncbi-code">compact</span> attribute is by default false, and the <span class="nctnt ncbi-code">type</span> attribute is left to the browser. <span class="nctnt ncbi-code">CHTML\_menu("square", true)</span> will create a compact list element with square icons. Items can be added to the list using <span class="nctnt ncbi-code">AppendChild(new CHTMLText("\<li\>...")</span>.

-   [CHTMLBasicPage](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLBasicPage) Derived from <span class="nctnt ncbi-class">CNCBINode</span>, defined in [page.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLBasicPage) - The base class for <span class="nctnt ncbi-class">CHTMLPage</span> and its descendants. The HTML page classes serve as generalized containers for collections of other HTML elements, which together define a web page. Each page has a [TTagMap](#ttagmap), which maps names (<span class="nctnt ncbi-type">strings</span>) to the HTML subcomponents embedded in the page. Two constructors are defined. The first takes no arguments, and the other, takes a pointer to a [CCgiApplication](ch_cgi.html#ch_cgi.cgi_app_class) and a <span class="nctnt ncbi-var">style</span> (<span class="nctnt ncbi-type">int</span>) argument.

-   [CHTMLComment](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLComment) Derived from <span class="nctnt ncbi-class">CHTMLNode</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLComment) - used to insert an HTML comment. The constructor takes at most one argument, which may be a <span class="nctnt ncbi-type">char\*</span>, a <span class="nctnt ncbi-type">string</span>, or a <span class="nctnt ncbi-class">CNCBINode</span>. The constructor then uses [AppendPlainText()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=AppendPlainText) or [AppendChild()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=AppendChild), depending on the type of argument, to append the argument to the comment node.

-   [CHTMLElement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLElement) Derived from <span class="nctnt ncbi-class">CHTMLOpenElement</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLElement) - the base class for all tagged elements which require a closing tag of the form `</tagname>`. <span class="nctnt ncbi-class">CHTMLElement</span> specializes the [PrintEnd()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=PrintEnd) method by generating the end tag `</m_Name>` on the output, where <span class="nctnt ncbi-var">m\_Name</span> stores the tagname of the instance's subclass. Subclasses include <span class="nctnt ncbi-class">CHTML\_a, CHTML\_basefont, CHTML\_dl, CHTML\_font, CHTML\_form, CHTML\_option, CHTML\_select, CHTML\_table, CHTML\_tc, CHTML\_textarea</span>, and <span class="nctnt ncbi-class">CHTMLListElement</span>.

-   [CHTMLListElement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLListElement) Derived from [CHTMLElement](#chtmlelement), defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLListElement) - the base class for <span class="nctnt ncbi-class">CHTML\_ul</span>, <span class="nctnt ncbi-class">CHTML\_ol</span>, <span class="nctnt ncbi-class">CHTML\_dir</span>, and <span class="nctnt ncbi-class">CHTML\_menu</span> lists. Arguments to the constructor include the <span class="nctnt ncbi-var">tagname</span> and <span class="nctnt ncbi-var">type</span> strings for the list, along with a Boolean indicating whether or not the list is compact.

-   [CHTMLNode](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLNode) Derived from <span class="nctnt ncbi-class">CNCBINode</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLNode) - the base class for <span class="nctnt ncbi-class">CHTMLComment</span> and <span class="nctnt ncbi-class">CHTMLOpenElement</span>. Attributes include <span class="nctnt ncbi-code">style, id, title, accesskey, color, bgcolor, height, width, align, valign, size, name,</span> and <span class="nctnt ncbi-code">class</span>. All of the constructors require a <span class="nctnt ncbi-var">tagname</span> argument, which may be either type <span class="nctnt ncbi-type">char\*</span> or <span class="nctnt ncbi-type">string</span>. The optional second argument may be type <span class="nctnt ncbi-type">char\*</span>, <span class="nctnt ncbi-type">string</span>, or <span class="nctnt ncbi-class">CNCBINode</span>.

-   [CHTMLOpenElement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLOpenElement) Derived from <span class="nctnt ncbi-class">CHTMLNode</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLOpenElement) - the base class for all tag elements, including <span class="nctnt ncbi-class">CHTMLElement</span>, <span class="nctnt ncbi-class">CHTML\_br</span>, <span class="nctnt ncbi-class">CHTML\_hr</span>, <span class="nctnt ncbi-class">CHTML\_img</span>, and <span class="nctnt ncbi-class">CHTML\_input</span>. All of the constructors require a <span class="nctnt ncbi-var">tagname</span> argument, which may be either type <span class="nctnt ncbi-type">char\*</span> or <span class="nctnt ncbi-type">string</span>. The optional second argument may be type <span class="nctnt ncbi-type">char\*</span>, <span class="nctnt ncbi-type">string</span>, or <span class="nctnt ncbi-class">CNCBINode</span>.

-   [CHTMLPage](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPage) Derived from <span class="nctnt ncbi-class">CHTMLBasicPage</span>; defined in [page.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPage) - the basic 3 section NCBI page. There are two constructors. The first takes a title (type <span class="nctnt ncbi-type">string</span>) and the name of a template file (type <span class="nctnt ncbi-type">string</span>). Both arguments are optional. The other constructor takes a pointer to a [CCgiApplication](ch_cgi.html#ch_cgi.cgi_app_class), a <span class="nctnt ncbi-var">style</span> (type <span class="nctnt ncbi-type">int</span>), a title and a template\_file name. All but the first argument are optional.

-   [CHTMLPlainText](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPlainText) Derived from <span class="nctnt ncbi-class">CNCBINode</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPlainText) - A simple text component, which can be used to insert text that will be displayed verbatim by a browser (may require encoding). The constructor takes two arguments: the text to be inserted (<span class="nctnt ncbi-type">char\*</span> or <span class="nctnt ncbi-type">string</span>) and a Boolean (default `false`) indicating that the output **should** be encoded. See also <span class="nctnt ncbi-class">CHTMLText</span>.

-   [CHTMLTagNode](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLTagNode) Derived from <span class="nctnt ncbi-class">CNCBINode</span>; defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLTagNode).

-   [CHTMLDualNode](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLDualNode) Derived from <span class="nctnt ncbi-class">CNCBINode</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLDualNode) - Allows the user to [explicitly specify](#explicitly-specify) what exactly to print out in <span class="nctnt ncbi-var">eHTML</span> and in <span class="nctnt ncbi-var">ePlainText</span> modes. The constructor takes 2 arguments -- the first one is for <span class="nctnt ncbi-var">eHTML</span> mode output (string or a pointer to a <span class="nctnt ncbi-class">CNCBINode</span>), and the second one is a plain text for <span class="nctnt ncbi-var">ePlainText</span> mode output.

-   [CHTMLSpecialChar](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLSpecialChar) Derived from <span class="nctnt ncbi-class">CHTMLDualNode</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLSpecialChar) - A class for HTML special chars like <span class="nctnt ncbi-code">&nbsp</span>, <span class="nctnt ncbi-code">&copy,</span> etc. Elements of this class have two variants for output, for eHTML and ePlainText modes. For example: <span class="nctnt ncbi-code">&nbsp</span> have plain text variant - " ", and <span class="nctnt ncbi-code">&copy</span> - "(c)". `html.hpp` has several predefined simple classes, based on this class, for any special chars. It is <span class="nctnt ncbi-class">CHTML\_nbsp</span>, <span class="nctnt ncbi-class">CHTML\_gt</span>, <span class="nctnt ncbi-class">CHTML\_lt</span>, <span class="nctnt ncbi-class">CHTML\_quot</span>, <span class="nctnt ncbi-class">CHTML\_amp</span>, <span class="nctnt ncbi-class">CHTML\_copy</span> and <span class="nctnt ncbi-class">CHTML\_reg</span>. Each have one optional arqument, which specify the number of symbols to output.

-   [CHTMLText](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLText) Derived from <span class="nctnt ncbi-class">CNCBINode</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLText) - A simple text component which can be used to install a default web page design (stored in a `template file`) on a <span class="nctnt ncbi-class">CHTMLPage</span> or to simply insert `encoded` text. The [PrintBegin()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=PrintBegin) is specialized to handle tagnodes occurring in the text. The constructor takes a single argument - the text itself - which may be of type <span class="nctnt ncbi-type">char\*</span> or <span class="nctnt ncbi-type">string</span>. <span class="nctnt ncbi-class">CHTMLPlainText</span> should be used to insert text that does not embed any `tagnodes` and requires further encoding.

-   [CNCBINode](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CNCBINode) Derived from <span class="nctnt ncbi-class">CObject</span>, defined in [node.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CNCBINode) - A base class for all other HTML node classes. Contains data members <span class="nctnt ncbi-var">m\_Name, m\_Attributes</span>, and <span class="nctnt ncbi-var">m\_Children</span>. The constructor takes at most one argument, <span class="nctnt ncbi-var">name</span>, which defines the internal data member <span class="nctnt ncbi-var">m\_Name</span>.

-   [CPageList](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CPageList) (Custom feature not for general use.) Derived from <span class="nctnt ncbi-class">CHTML\_table</span>; defined in [components.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CPageList). Used by the pager box components to page between results pages; contains forward and backward URLs, the current page number, and a <span class="nctnt ncbi-code">map\<int, string\></span> that associates page numbers with URLs.

-   [CPager](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CPager) (Custom feature not for general use.) Derived from <span class="nctnt ncbi-class">CNCBINode</span>, defined in [html.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CHTMLPlainText)

-   [CPagerBox](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CPagerBox) (Custom feature not for general use.) Derived from <span class="nctnt ncbi-class">CNCBINode</span>; defined in [components.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CPagerBox). A more elaborate paging component than the <span class="nctnt ncbi-class">CSmallPagerBox</span>; contains pointers to a <span class="nctnt ncbi-class">CPageList</span> and (3) <span class="nctnt ncbi-class">CButtonList</span> components (<span class="nctnt ncbi-code">left, right</span>, and <span class="nctnt ncbi-code">top</span>). Additional properties include width, background color, and number of results.

-   [CPagerView](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CPagerView) (Custom feature not for general use.) Derived from <span class="nctnt ncbi-class">CHTML\_table</span>; defined in [pager.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CPagerView).

-   [CQueryBox](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CQueryBox) (Custom feature not for general use.) Derived from <span class="nctnt ncbi-class">CHTML\_table</span>; defined in [components.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CQueryBox).

-   [CSelection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CSelection) (Custom feature not for general use.) Derived from <span class="nctnt ncbi-class">CNCBINode</span>; defined in [components.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CSelection). A checkbox-like component whose choices are generated (using the <span class="nctnt ncbi-func">CreateSubNodes()</span> method) from the [TCgiEntries](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=TCgiEntries) of a [CCgiRequest](ch_cgi.html#ch_cgi.cgi_http_req) object.

-   [CSmallPagerBox](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CSmallPagerBox) (Custom feature not for general use.) Derived from <span class="nctnt ncbi-class">CNCBINode</span>; defined in [components.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CSmallPagerBox). A minimal paging component that displays the number of results from the query and the current page being viewed. Has background color and width attributes and contains a pointer to a <span class="nctnt ncbi-class">CPageList</span>. See also <span class="nctnt ncbi-class">CPagerBox</span> and <span class="nctnt ncbi-class">CPager</span>.


