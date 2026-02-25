---
layout: page
title: Blogs
permalink: /blogs/
---

<div class="max-w-5xl mx-auto px-6 py-12">
  <h1 class="text-4xl font-bold mb-8">Blog</h1>
  <p class="text-gray-400 mb-12">Thoughts, experiments, and lessons learned while building automations and agents.</p>

  <div class="space-y-8">
    {% for post in site.posts %}
      <article class="bg-[#1a1a1a] rounded-2xl p-6 border border-white/5 hover:border-white/10 transition-colors">
        <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
          <div>
            <h2 class="text-2xl font-semibold mb-2">
              <a href="{{ post.url }}" class="hover:text-primary transition-colors">{{ post.title }}</a>
            </h2>
            <div class="flex items-center gap-4 text-gray-400 text-sm mb-4">
              <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y-%m-%d" }}</time>
              {% if post.categories %}
                <span>·</span>
                <div class="flex gap-2">
                  {% for category in post.categories %}
                    <span class="px-3 py-1 bg-white/5 rounded-full text-xs">{{ category }}</span>
                  {% endfor %}
                </div>
              {% endif %}
            </div>
            <p class="text-gray-300">{{ post.excerpt | strip_html | truncate: 200 }}</p>
          </div>
          <a href="{{ post.url }}" class="inline-flex items-center gap-2 text-primary hover:text-blue-400 transition-colors whitespace-nowrap">
            Read more
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"></path></svg>
          </a>
        </div>
      </article>
    {% else %}
      <p class="text-gray-400 text-center py-12">No posts yet. Check back soon!</p>
    {% endfor %}
  </div>
</div>