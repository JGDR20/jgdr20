---
layout: page
title: Portfolios
sidebar: No
asset: "/assets/Portfolios/"
---
Below you will find a selection of photos arranged into portfolios.

<div>
{%- for portfolio in site.data.Portfolios.portfolios -%}
	{%- assign rel_url = page.asset | append: portfolio.image -%}
	<!-- {{ portfolio | inspect }} -->
	  {%- include portfolio_cover.html title=portfolio.title url=rel_url description=portfolio.description page=portfolio.page -%}
{%- endfor -%}
</div>