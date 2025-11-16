---
date: {{date}}
tags: 
authors: {{authors}}

Abstract:  {{abstractNote}}
---
### {{title}}
{{pdfZoteroLink}}

### Notes
{% for annotation in annotations -%}
  {%- if annotation.annotatedText -%}
    {%- if "Red" in annotation.colorCategory %}
# {{ annotation.annotatedText | escape }}
([{{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey }}?page={{ annotation.page }}&annotation={{ annotation.id }}))

    {%- elif "Purple" in annotation.colorCategory %}
## {{ annotation.annotatedText | escape }}
([{{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey }}?page={{ annotation.page }}&annotation={{ annotation.id }}))

    {%- elif "Blue" in annotation.colorCategory %}
### {{ annotation.annotatedText | escape }}
([{{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey }}?page={{ annotation.page }}&annotation={{ annotation.id }}))

    {%- elif "Orange" in annotation.colorCategory %}
#### {{ annotation.annotatedText | escape }}
([{{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey }}?page={{ annotation.page }}&annotation={{ annotation.id }}))

    {%- elif "Green" in annotation.colorCategory %}
##### {{ annotation.annotatedText | escape }}
([{{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey }}?page={{ annotation.page }}&annotation={{ annotation.id }}))

    {%- elif "Yellow" in annotation.colorCategory %}
###### {{ annotation.annotatedText | escape }}
([{{ annotation.page }}](zotero://open-pdf/library/items/{{ annotation.attachment.itemKey }}?page={{ annotation.page }}&annotation={{ annotation.id }}))

    {%- elif "Gray" in annotation.colorCategory %}
{{ annotation.annotatedText | escape }}

    {%- else %}
{{ annotation.annotatedText | escape }}

    {%- endif %}
  {%- endif %}

  {%- if annotation.imageRelativePath %}
![[{{ annotation.imageRelativePath }}]]
  {%- endif %}

{%- if annotation.comment %}
  {%- if "Red" in annotation.colorCategory %}
<div class="custom-comment" style="border-left: 4px solid #ff0000; background-color: #fff0f0;">
  <p class="comment-label" style="color: #ff0000;">Мой комментарий:</p>
  <p class="comment-text">{{ annotation.comment }}</p>
</div>
  {%- elif "Purple" in annotation.colorCategory %}
<div class="custom-comment" style="border-left: 4px solid #800080; background-color: #f8f0ff;">
  <p class="comment-label" style="color: #800080;">Мой комментарий:</p>
  <p class="comment-text">{{ annotation.comment }}</p>
</div>
  {%- elif "Blue" in annotation.colorCategory %}
<div class="custom-comment" style="border-left: 4px solid #0000ff; background-color: #f0f8ff;">
  <p class="comment-label" style="color: #0000ff;">Мой комментарий:</p>
  <p class="comment-text">{{ annotation.comment }}</p>
</div>
  {%- elif "Orange" in annotation.colorCategory %}
<div class="custom-comment" style="border-left: 4px solid #ffa500; background-color: #fffaf0;">
  <p class="comment-label" style="color: #ffa500;">Мой комментарий:</p>
  <p class="comment-text">{{ annotation.comment }}</p>
</div>
  {%- elif "Green" in annotation.colorCategory %}
<div class="custom-comment" style="border-left: 4px solid #008000; background-color: #f0fff0;">
  <p class="comment-label" style="color: #008000;">Мой комментарий:</p>
  <p class="comment-text">{{ annotation.comment }}</p>
</div>
  {%- elif "Yellow" in annotation.colorCategory %}
<div class="custom-comment" style="border-left: 4px solid #ffff00; background-color: #fffff0;">
  <p class="comment-label" style="color: #ffff00;">Мой комментарий:</p>
  <p class="comment-text">{{ annotation.comment }}</p>
</div>
  {%- elif "Gray" in annotation.colorCategory %}
{{ annotation.comment }}
  {%- else %}
<div class="custom-comment" style="border-left: 4px solid #cccccc; background-color: #f8f8f8;">
  <p class="comment-label" style="color: #666666;">Мой комментарий:</p>
  <p class="comment-text">{{ annotation.comment }}</p>
</div>
  {%- endif %}
{%- endif %}
{% endfor -%}