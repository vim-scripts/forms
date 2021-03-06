# Events 

There are two types of input events. The first type is simply the
Character or Number returned by calling Vim's 'getchar()' function.
These are called "character" events. A Viewer examine all such
character events and map some of them into the second type of event
supported by Forms, a Dictionary object or object event that must
have a 'type' key whose value is one of a set of allowed names. As an
example, a &lt;LeftMouse> Number returned by 'getchar()' is converted
into a Dictionary event object of type 'NewFocus' where the Dictionary
also has entries for the line and column (v:mouse_line and
v:mouse_column). Some object events are generated by the Forms
runtime system. An example of such an object event is a ReSize event
which is generated when a Glyph actually changes its size (normally, a
very rare occurrence). More common runtime generated event objects are
the Cancel and Submit events which are, generally, generated by a
"close" and an "accept" button.

A user interacts with a form by sending events to the form. Such 
events are generated by the keyboard and mouse. Events are
read by the Viewer with current focus, mapped by that Viewer
and then forwarded to the Viewer's child Glyph that has focus.
If a Viewer has no child Glyphs that accept focus, then all events
are ignored (except the &lt;Esc> character which will pop the user out
of the current Viewer/Form).

If a Glyph consumes an input event, it might require redrawing (such
as adding a character to an editor). In this case, the Glyph registers
itself with a viewer-redraw-list and when control is returned to the
Viewer, the Glyph is redrawn.

Sometimes a Glyph will consume an event and an action will be
triggered. For example, a left mouse click or keyboard &lt;CR> entry
on a button will cause the action associated with that button to
execute.

## Limitations

### New Types of Events

A developer can create a new Event type, but the Glyphs that handle Events
will simply ignore it.

Currently, any Glyph that handles Events does so in one of its methods;
there is no notion of a pluggable, independent Event Handler.

### Representation

I've thought about but never reached a design that I was happy with
concerning the scripting of a Form interaction. That is, add Events
to the Input Queue and then call a Form's 'run()' method.
The issue is that of representing the Number and String that are
returned by 'getchar()' and 'nr2char()' methods. And, in addition,
some events have secondary data such as Mouse Events.

A companion problem is how to log and capture a user's 'getchar()' Character 
stream in some format that would allow for editing the resultant 
captured data and replay.

In both cases, I do not see a solution short of enumerating all possible
Character/Event types.

I might add, a good Character/Event capture/edit/replay capability would
allow for creating a suit of non-visual unit tests for the Forms library. 

