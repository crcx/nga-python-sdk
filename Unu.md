# Unu

    unu
    (verb) (-hia) pull out, withdraw, draw out, extract.

Unu is a tool for extracting fenced code blocks from Markdown documents.

I always found documenting my projects to be annoying. Eventually I decided to start mixing the code into the commentary using Markdown. Unu is the tool I use to extract the sources from the original files. I've found that this makes it easier for me to keep the commentary up to date.

## The Code

### Headers

````
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
````

### read_line(FILE *file, char *line_buffer)

````
void read_line(FILE *file, char *line_buffer) {
  if (file == NULL || line_buffer == NULL)
  {
    printf("Error: file or line buffer pointer is null.");
    exit(1);
  }

  char ch = getc(file);
  int count = 0;

  while ((ch != '\n') && (ch != EOF)) {
    line_buffer[count] = ch;
    count++;
    ch = getc(file);
  }

  line_buffer[count] = '\0';
}
````

### extract(char *fname)

````
void extract(char *fname) {
  char source[32*1024];

  FILE *fp;
  int inBlock;

  inBlock = 0;

  fp = fopen(fname, "r");
  if (fp == NULL)
    return;

  while (!feof(fp)) {
    read_line(fp, source);
    if (strcmp(source, "````") == 0) {
      if (inBlock == 0)
        inBlock = 1;
      else
        inBlock = 0;
    } else {
      if (inBlock == 1) {
        if (strlen(source) != 0)
          printf("%s\n", source);
      }
   }
  }

  fclose(fp);
}
````

### main(int argc, char **argv)

````
int main(int argc, char **argv) {
  int i = 0;
  if (argc > 1) {
    while (i <= argc) {
      extract(argv[i++]);
    }
  }
  else
    printf("unu\n(c) 2013, 2016 charles childers\n\nTry:\n  %s filename\n", argv[0]);
  return 0;
}
````
