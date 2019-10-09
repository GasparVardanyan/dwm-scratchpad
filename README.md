# dwm-scratchpad
Patch to enable a scratchpad feature in dwm as in i3wm.
## interface (dwm.c)
- **static void scratchpad_hide ();** - *move selected window to scratchpad*
- **static void scratchpad_remove ();** - *remove selected window from scratchpad*
- **static void scratchpad_show ();** - *restore sequential window from scratchpad*
## default keybindings (config.def.h)
- **MODKEY**, **XK_minus** - *scratchpad_show*
- **MODKEY|ShiftMask**, **XK_minus** - *scratchpad_hide*
- **MODKEY**, **XK_equal** - *scratchpad_remove*
## changes in keybindings (config.def.h)
```diff
-	{ MODKEY,                       XK_0,      view,           {.ui = ~0 } },
-	{ MODKEY|ShiftMask,             XK_0,      tag,            {.ui = ~0 } },
+	{ MODKEY,                       XK_0,      view,           {.ui = ~scratchpad_mask } },
+	{ MODKEY|ShiftMask,             XK_0,      tag,            {.ui = ~scratchpad_mask } },
```
## variables to be declared in config.h (config.def.h)
- **static const unsigned scratchpad_mask = 1u << sizeof tags / sizeof * tags;** - *scratchpad's tag mask*
## other functions (dwm.c)
- static _Bool scratchpad_last_showed_is_killed (void);
- static void scratchpad_show_client (Client * c);
- static void scratchpad_show_first (void);
## other variables (dwm.c)
- static Client * scratchpad_last_showed = NULL;
