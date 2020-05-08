# dwm-scratchpad
Patch to enable a scratchpad feature in dwm as in i3wm.

## interface (dwm.c)
- **# define scratchpad_mask (1u << sizeof tags / sizeof * tags)** - *macro that can be used in config.h*
- **static void scratchpad_hide ();** - *move selected window to scratchpad*
- **static void scratchpad_remove ();** - *remove selected window from scratchpad*
- **static void scratchpad_show ();** - *restore sequential window from scratchpad*

## default keybindings (config.def.h)
- **MODKEY**, **XK_minus** - *scratchpad_show*
- **MODKEY|ShiftMask**, **XK_minus** - *scratchpad_hide*
- **MODKEY**, **XK_equal** - *scratchpad_remove*

## changes in dwm.c
```diff
 /* compile-time check if all tags fit into an unsigned int bit array. */
-struct NumTags { char limitexceeded[LENGTH(tags) > 31 ? -1 : 1]; };
+struct NumTags { char limitexceeded[LENGTH(tags) > 30 ? -1 : 1]; };

@@ -309,7 +319,7 @@ applyrules(Client *c)
 		XFree(ch.res_class);
 	if (ch.res_name)
 		XFree(ch.res_name);
-	c->tags = c->tags & TAGMASK ? c->tags & TAGMASK : c->mon->tagset[c->mon->seltags];
+	c->tags = c->tags & (TAGMASK | scratchpad_mask) ? c->tags & (TAGMASK | scratchpad_mask) : c->mon->tagset[c->mon->seltags];
 }

 int
```

## other functions (dwm.c)
- static _Bool scratchpad_last_showed_is_killed (void);
- static void scratchpad_show_client (Client * c);
- static void scratchpad_show_first (void);

## other variables (dwm.c)
- static Client * scratchpad_last_showed = NULL;

## start a window scratchpad?
Try to add something like this in **rules** (config.h):
```c
{ NULL, NULL, "hidden", scratchpad_mask, 0, -1 },
```

And launch something like this:
```
st -t hidden
```
