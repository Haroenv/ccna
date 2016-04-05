---
layout: default
---
{::options parse_block_html="true" /}
<div class="aside">
<div class="aside-content">
* #markdown-toc
{:toc}

*[VLAN]: Virtual Local Area Network
*[IOS]: Internetwork Operating System
*[RIP]: Routing Information Protocol

# {{ site.title }}

By [Haroen Viaene](https://github.com/haroenv) and [Elias Meire](https://github.com/eliasmeire).

Fork this on [GitHub](https://github.com/haroenv/ccna-summary/blob/gh-pages/index.md) to add info.

</div>
</div>

{::options parse_block_html="true" /}
<div class="content">

{% assign chapters = site.chapters | sort: 'order' %}
{% for chapter in chapters %}
<!-- <section> -->

# {{chapter.order}}. {{chapter.title}}

{{chapter.content}}

<!-- </section> -->
{% endfor %}

# Footnotes

[^1]: <https://supportforums.cisco.com/document/31051/executing-show-commands-global-configuration-mode>
[^2]: <http://www.9tut.com/rip-routing-protocol-tutorial>
</div>
