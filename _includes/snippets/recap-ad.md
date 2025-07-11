{% capture /dev/null %}

<!--
recap-ad.md: creates an advertisement for the next recap
-->

  Input:
    - when: UTC time of next recap in any format supported by the `date`
      filter, e.g. YYYY-MM-DD HH:MM.  Cannot be earlier than the
      page.date

  Output:
    A div with a paragraph announcing the next recap and how to
    participate in it.

  Example:
    Input:
      % include linkers/issues.md issues="123,456" %
    Output
      ... at HOUR on DAY ...
-->
{% assign current_epoch_time = site.time | date: "%s" %}
{% assign when_epoch_time = include.when | date: "%s" %}
{% assign page_epoch_time = page.date | date: "%s" %}
{% if page_epoch_time > when_epoch_time %}{% include ERROR_RECAP_EARLIER_THAN_PUBLICATION_DATE %}{% endif %}
{% assign recap_formatted_date_utc = include.when | date: "%B %-d" %}
{% assign recap_formatted_time_utc = include.when | date: "%H:%M" %}
{% endcapture %}{% capture recap_ad_text %}
<div markdown="1" class="callout">

{% case page.lang %}
{% when "cs" %}

{% assign recap_formatted_date_utc = include.when | date: "%-d. %-m." %}

## Chcete víc?

Další diskuze o tématech zmíněných v tomto zpravodaji proběhnou v týdenním
Bitcoin Optech Recap na [Riverside.fm][] dne {{recap_formatted_date_utc}}
v {{recap_formatted_time_utc}} UTC. Diskuze jsou nahrávány a zpřístupněny
na stránce našeho [podcastu][podcast].
{% when "fr" %}

## Vous en voulez plus?

Pour plus de discussions sur les sujets mentionnés dans ce bulletin,
rejoignez-nous pour le récapitulatif hebdomadaire Bitcoin Optech sur
[Riverside.fm][] à {{recap_formatted_time_utc}} UTC le jeudi (le jour suivant la
publication de la newsletter). La discussion est également enregistrée et sera
disponible sur notre page de [podcasts][podcast].
{% when "ja" %}

## もっと知りたいですか？

このニュースレターで言及されたトピックについてもっと議論したい方は、
{{recap_formatted_time_utc}} UTCに
[Riverside.fm][]で毎週配信されているBitcoin Optech Recapにご参加ください。
この議論は録画もされ、[ポッドキャスト][podcast]ページからご覧いただけます。

{% when "zh" %}

## 想了解更多？

想了解更多本周报中提到的内容，请加入我们每周的比特币 Optech 回顾的
[Riverside.fm][]，时间为每周四 {{recap_formatted_time_utc}} UTC（即周报发布后的
一天）。讨论内容会被录制下来，也将在我们的[播客][podcast]页面上提供。

{% else %}
## Want more?

For more discussion about the topics mentioned in this newsletter, join us for
the weekly Bitcoin Optech Recap on [Riverside.fm][] at
{{recap_formatted_time_utc}} UTC on {{recap_formatted_date_utc}}.  The
discussion is also recorded and will be available from our [podcasts][podcast]
page.
{% endcase %}

</div>
{% endcapture %}
{% if when_epoch_time > current_epoch_time %}{{recap_ad_text}}{% endif %}

[Riverside.fm]: https://riverside.fm/studio/bitcoin-optech
