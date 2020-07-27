# dwm-scratchpad
Patch to enable a scratchpad feature in dwm as in i3wm.

## interface (dwm.c)
- **# define SCRATCHPAD_MASK (1u << sizeof tags / sizeof * tags)** - *macro that can be used in config.h*
- **static void scratchpad_hide ();** - *move selected window to scratchpad*
- **static void scratchpad_remove ();** - *remove selected window from scratchpad*
- **static void scratchpad_show ();** - *restore sequential window from scratchpad*

## default keybindings (config.def.h)
- **MODKEY**, **XK_minus** - *scratchpad_show*
- **MODKEY|ShiftMask**, **XK_minus** - *scratchpad_hide*
- **MODKEY**, **XK_equal** - *scratchpad_remove*

## start a window in scratchpad?
Try to add something like this in **rules** (config.h):
```c
{ NULL, NULL, "hidden", SCRATCHPAD_MASK, 0, -1 },
```

And launch something like this:
```
st -t hidden
```

## doesn't conflict with the namedscratchpads patch
namedscratchpads is a great scratchpad implementation, and this patch doesn't conflict with it. Why to use both this and the namedscratchpads patches? Because though they're both named 'scratchpad' patches, they do different things. This patch doesn't implement namedscratchpads' functionality to avoid bloating and keep simple.
