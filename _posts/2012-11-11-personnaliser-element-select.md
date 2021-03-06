---
layout: post
title: Personnaliser un &eacute;l&eacute;ment &lt;select&gt;
published_date: 11 Novembre 2012
---
<section class="MainText">
<div class="Warning">
<p><strong>EDIT 25/04/2013</strong>: J’ai créé un tout petit javascript plugin pour mettre en place cette technique plus facilement : <a class="Link" href="https://github.com/jeromesmadja/selectyle">Selectyle sur Github</a></p>
</div>

<p>La problématique en question est d’avoir un <code class="Code">&lt;select&gt;</code> aux couleurs de la charte graphique du site,
<strong>tout en gardant l’élément accessible</strong> (touch events, navigation par clavier, souris, …).</p>

<p>On ne modifiera pas la manière dont un élément <code class="Code">&lt;select&gt;</code>  se comporte. Il y a beaucoup trop d’appareils de nos jours à prendre en compte, laptops, tablettes, smartphones, TVs, donc ne réinventons pas la roue et <strong>laissons le navigateur et l’OS faire leur travail</strong>.</p>

<p>Juste quelques lignes de jQuery suffiront, à accomplir tout ça.</p>

<h2 id="tldr">TL;DR</h2>

<p><a class="Link" href="http://codepen.io/jeromesmadja/pen/gGIAf">Essayer la démo</a></p>

<h3 id="html">HTML</h3>

<p>On ajoute juste container et un <code class="Code">&lt;span&gt;</code> qui servira de placeholder pour le texte.</p>
</section>
<section class="CodeContainer">
<pre class="language-html"><code>&lt;div class="select-wrapper"&gt;
    &lt;span class="select-text"&gt;&lt;/span&gt;&lt;!-- Placeholder that will update on the select change event --&gt;
    &lt;select&gt;
            &lt;option&gt;France&lt;/option&gt;
            &lt;option&gt;Australia&lt;/option&gt;
            &lt;option&gt;Canada&lt;/option&gt;
            &lt;option&gt;USA&lt;/option&gt;
    &lt;/select&gt;
&lt;/div&gt;
</code></pre>
</section>
<section class="MainText">
<h3 id="jquery">jQuery</h3>

<p>Ok, juste quelques lignes de jQuery pour mettre à jour le <code class="Code">&lt;span&gt;</code>.</p>
</section>
<section class="CodeContainer">
<pre  class="language-javscript"><code>$(function() {

   // Get select elements
   var selects = $('.select-wrapper &gt; select');
   if ( !selects.length ) return;

   // Add change event to selects, and trigger it on load, so the span is up to date
   selects.on('change', function() {

       var select = $(this),
           placeholder = select.prev('.select-text');

       if ( !placeholder.length ) return;

       // Get the text from the selected option
       var selected_text = select.children('option:selected').text();

       // Update the placeholder text
       placeholder.html(selected_text);

   }).change();

   // Workaround for Firefox that doesn't trigger the change event,
   // if the user is using the keyboard to navigate through the options
   selects.on('keypress', function() {
       selects.trigger('change');
   });
});
</code></pre>
</section>
<section class="MainText">
<h3 id="css">CSS</h3>

<p>La principale ligne à regarder ici est l’opacité. En gros, l’opacité du <code class="Code">&lt;select&gt;</code> est 0 ce qui le rend invisible pour l’utilisateur, mais restant au-dessus du <code class="Code">&lt;span&gt;</code>, le navigateur va alors déclencher le click event sur le <code class="Code">&lt;select&gt;</code>.</p>
</section>
<section class="CodeContainer">
<pre class="language-css"><code>.select-wrapper {
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    border-radius: 5px;
     /* Replace the url with your own arrow image here */
    background: url('http://i50.tinypic.com/294gl55.png') no-repeat right center #e3e8f2;
    border: 1px solid #238db1;
    display: inline-block;
    font-family: Arial, Helvetica, sans-serif;
    height: 32px;
    min-width: 150px;
    padding-right: 35px;
    position: relative;

   /** If width is set you'll need overflow: hidden, otherwise it's optional **/
   overflow: hidden;
   width: 200px;

}

.select-text {
    border-right: 1px solid #ccc;
    color: #238db1;
    display: block;
    font-size: 14px;
    height: 32px;
    line-height: 32px;
    padding: 0 0 0 5px;
    width: 100%;
}

select {
    /* Position the select over the span*/
    height: 32px;
    left: 0;
    position: absolute;
    top: 0;
    width: 100%;

    /*  The select is now positioned over the span
     *  We don't want to see the select, but we want to be able to click on it
     *  So we set the opacity to 0
     */
    -ms-filter:"progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
    -moz-opacity: 0;
    -khtml-opacity: 0;
    -webkit-opacity: 0;
    opacity: 0;
}
</code></pre>
</section>
<section class="MainText">
<p>Testé sous Windows 7 sur Chrome, Firefox, Safari, Interner Explorer 8+ (désolé pas de support pour IE7), et Safari pour iPad sous iOS 6.</p>
</section>