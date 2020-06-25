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

## other functions (dwm.c)
- static _Bool scratchpad_last_showed_is_killed (void);
- static void scratchpad_show_client (Client * c);
- static void scratchpad_show_first (void);

## other changes in dwm.c
```diff
@@ -269,11 +275,15 @@ static Drw *drw;
 static Monitor *mons, *selmon;
 static Window root, wmcheckwin;

+/* scratchpad */
+# define SCRATCHPAD_MASK (1u << sizeof tags / sizeof * tags)
+static Client * scratchpad_last_showed = NULL;
+
 /* configuration, allows nested code to access above variables */
 #include "config.h"

 /* compile-time check if all tags fit into an unsigned int bit array. */
-struct NumTags { char limitexceeded[LENGTH(tags) > 31 ? -1 : 1]; };
+struct NumTags { char limitexceeded[LENGTH(tags) > 30 ? -1 : 1]; };

 /* function implementations */
 void
@@ -309,7 +319,8 @@ applyrules(Client *c)
 		XFree(ch.res_class);
 	if (ch.res_name)
 		XFree(ch.res_name);
-	c->tags = c->tags & TAGMASK ? c->tags & TAGMASK : c->mon->tagset[c->mon->seltags];
+	if (c->tags != SCRATCHPAD_MASK)
+		c->tags = c->tags & TAGMASK ? c->tags & TAGMASK : c->mon->tagset[c->mon->seltags];
 }

 int
```

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
