# Character Text Splitter
This is the simplest method of text splitting. This splits based on characters (by default "\n\n") and measure chunk length by number of characters.
## Inputs
Separator: default "\n\n" <br>
Chunck Size: the maximum size of your chunk length by number of characters <br>
Chunck Overlap: the maximum overlap between chunks. It can be nice to have some overlap to maintain some continuity between chunks (e.g. do a sliding window) <br>
## Output 
Charater Text Splitter: the split chunks of text
