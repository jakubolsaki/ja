PS4 6.20 WebKit Code Execution PoC
==============

This repo contains a proof-of-concept (PoC) RCE exploit targeting the PlayStation 4 on firmware 6.20 leveraging CVE-2018-4441. The exploit first establishes an arbitrary read/write primitive as well as an arbitrary object address leak in `wkexploit.js`. It will then setup a framework to run ROP chains in `index.html` and by default will provide two hyperlinks to run test ROP chains - one for running the `sys_getpid()` syscall, and the other for running the `sys_getuid()` syscall to get the PID and user ID of the process respectively.

Each file contains a comment at the top giving a brief explanation of what the file contains and how the exploit works. Credit for the bug discovery is to lokihardt from Google Project Zero (p0). The bug report can be found [here](https://bugs.chromium.org/p/project-zero/issues/detail?id=1685&desc=2).

Note: It's been patched in the 6.50 firmware update.



Files
==============

Files in order by name alphabetically;

* `index.html` - Contains post-exploit code, going from arb. R/W -> code execution.
* `rop.js` - Contains a framework for ROP chains.
* `syscalls.js` - Contains an (incomplete) list of system calls to use for post-exploit stuff.
* `wkexploit.js` - Contains the heart of the WebKit exploit.



Notes
==============

* This vulnerability was patched in 6.50 firmware!
* This only gives you code execution in **userland**. This is **not** a jailbreak nor a kernel exploit, it is only the first half.
* This exploit targets firmware 6.20. It should work on lower firmwares however the gadgets will need to be ported, and the `p.launchchain()` method for code execution may need to be swapped out.
* In my tests the exploit as-is is pretty stable, but it can become less stable if you add a lot of objects and such into the exploit. This is part of the reason why `syscalls.js` contains only a small number of system calls.



Usage
==============

Setup a web-server hosting these files on localhost using xampp or any other program of your choosing. Additionally, you could host it on a server. You can access it on the PS4 by either;

1) Fake DNS spoofing to redirect the manual page to the exploit page, or

2) Using the web browser to navigate to the exploit page (not always possible).



Vulnerability Credit
==============

I wrote the exploit however I did not find the vulnerability, as mentioned above the bug (CVE-2018-4441) was found by lokihardt from Google Project Zero (p0) and was disclosed via the Chromium public bug tracker.



Resources
==============

[Chromium Bug Report](https://bugs.chromium.org/p/project-zero/issues/detail?id=1685&desc=2) - The vulnerability.

[Phrack: Attacking JavaScript Engines by saelo](http://www.phrack.org/papers/attacking_javascript_engines.html) - A life saver. Exploiting this would have been about 1500x more difficult without this divine paper.



Thanks
==============

lokihardt - The vulnerability
qwertyoruiop - WebKit School
saelo - Phrack paper