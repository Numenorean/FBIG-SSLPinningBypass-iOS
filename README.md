# How it works

1. After loading FBSharedFramework into ida or another tool we need find string `Error null`
2. Jump to xref -> jump to xref. We got needed function, but from version to version it's in different locations, so we need to find out some patterns that helps us to find function address without ida's xrefs(because it's clear that frida can't reproduce this steps)
3. Let's try to jump to the next xref, we need this one (just because it's a bit easier)
    ![](https://i.imgur.com/AIhTepA.png)
4. We got this branch

    ![](https://i.imgur.com/GvX0bAB.png)
  
5. The pattern we need is highlighted with green color
    ![](https://i.imgur.com/ZoB8bWp.png)
6. Go to hexdump and take this data, which also highlighted with green color
    ```
    A2 02 02 91 A3 12 44 A9  A6 C2 04 91 A5 1A 40 F9
    E0 03 14 AA E1 03 13 AA  4F 8D 94 97 F3 03 00 AA
    ```
    
    `4F 8D 94 97` is actual data we need to get with frida for further parsing
7. It's a dynamic data, so we need to use Memory.scanSync in order to find it, then just parse needed instruction
