Making Infinite Jekyll
======================

* The infinite jekyll should use its own layout.

* Then the infinite jekyll will be called by using menu specific index.md which is

    ---
    layout: archive
    title: "Articles"
    date: 2014-05-30T11:39:03-04:00
    modified:
    excerpt: "A collection of thoughts, inspiration, mistakes, and other minutia."
    tags: []
    image:
      feature:
      teaser:
    ---
    
    <div class="tiles">
    {% for post in site.categories.articles %}
      {% include simple-post-grid.html %}
    {% endfor %}
    </div><!-- /.tiles -->

* Then I put the infinite jekyll specific 

    {% for post in site.posts limit: 10 %}	
    <div class="infinite-spinner"></div>
    <script src="{{ site.url }}/js/plugins/infinite-jekyll.js"></script>


    These will be inserted in the infinite-jekyll layout. 

* Mathjax Async update.

    Modify the infinite-jekyll.js to asynchrounously reload MathJax. a

    <http://docs.mathjax.org/en/latest/typeset.html>

    var math = document.getElementById("MathExample");
    MathJax.Hub.Queue(["Typeset",MathJax.Hub,math]);
