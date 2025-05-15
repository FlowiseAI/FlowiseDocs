# Character Text Splitter

This is the simplest method of text splitting. This splits based on characters (by default "\n\n") and measure chunk length by number of characters.

## Inputs

Separator: default "\n\n"\
Chunck Size: the maximum size of your chunk length by number of characters\
Chunck Overlap: the maximum overlap between chunks. It can be nice to have some overlap to maintain some continuity between chunks (e.g. do a sliding window)\


## Output

Charater Text Splitter: the split chunks of text
