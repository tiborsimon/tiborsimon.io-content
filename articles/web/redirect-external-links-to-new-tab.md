Title: Redirect external links to new tab
Subtitle: Fix Markdown's lack of hyperlink targeting feature with javascript
Tags: pelican, hack, javascript
Date: 2015-05-17
Img: img/thumb-449x286-7.jpg
Summary: As Pelican can use Markdown to render it's contents, and Markdown doesn't support control over the hyperlink target attribute, you were forced to write your external links and references manually. Or not?


Hopefully, there is a simple javascript hack, that can redirect your external
links to a new blank tab.

All the credits go to [Austin](http://stackoverflow.com/users/1504966/austin)
and to his [stackoverflow post](http://stackoverflow.com/a/11597448). I am just
a happy user of this method :)

```
$('a').each(function() {
   var a = new RegExp('/' + window.location.host + '/');
   if(!a.test(this.href)) {
       $(this).click(function(event) {
           event.preventDefault();
           event.stopPropagation();
           window.open(this.href, '_blank');
       });
   }
});
```

The method just works. Nothing fancy about it, it does what is supposed to do.
Thank you Austin!
