# LaptopOpenVideoHardwareStub

This is an experimental attempt of development crowdfunding and crowdsourcing.
Is it possible to make such a complicated thing from scratch?
Time will show...

The device itself is an extension card for laptops which lack a dedicated GPU chip. Due to restricted space, it supports widely different options. In most cases, one or more options will work.
1) ExpressCard/34. Narrow space, external RAM can't be installed.
2) ExpressCard/54. Wide space, so an additional SODIMM can be installed.
I love the idea of using cheap upgrade leftovers for those cards. When one replaces a 2Gb module with a 8Gb, the 2Gb SODIMM can be used as a VRAM. It's too few for the system RAM but for a VRAM extension (plus the minimal 1Gb or 2Gb already soldered on the video card) it's very decent size.
3) With some luck, it can be installed in a miniPCIe slot. Sometimes, it's mounted nearly a big free space such as useless CD bay. Aside placing the whole card inside a laptop, it also allows to free the PCIe x1 from sending the rendered picture back to the mainboard: there are USB pins on a miniPCIe slot which can be used for a virtual "video input device".
4) Options are not mutually exclusive: the most powerful configuration is placing it into an ExpressCard slot, and also connecting it to a miniPCIe slot by a twisted pair extension cord. It'll allow to use PCIe x2 and USB 2.0.

Speaking about a GPU itself... I'm not certain on a chip. Fun fact: hi-end Cherry Trail looks very suitable, it has USB, exactly two PCIe lanes, it's GPU core is relatively OK (and can use some software help from CPU cores), and it can work with DDR, not GDDR. If nothing else works, we have a Plan B: use Cherry Trail.

About Red Area (parts which don't fit into ExpressCard slot and protrude outside). They can have a miniHDMI connector, it allows to use an external display without sending the rendered picture via PCIe x1, which is already overloaded (big SODIMM allows to put all textures into VRAM and forget about them, so I hope it'll not slow things down *too* much, at least, unless software refreshes the scene too often or generate dynamic textures on CPU, not GPU).

Speaking about power... Either ExpressCard or miniPCIe can be far from enough. Even options 1 and 2 may require an additional adapter cable from protruding miniPCIe to laptop USBs (aside from having 5V, 1A from each USB it also allows to free PCIe x1 from returning GPU output, so if power problems make that cable mandatory, it's probably no reason to support rendering back to PCIe: the card won't work without additional USB either way, so let's have the output only on USB pins).

Fun fact 1: such cables exist and are used for connecting "pins-34-to-40-only" mini-cards externally (yes, some cards use only USB pins of the miniPCIe connector, not the PCIe itself).

Fun fact 2: USB cable can be connected to *another* laptop, allowing to post-process (or grab) the output on a "cluster" of two PCs. Option 4 will however require a splitter cable for this (miniPCIe goes inside the laptop, USB goes to another one).

If, with some luck (or with making a jigsaw hole in the lower cover), we have the third option, additional power can be taken from SATA. Removing a SATA CD (if any) and using it's connector is obvious, but HDD can also work: replacing it with SSD releases some amperage which was used for starting it's spindle before. So the cable should look like a 2.5" to 1.8" SATA adapter with a pair of long wires, connecting it's +5 and GND to a small connector on the video card.

The SODIMM slot can be a small problem: most of them are mezzanine, the option we don't have because of the insanely flat ExpressCard slot (seriously, who designed this idiotic form factor? A single PCIe where's a place for four, too thick for having two and too thin for having one... no wonder it's almost dead. How about a thicker PCIe x4 or x8 version?) Either way, we have to use some sort of exotic non-mezzanine SODIMM connectors which connect boards edge-to-edge. Also, having two SODIMM locks breaks the compatibility with /34 slot, so only one (in Red Area) can be used. Good thing the SODIMM itself have no space to escape inside a /54 slot, it's secured in the connector by the slot wall, but option 3 may require a stripe of duct tape. Another problem is the lower miniPCIe screw which can't be used at all (it's right inside the SODIMM), but may also require additional insulation from it's nut. Maybe with some extreme luck it's possible to have this screw *under* the SODIMM, to fix the long card better.

Speaking about the PCB itself… it's sadistically dense, even for SoC such as Cherry Trail. For a general-purpose GPU, it's nearly impossible. Maybe mobile GPU will help?
