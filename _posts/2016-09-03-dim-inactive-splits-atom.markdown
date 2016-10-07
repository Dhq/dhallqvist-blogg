---
layout: post
title:  "Dim/fade inactive splits in Atom"
date:   2016-9-3
---
![Demo of dimming inactive pane](/assets/img/post-images/2016-09-03-dim-atom.gif)

I am a fan of both Vim and Atom. And even though I currently almost only use Vim for developing, I like how easy it is
to write packages etc for Atom. One feature that I like with my Vim setup is how that the inactive splits/windows get a fade.
This makes it clear(er) which split that is currently active. Depending on what theme you use in Atom, this can be more or less obvious there as well.
If you feel that it is __less__ obvious, maybe this can help you:

```scss
.pane:not(.active) {
  opacity: 0.5;
}
```

Obviously, with as much or less opacity as you like. To quickly edit the atom stylesheet, run the `Application: Open your stylesheet` command in the command pallette (cmd/ctrl + shift + p).



