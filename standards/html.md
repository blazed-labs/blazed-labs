# Blazed HTML Standards
1. Semantic HTML
2. Schema.org Microdata Linking


## Semantic HTML
With semantic HTML comes purpose-based rules for when to use what. In these standards, a common HTML body should look like:
```html
<body>
 <header>
  <nav> ... </nav>
 </header>
 <main>
  ...
 </main>
 <section>
  ...
 </section>
 <footer>
  ...
 </footer>
</body>
```
Whereas:
* &lt;section>, &lt;header>, &lt;main>, &lt;footer> must NEVER be used in any place other than the "root" of the body.
* &lt;section> should wrap components outside of the main element.
* &lt;main> should wrap each page's main content.
* &lt;article> may be used to denote a independent, self-contained content. This could be a forum post, a magazine or newspaper article, a blog entry, a user-submitted comment, an interactive widget or gadget, or any other independent item of content[?](https://html.spec.whatwg.org/multipage/sections.html#the-article-element).
* &lt;aside> should be used to denote sidebars.

> Data should be properly wrapped

* &lt;summary> should be used to denote a summary or abbridgement of greater text, usually present in "browse" lists.
* &lt;time> should wrap any date/time, to provide a timestamp against pure text. This helps with web-crawlers and screen readers.
* &lt;figure> should wrap a reference image/component/etc.

## Schema.org Microdata Linking
Schema for certain elements can be defined with schema tags and a schema template. This data provides more context about the particular code, making more readable and maintainable.
- [Getting Started with Schema.org](https://schema.org/docs/gs.html)

ex:
```html
<div itemscope itemtype="https://schema.org/OfferCatalog" class="grid grid-cols-1 md:grid-cols-3 gap-5 px-5 lg:px-10 select-none">
    <meta itemprop="name" content="Blazed Labs Services" />
    <meta itemprop="description" content="Services offered by Blazed Labs LLC." />
    <meta itemprop="identifier" content="43ddcabc-0bd1-400b-b4e1-748b3cd37c2a"/>
    <article *ngFor="let service of services$ | async" itemprop="itemListElement" itemscope itemtype="https://schema.org/Service">
        <meta itemprop="serviceType" content="{{ service.name }}" />
        <meta itemprop="logo" content="{{ service.logo }}?w=350&h=350" />
        <meta itemprop="url" content="{{ service.website }}" />
        <span class="sr-only" itemprop="provider" itemscope itemtype="https://schema.org/LocalBusiness">
            <span class="indicator-item badge badge-primary" itemprop="name">
                {{ service.brand }}
            </span>
        </span>
        <a title="Visit {{ service.brand }}" class="group inline-flex" href="{{ service.website }}">
            <div class="inline-flex card min-w-96 max-h-96 bg-base-100 shadow-lg group-hover:shadow-xl">
                <figure itemprop="image">
                    <img src="{{ service.image }}" alt="{{ service.name }}" />
                </figure>
                <div class="card-body">
                    <h2 itemprop="serviceType" class="card-title group-hover:underline">
                        {{ service.name }}
                    </h2>
                    <p itemprop="description">
                        {{ service.description }}
                    </p>
                    <div class="card-actions justify-end">
                        <div class="badge badge-outline group-hover:badge-accent">
                            Learn More
                        </div> 
                    </div>
                </div>
            </div>
        </a>
    </article>
</div>
```