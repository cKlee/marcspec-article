# MARCspec - a common MARC record path language

## Introduction

People who are familiar with the MARC21 format [1] and especially with the MARC21 format for bibliographic data [2] know what it means when they read

```
245 $a
```

For those who don't: It is a simple way to express that we talk about the content of the subfield 'a' of the field '245' in a MARC record. You can do this with every combination of field and subfield possible by the MARC specification. In general this is referred to as __MARC field specification__ or shorter a __MARC spec__.

Such field specifications are commonly used for documentation or illustrative issues like mappings (see [3]). But also existing tools like solrmarc [4] or catmandu [5] are using their own flavour of MARC field specifications as a tool specific configuration language. You cannot expect the same MARC field specification is working across different tools.

The purpose of the hereby described specification __MARCspec__ [6] is it to unify the way a MARC field specification is expressed. MARCspec parsers could then be build on top of this specification working as a basis of different tools and assuring a common syntax for MARC field specifications across different tools.

The current version of MARCspec is a preliminary draft for open discussion. Feedback [7] is welcome!

## What is a MARCspec?

To understand what a MARCspec represents one must have a basic knowledge on the MARC principles. For further references I'm giving a brief introduction. Please skip the next paragraph if you are familiar with MARC.

Machine-Readable Cataloguing (MARC) is a document-based key-value exchange format for bibliographic and other library related data. A MARC record consists of three main sections: the leader, the directory, and the variable fields with the data content. There are two kinds of (variable) fields: (variable) control fields and (variable) data fields. The term 'fixed field' stands for fields whose length does not vary, like the leader and some of the control fields. The field content in the the fixed fields can be accessed through its character position or character range. Only data fields are divided into subfields. Subfields can also be contextualized through indicators. There is an indicator 1 and an indicator 2 for all data fields, both are optional. For a deeper explanation of MARC see [8] and [9].

This image illustrates the structure of a data field

![MARC structure](images/marc_structure.jpg)

A MARCspec is expressed as a string and is a reference to field data of a MARC record and it is very much like XPath for XML. With a MARCspec data of a MARC record can be referenced on different levels defined through the fields, character positions, subfields and indicators.

## MARCspec reference levels

With some exceptions a MARCspec can reference data with a range from the whole record down to a single character of the content.


A MARCspec allows the following basic references:

- Reference to record data (except data from the record directory)
- Reference to field data
- Reference to data content of subfields
- Reference to substrings of data content of fixed fields and subfields

References a MARCspec does allow are:

- Reference to single designators, field tags or subfield codes
- Reference to a position index of a specific field, subfield or character
- Reference to data in related records
- Reference to data elements (entries) in the MARC record directory



## References

[1]: #1 
\[1] http://www.loc.gov/marc/

[2]: #2 
\[2] http://www.loc.gov/marc/bibliographic/

[3]: #3 
\[3] http://www.loc.gov/standards/mods/mods-mapping.html#mapping

[4]: #4 
\[4] https://code.google.com/p/solrmarc/

[5]: #5 
\[5] http://librecat.org/ and https://metacpan.org/pod/Catmandu::Fix::marc_map

[6]: #6 
\[6] http://cklee.github.io/marc-spec/

[7]: #7 
\[7] https://github.com/cklee/marc-spec/issues

[8]: #8 
\[8] http://www.loc.gov/marc/96principl.html

[9]: #9 
\[9] http://www.loc.gov/marc/specifications/specrecstruc.html