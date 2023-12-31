# ap-el-kb

A mobile keyboard for APL

Visit https://turtleyacht.github.io/ap-el-kb.github.io/ to check it out.

Inspired by pushfoo and 082349872349872 discussion in _The Cognitive Style of
Unix_ (2010) on Hacker News.

## Features

Type in APL on mobile. Receive info about the symbol. Through play,
learn passively various ideas and operations that could filter into your daily
(other) programming languages. Spark curiosity.

Have a place for puzzles, poetry, reference, and general info.

Before we can implement features, we need to be able to type out APL symbols.

## Keyboard Setup (Development Only)

Setup depends on the operating system and defaults. We use OpenBSD on a Unicomp
Classic 101 keyboard (US), which does not have Windows or Menu keys.

Unicode (UTF-8) needs to be enabled. This is required, since the APL symbols are
not accessible by default, and we need additional symbols.

International characters can be produced with the Compose key. We might as well
enable accented letters, currency symbols, and all the diverse characters
outside the ASCII set. Users can type both APL symbols and existing alphabets
without foregoing the latter.

Typing APL should be a "mode" that can be toggled. This sustains flow during
coding.

Combining compose key with mode switch maximizes flexibility. We will likely
need to map (one, mostly two) APL symbols to each letter, number, and special
character on the keyboard--and there are more APL symbols than keys! So, to get
the most allocation up front, we will employ both composition and single-key
(and Shift) techniques.

Of course, after all that, you'll know enough to set up your own preferred way.
That's likely best for a programming language like APL.

For our purposes, we will modify a few files. First, set UTF-8 in ~/.profile:

    export LC_CTYPE=en_US.UTF-8

Next, add a line to ~/.Xdefaults:

    XTerm*locale:utf8

We will take the system default's xinitrc to modify it:

    cp /etc/X11/xinit/xinitrc ~/.xinitrc

The file is a script that is run with `startx`. Near the end of the file, we add
these options:

    setxkbmap -option lv3:ralt_switch_multikey
    setxkbmap -option lv3:caps_switch_latch
    setxkbmap -option grp:ctrl_alt_toggle
    xmodmap -e "keycode 38 = a A U237a question"

Admittedly, this only touches the surface of starting out with APL. We only
assigned one APL symbol (alpha), and use `?` for testing purposes. Further work
will be done here to separate the keycode mapping elsewhere for tidiness.

Because we take over Left Control + Left Alt, we can't cycle consoles with the
Function keys (F1-F5, for example). This seems okay since we shouldn't be
switching to root too often.

Right Alt + Caps Lock is an ISO_Level_3_Shift, which is an escape route in case
of needing additional Unicode characters (maybe?).

## Verifying APL Symbol Setup

Here are a few test cases to try after launching with `startx`; first, we verify
Compose key works:

1. Press Shift and Right Alt.
2. Type backtick (grave) key.
3. Type `a`

The terminal should produce accented a.

Next, we verify mode switching is enabled:

1. Run `xev -event keyboard`
2. Move pointer to the small window that pops up.
3. Press and hold Left Control.
4. With Left Control pressed, press and hold Left Alt.

This should output "... keycode 64... ISO_Next_Group..."

Now, we can verify one APL symbol and its Shift-assigned keys work:

1. Press Left Control and Left Alt together, and release.
2. Type `a`
3. Type `a` with Shift.
4. Repeat steps 1-3.

Output should be alpha, question mark, lowercase a, and uppercase A.

The test cases may seem extraneous, but the last two are techniques for a
development feedback loop to debug key mappings.

## Pattern Notes

Reviewing the Unicode characters for APL, the modifiers look to be quad, stile,
underbar, tilde, diaresis (double-quote), macron (overbar), circle, slash,
backslash, jot, minus, and down tack. (More details to be shared in a table
later, once key mappings are complete.)

