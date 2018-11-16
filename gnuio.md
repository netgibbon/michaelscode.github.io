---
layout: page
permalink: portfolio/samples/gnuio/
---

# GNUIO

This is a GNU/Linux implementation of the famous MS-DOS and Windows C and C++ header, conio. I made this for a project at Soft Boiled Elk where we had to get some old ANSI C code running on GNU/Linux Debian. Because it would have been too much work to go through the code and replace conio functions with BaSH escape codes, I designed gnuio.h as a replacement for the outdated windows library and it later versions, I also tried to short-hand a lot of the functions too to speed up development of console programs as you can see below.

This is licensed under GPL-3. Do with it what you wish.

# gnuio.c
{% highlight c %}
/*This is essentially just for linking. Have this in the project root*/
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "gnunio.h"

void cagXY (unsigned int x, unsigned int y)
{
    printf("%s\x1b[%d;%df", clr, y, x);
}

void clrcon()
{
    printf("%s",clr);
}

char getch()
{
char c;
system("sttyraw -echo");
c = getchar();
system("stty cooked echo");

return c;
}

void gotox(unsigned int x)
{
    printf("\x1b[%dG", x);
}

void gotoxy(unsigned int x, unsigned int y)
{
    printf("\x1b[%d;%df]", y, x);
}

void hidecrs()
{
    printf("\x1b[?25L");
}

void showcrs()
{
    printf("\x1b[?25h");
}

void vidreset()
{
    printf("\x1b[0m");
}

void txtcol(char *color)
{
    printf("%s", color);
}

void txtbg(char color[11])
{
char col[11];
strcpy(col, color);
col[2]='4'; //Given color = fg col.
printf("%s", col);
}
{% endhighlight %}

# gnuio.h
{% highlight c %}
/*
 * GNUIO.h - A conio.h for GNU/Linux 
 * Author: Michael Laws
 * Date: 11/09/18
 * Version: 1.0.0
 * Language: C, C++
 */

#ifndef GNUIO_H
#define GNUIO_H

/*The following files also have to be included in gnuio.c
 *In your program, just make sure to include "gnuio.h" and stdio.h
 */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>


/*Console colours*/
/*Color Clear*/
#define ccClear "\x1b[0m"

//Foreground
#define fgBlack "\x1b[30m"
#define fgRed "\x1b[31m"
#define fgGreen "\x1b[32m"
#define fgYellow "\x1b[33m"
#define fgBlue "\x1b[34m"
#define fgMagenta "\x1b[35m"
#define fgCyan "\x1b[36m"

//Foreground intense
#define fgiBlack "\x1b[30;1m"
#define fgiRed "\x1b[31;1m"
#define fgiGreen "\x1b[32;1m"
#define fgiYellow "\x1b[33;1m"
#define fgiBlue "\x1b[34;1m"
#define fgiMagenta "\x1b[35;1m"
#define fgiCyan "\x1b[36;1m"

//Background
#define bgBLACK “\x1b[40m”
#define bgRED “\x1b[41m”
#define bgGREEN “\x1b[42m”
#define bgYELLOW “\x1b[43m”
#define bgBLUE “\x1b[44m”
#define bgMAGENTA “\x1b[45m”
#define bgCYAN “\x1b[46m”
#define bgWHITE “\x1b[47m”

//Background intense
#define bgBLACK “\x1b[40;1m”
#define bgRED “\x1b[41;1m”
#define bgGREEN “\x1b[42;1m”
#define bgYELLOW “\x1b[43;1m”
#define bgBLUE “\x1b[44;1m”
#define bgMAGENTA “\x1b[45;1m”
#define bgCYAN “\x1b[46;1m”
#define bgWHITE “\x1b[47;1m”

//General utilities

#define clr "\x1b[2J" //Clear
#define cursor11 "\x1b[1;1f" //Place cursor at 1,1 in respective console window

//Text modifiers - Fast blink and slow blink
#define slowBlink "\x1b[5m"
#define fastBlink "\x1b[6m"

//general functions

void cagXY(unsigned int x, unsigned int y); //Clear and go to X,Y
void clrcon(); //Clear console
char getch(); //Get character
void gotox(unsigned int x); //Goto X
void gotoXY(unsigned int x, unsigned int y); //Goto X,Y
void hidecrs(); //hide cursor
void showcrs(); //show cursor
void vidreset(); //reset video attributes
void txtcol(char *color); //Text foreground
void txtbg(char color[11]); //Text background

{% endhighlight %}