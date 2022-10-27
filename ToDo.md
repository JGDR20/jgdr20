---
layout: page
title: ToDo
# permalink: /todo/index.html
sidebar: No
---
* [x] Create Category landing pages with filtering
* [x] Add Grandad's site
  * [x] Fix image paths in ^
  * [ ] Cleanup empty .htm files
  * [x] Recreate as parallel MD site
  * [ ] Move/copy model dimensions to near top of page for uniformity
  * [ ] Find unlinked pages and recreate as MD docs
* [ ] Implement photo galleries, possibly lightbox.js? Maybe something with no script or plugin
  * [x] Done with Flexbox CSS
  * [ ] Needs further work to prevent last element from being too wide, maybe set hard dimensions?
* [ ] Bring over blog posts from WP site
* [ ] Create print-friendly alt css for resumé
  * Added divs around blocks that should be kept together on pages (e.g. About Me, Education etc...)
  * [ ] page-break-inside: avoid; doesn't appear to work with flex-box  
  [SO Question 1][SO_Page_Break_1]  
  [SO Question 2][SO_Page_Break_2]  
  [Kenan Yusuf's Flexbox without Flexbox][Kenan_Yusuf]
  * [ ] Extend print-friendly css to whole site
* [x] Contact form
  * [ ] Change validation to be more sensible
* [x] Restructure blog categories with hierarchy?
* [x] Look at calculating duration of current job on page load
 * Javascript to rewrite blank element?

[SO_Page_Break_1]: https://stackoverflow.com/questions/34534231/page-break-insideavoid-not-working
[SO_Page_Break_2]: https://stackoverflow.com/questions/20408033/how-to-get-page-break-inside-avoid-to-work-nicely-with-flex-wrap-wrap
[Kenan_Yusuf]: https://kyusuf.com/post/almost-complete-guide-to-flexbox-without-flexbox/