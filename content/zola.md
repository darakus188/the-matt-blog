+++
title = "Using Zola for a Personal Blog"
date = 2019-07-04

[taxonomies]
categories = ["Technology"]
tags = ["blog", "zola", "rust"]
+++

When I set out to create a personal blog I had only a vague idea of what the
possibilities were. I had heard about this fancy wizbang content management
system called [Wordpress](https://wordpress.org/). After taking a look at
Wordpress I was fairly impressed. There is quite an ecosystem of themes and
plugins which allows for nearly limitless design flexibility. Wordpress enjoys
many web hosts who cater specifically to its user base. For a modest fee, these
companies will do most of the _not so hard_ work of getting an instance of
Wordpress running on a web server. It was all rather slick and easy. So of
course I decided that was certainly not the direction for me to take.

Instead I wanted something a bit more manual, but still with some of the
convenient features. Something simple, spartan even, but still powerful. I
didn't care about monetization. I didn't want any distracting animations. I
definitely have no interest in tracking<sup>1</sup> anyone who visits my blog.
Open source is a must. And it would be really sweet if I could use
[Rust](https://www.rust-lang.org/) would it not?

After thinking about the features and anti-features that I wanted and doing
some Googling I decided to try out [Zola](https://www.getzola.org/).
Zola is a static site generator written in Rust that takes articles written in
markdown and spits out a website. There is a bit more to it than that, but that
is the main idea. Zola has many features but also is still in active
development which is right up my alley. I almost chose Hugo over Zola because
Hugo supports asciidoc but the allure of Rust was too much. I may even try to
implement support for asciidoc into Zola because it is the format that the
[Fedora Docs](https://docs.fedoraproject.org) team uses and learning it might
make contributing there easier in the future. And what better way to learn
something than to implement it in code.

Zola, and static site generators in general, are pretty cool little programs.
They are like a middle ground between coding the site entirely by hand and
using a full blown CMS like Wordpress. Zola uses a powerful template engine
that allows you to use programming features like iteration, conditionals and
variables to construct html pages. It then reads the content directory, which is just a bunch of markdown files, each containing an article. With the template and the content in hand, Zola smashes them together into a website. There is even the option to host the site right from the Zola binary. The developers of Zola claim that due to the static nature of the generated websites, Zola sites are exceptionally fast and scalable since they don't require server side processing or database queries. My site seems lightning fast so at this point I have no reason to be skeptical about that and see no technical reasons why it wouldn't scale to many, many users.

I decided to start out with a very spartan theme. It is just a wall of article
links and the option to filter by categories or tags. I wanted to create a
distraction free experience and I think I've already accomplished that. One
thing I personally don't like about my theme is that it is a light theme.
However I know there are many people who do like light themes so I've decided
to leave it light and dark theme users can use
[DarkReader](https://addons.mozilla.org/en-US/firefox/addon/darkreader/) or
something like that. I might change that in the future.

While I know the basics of markdown, I have not worked with it extensively in
the past. I tried two different markdown editors while setting up this blog.
First was called Marker. It is a GTK3+ application that would normally fit in
very well on a [Fedora Workstation](https://getfedora.org/). However I use the
Adwaita Dark theme for GNOME and Marker had some visual problems using that. I
then tried UberWriter, also a GTK3+ application. UberWriter worked quite
beautifully. It was elegant and hid its user interface while I was writing. It
was a very good experience. But being the nerd that I am I soon began longing
for the familiarity of Vim. After some fiddling around, I was able to setup Vim
to be an excellent companion to writing prose. Vim supports markdown syntax
highlighting out of the box, but to really make it a better markdown editor you
need to change some of the defaults for markdown files. This is what I added to
my `~/.vimrc`:
```vim
    autocmd FileType markdown setlocal wrap 
    autocmd FileType markdown setlocal linebreak
    autocmd FileType markdown setlocal spell
    autocmd FileType markdown setlocal tw=0
    autocmd FileType markdown setlocal wrapmargin=0
```
This gave me access to spell checking and automatic word wrapping. I never knew
that Vim had a built in spell checker until today and I have been using it for
years.

From this simple starting point I will hopefully create a habit of writing and
sharing my experiences with the internet. To humble beginning. Cheers.
<br>
<br>
<sup>1</sup> There is a javascript library that is used currently in my blog
that is used to create a fancy slide out menu on mobile devices. The library
comes from _cdnjs.cloudflare.com_ and it gets flagged as a potential tracker by the excellent Firefox add-on [Privacy Badger](https://www.eff.org/privacybadger/faq). This was part of the template I am using and will work to fix this in the near future.
