---
title: 'ISITDTU CTF 2022 Finals - Slow'
authors:
  - FazeCT
date: '2023-01-13T15:44:54Z'
doi: ''

# Schedule page publish date (NOT publication's date).
publishDate: '2023-01-13T16:17:00Z'

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ['9']

# Publication name and optional abbreviated publication name.
publication: ''
publication_short: ''

abstract: An in-depth writeup on ISITDTU CTF 2022 Finals - Slow

# Summary. An optional shortened abstract.
summary: An in-depth writeup on ISITDTU CTF 2022 Finals - Slow

tags:
  - ctf
  - writeup
  - re
  - isitdtu-2022

featured: true

links:
  # - name: Custom Link
  #   url: http://example.org
url_pdf: ''
url_code: ''
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.


# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects:
    - ctf-2022-writeups

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides:
---
{{< toc >}}

## Introduction

**Given binary:** [Get it here!](https://drive.google.com/file/d/1K2NjzRQadtL9CkbTINYDvrH7HRgSfDc1/view?usp=share_link)

**Description:** If you can make the program runs faster, you'll get the flag!

**Category:** Reverse Engineering

## Static Analysis

The challenge provides us with a single binary, named **slow.exe**. By using **IDA Pro** or **Ghidra** or any other kinds of decompiler, we will get the decompiled code.

Analyze the **main** function, we claim that the program initiates an array whose size is **45**, then modifies it through some more functions, as shown below.

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  void *Block; // [esp+4h] [ebp-BCh]
  int v5[45]; // [esp+8h] [ebp-B8h] BYREF

  v5[0] = 10;
  v5[1] = -3;
  ... snip
  v5[43] = 14;
  v5[44] = 16;
  Block = (void *)sub_401AC0(v5, 38, 0);
  sub_4013B0(Block);
  sub_401B40(Block);
  return 0;
}
```

The function **sub_401AC0(v5, 38, 0)** allocates dynamic memory using **malloc** based on **v5** then assigns it into variable **Block**. That variable is then being passed into function **sub_4013B0(Block)**, which will produce our flag once we have fixed it.

```c
int __cdecl sub_4013B0(_DWORD *a1)
{
  int result; // eax
  int v2; // eax
  int v3; // [esp+4h] [ebp-64h]
  ... snip
  int v37; // [esp+64h] [ebp-4h]
  int v38; // [esp+64h] [ebp-4h]

  while ( 1 )
  {
    v6 = *(_DWORD *)(a1[1] + 4 * a1[3]++);
    result = v6 - 1;
    switch ( v6 )
    {
      case 1:
        v22 = *(_DWORD *)(a1[2] + 4 * a1[4]--);
        v26 = *(_DWORD *)(a1[2] + 4 * a1[4]--);
        v2 = sub_401110(v26, v22);
        v16 = a1[4] + 1;
        a1[4] = v16;
        *(_DWORD *)(a1[2] + 4 * v16) = v2;
        break;
      case 2:
        ... snip
      case 4:
        ... snip
      case 5:
        ... snip
      case 6:
        ... snip
      case 7:
        ... snip
      case 8:
        ... snip
      case 9:
        ... snip
      case 10:
        ... snip
      case 11:
        ... snip
      case 12:
        ... snip
      case 13:
        ... snip
      case 14:
        v38 = *(_DWORD *)(a1[2] + 4 * a1[4]--);
        sub_401040("RESULT: %d\n", v38);
        sub_401260(v38);
        break;
      case 15:
        ... snip
      case 16:
        ... snip
      case 17:
        ... snip
      case 18:
        ... snip
      default:
        continue;
    }
  }
}
```

It is easy to observe that only case 1 and case 14 involve calling other functions. 

To be more precise, if the program reaches **case 1**, the function **sub_401110(v26, v22)** will be called, and on the other hand, if the program reaches **case 14**, the function **sub_401260(v38)** will be called. We will talk more about these two functions in the next parts of this blog.

## Reaching case 14

As stated earlier, the function **sub_401260(v38)** will be called if the program reaches **case 14**, which will be the last part of our code flow. 

```c
int __cdecl sub_401260(char a1)
{
  char v2[256]; // [esp+10h] [ebp-224h] BYREF
  char Buffer; // [esp+110h] [ebp-124h] BYREF
  _BYTE v4[3]; // [esp+111h] [ebp-123h] BYREF
  char v5[32]; // [esp+210h] [ebp-24h] BYREF

  qmemcpy(v5, "Áõ", 2);
  v5[2] = -77;
  v5[3] = 26;
  ... snip
  v5[28] = -66;
  v5[29] = 63;
  memset(v2, 0, sizeof(v2));
  sub_401D50(&Buffer, "%d", 55 * a1);
  sub_401160(v5, v2, 30, &Buffer, &v4[strlen(&Buffer)] - v4);
  return sub_401040("flag is: %s", (char)v2);
}
```

The function receives our modified variable **Block**, then uses it to produce our flag.

## Reaching case 1

Here is where things get interesting. Take a look at the function **sub_401110(v26, v22)**, we can conclude that this is why our program runs slowly. The fact that it makes our program sleeps plus it is possibly called many times throughout the process makes our executable runs without any output for a very long time.

```c IDA Decompiled Pseudocode
int __cdecl sub_401110(int a1, int a2)
{
  int v3; // [esp+0h] [ebp-4h]

  v3 = sub_4010F0(0);
  Sleep(1000 * a1);
  Sleep(1000 * a2);
  return sub_4010F0(0) - v3;
}
```

The algorithm here is very simple, however this is author's idea to let the program sleeps for a total of **(a1 + a2) seconds** each time this function is called. The intended result of this function is to **return a1 + a2**. We will have to patch the binary to get our flag.

## Patch the binary

So we know what makes our program runs slowly, it is time to fix that. Below is the decompiled assembly code of that part.

```
mov     ecx, [ebp+arg_0]
mov     edx, [ecx+10h]
sub     edx, 1
mov     eax, [ebp+arg_0]
mov     [eax+10h], edx
mov     ecx, [ebp+var_10]
push    ecx
mov     edx, [ebp+var_C]
push    edx
call    sub_401110
add     esp, 8
mov     [ebp+var_58], eax
mov     eax, [ebp+arg_0]
```
Instead of calling **sub_401110**, we should patch the program to directly calculates **ecx + edx** then assigns it into **eax**. We find out that the opcode of **call sub_401110** is **E8 77 FC FF FF**.

Using **IDA Pro** integrated settings, which can be found at **Options > Generals > Number of Opcode bytes (non-graph) set to a large enough number**, we can view each instruction's opcode.

With [pwntools](https://github.com/Gallopsled/pwntools) library, we also find out the opcode for **add ecx, edx** and **move eax, ecx** is **01 D1** and **89 C8** using this script written in **Python** below.

```python
from pwn import *
context.arch = 'amd64'
print(asm('add ecx, edx'))
print(asm('mov eax, ecx'))
```

It is now time to patch the binary. Use any hex editor of your choice to patch the binary, here I use **IDA Pro**'s integrated **hex view** to patch the binary.

Change **E8 77 FC FF FF** to **01 D1 89 C8 90** using any hex editor of your choice (here **90** corresponds to the **NOP** instruction).

## Result

After patching the binary, run it again to get our flag.

```
fazect@LAPTOP-CQA118DI:/mnt/d/Downloads$ ./slow.exe
RESULT: 75025
flag is: Pr4ct1c3_VMc0d3_w1th_F1b0n4cc1
```

Wrap the flag with **ISITDTU{}**, we have our flag for the challenge: **ISITDTU{Pr4ct1c3_VMc0d3_w1th_F1b0n4cc1}**.
