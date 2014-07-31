# MARCspec - a common MARC record path language

## Introduction

People who are familiar with the MARC 21 format [1] and especially with the MARC 21 format for bibliographic data [2] know what it means when they read

```
245 $a
```

For those who don't: It is a simple way to express that we talk about the content of the subfield 'a' of the field '245' in a MARC record. You can do this with every combination of field and subfield possible in the MARC specification. In general this is called a __MARC field specification__ or shorter a __MARC spec__.

Such field specifications are commonly used for documentation or illustrative issues like mappings (see [3]). But also existing tools like solrmarc [4] or catmandu [5] are using their own flavour of MARC field specifications as a tool specific configuration language. You cannot expect the same MARC field specification is working across different tools.

The purpose of the hereby described specification __MARCspec__ [6] is it to unify the way a MARC field specification is expressed. MARCspec parsers could then be build on top of this specification working as a basis of different tools and assuring a common syntax for MARC field specifications across different tools.

The current version of MARCspec is a preliminary draft for open discussion. Feedback [7] is welcome!

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