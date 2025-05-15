# Character Text Splitter
Este es el método más simple de división de texto. Divide basándose en caracteres (por defecto "\n\n") y mide la longitud del chunk por número de caracteres.

## Entradas
Separator: por defecto "\n\n" <br>
Chunck Size: el tamaño máximo de la longitud de tu chunk por número de caracteres <br>
Chunck Overlap: el solapamiento máximo entre chunks. Puede ser útil tener algo de solapamiento para mantener cierta continuidad entre chunks (por ejemplo, hacer una ventana deslizante) <br>

## Salida 
Character Text Splitter: los chunks de texto divididos
