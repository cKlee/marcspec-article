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

References a MARCspec does not allow:

- Reference to single designators, field tags or subfield codes
- Reference to a position index of a specific field, subfield or character
- Reference to data elements in related records
- Reference to data elements (entries) in the MARC record directory

### Reference to record and field data

On the broadest level you can reference the whole record data with the MARCspec

```...```

The character '.' in this MARCspec must be interpreted as a wildcard for any allowed character in a field tag. E.g. 

```3..```

is then a reference to the data elements in all fields beginning with '3'.

While MARC 21 does only allow digits with the range 001–999 for field tags, for conformity to ISO 2709 [10] MARCspec allows alphabetic characters too. More precisely the field tag may consist of ASCII numeric characters (decimal integers 0-9) and/or ASCII alphabetic characters (uppercase or lowercase, but not both) or the character '.'.

Together with spec for the field tag it is possible to reference specific repetitions of fields like with the MARCspec

```300[0]```

The position of the repeatable field is expressed through its index enclosed with square brackets. The first position is always expressed through the index with the value '0'. Index ranges can also be expressed like in the following MARCpsec ( a reference to the first three repetitions of the field '300')

```300[0-2]```

### Reference to data content

Since field content of fixed fields can be referenced by its character position or character range, MARCspec allows to specify such references through a character spec. The character spec is prefixed by the character '/'. The following MARCspec references the the first character of the field content of the first repetition of field '007'

```007[0]/0```

Content of data fields is divided into subfields. Every subfield is referenced by its subfield code, prefixed with the character '$'. The following MARCspec references the content of subfield 'a' of field '245'

```245$a```

Content of multiple subfields of one field can also be referenced within a single MARCspec like

```245$a$b$c```

or a little bit less verbose by using a subfield tag range

```245$a-c``` 

Since subfields are also repeatable, MARCspec uses the same syntax for references to repetitions of subfields like for references to repetitions of repeatable fields. Thus the MARCspec

```020$z[2]```

is a reference to the content of the third repetition of subfield 'z' of field '020'.

Like the character spec for fixed fields, references to substrings of data content are also possible. E.g. the MARCspec

```020[0]$c/0-3```

is a reference to the first four characters of the content of subfield 'c' of the first repetition of field '020'.

### Index and character position syntax

As you might have noticed from the examples above, the index position of fields and subfields and the character spec of fixed fields and subfields share the same principles. The first index position and the first content character are always expressed through the value '0'. Both positions and substrings can be referenced by ranges of indizes or character positions.

At some point it might be necessary to reference the last repetition or the last character of some content without having knowledge how often a field is repeated or how many characters a subfield holds. Therefore MARCspec uses the character '#' as a symbol for the the last position. Thus the MARCspec

```245$a/#```

is a reference to the last character of subfield 'a' of field '245'. And the MARCspec

```020[#]```

is a reference to the last repetition of field '020'.

The character '#' can also be used in ranges. For instance the MARCspec

```020[1-#]```

is a reference to all but the first repetition of field '020'. To reference all repetitions of one field it is not necessary to specify an index range. Field specs without indizes are always a reference to all repetitions of the field, like subfield specs without a character spec are always references to all content of the subfield.

By using the character '#' as the starting index for an index or character range, the reverse order of indizes is assumned. 

A simple example should illustrate this different behaviour. Imagine a subfield 'q' with content 'pbk.'. As a default order the characters of this content have the following indizes:

```
p => 0
b => 1
k => 2
. => 3
```

The MARCspec

```...$q/1-#```

references the substring 'bk.', since 'b' has the index value '1' and '.' is the last character. If we are now using the character '#' as the starting index like

```...$q/#-1```

the MARCspec references the substring 'k.'. This might be a little bit unexpected. But this behaviour gets more clear if we look at the reverse index order

```
p => 3
b => 2
k => 1
. => 0
```

This interpretation makes it possible to specify a substring beginning at the end of the string.



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

[10]: #10 
\[10] http://en.wikipedia.org/wiki/ISO_2709