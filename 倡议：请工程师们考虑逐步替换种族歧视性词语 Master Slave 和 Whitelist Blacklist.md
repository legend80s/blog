> 谷歌决定放弃 Chrome 浏览器中“blacklist（黑名单）“、“whitelist（白名单）”的用法，后续使用“blocklist”和“allowlist”来替代它们。
>

master slave 这些词语对于遭受过贩卖和奴役的种族或人群来说每一次的阅读都是对其心灵的深层次打击，会让其联想到祖辈受过的奴役之苦。而黑白名单 whitelist blacklist 会让人下意识将“白好黑坏”联系到一起，相比 master slave 而言，其直观的心理暗示更让其具备种族优越性的负面教育和挑唆性。

Google、GitHub 等都在替换种族歧视单词的行动中，虽然国内程序员在使用 whitelist / blacklist 和 master / slave 等单词的时候不会潜意识将其和种族歧视联想到一起，但考虑到我们身处的互联网无分国界，我们所在公司的工程师也可能来自诸多国家，具备不同的文化背景，所以请工程师们从现在开始考虑将代码中 master slave 和 whitelist blacklist 等涉及种族歧视的词语渐进式替换成中性单词，虽然单纯替换这些令人反感的词语没法消除种族歧视的观念，但至少可以“让这个世界变得更友好”。我们最终的目标是让“master slave（主从）”和“blacklist whitelist（黑白名单）”等激进的表述从源码和交流表述中逐渐消失。

<p align="center">
  <img src="https://ata2-img.oss-cn-zhangjiakou.aliyuncs.com/f5604d09d4e6ea6c64b4237dc3c12687.png" alt="github 准备将 master 重命名成 main" style="zoom:50%;" width="50%" />
  <br>
  <a style="font-style: italic;" href="https://twitter.com/natfriedman/status/1271253144442253312?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1271253144442253312%7Ctwgr%5E&ref_url=https%3A%2F%2Fwww.techspot.com%2Fnews%2F85631-github-replace-terms-whitelist-blacklist-masterslave-racially-insensitive.html">github 准备将 master 重命名成 main</a>
</p>

微软、Github 已经表示正在火速下架所有相关词汇编程词汇，把 master 换成main（主要的），slave 换成secondary（次要的）。

当然不止 Google、微软、GitHub，Android Open Source Project (AOSP)、Go 语言、PHPUnit library, Curl 等公司或组织也表达了替换 whitelist blacklist 的意图。

## 可替换中性单词

master slave 可替换单词，来自网络和 [ietf](https://tools.ietf.org/id/draft-knodel-terminology-00.html#suggested-alternatives)

- primary secondary
- main secondary
- leader follower
- active standby
- primary replica
- writer reader
- coordinator worker
- parent helper

whitelist blacklist  可替换单词，来自网络和 [ietf](https://tools.ietf.org/id/draft-knodel-terminology-00.html#suggested-alternatives)

- allowlist denylist
- allowlist blocklist
- permit block

大家根据代码上下文选择其最合适的组合即可。

## 参考链接

- [谷歌也怕了，“blacklist”等表述逐渐从各大公司的源代码中消失！](https://mp.weixin.qq.com/s/NyUw9UNxOYmh4z9f5N16Ew)
- [Github says it will replace the terms whitelist, blacklist, and master/slave for being racially insensitive](https://www.techspot.com/news/85631-github-replace-terms-whitelist-blacklist-masterslave-racially-insensitive.html)
- [IETF - Terminology, Power and Oppressive Language draft-knodel-terminology-00](https://tools.ietf.org/id/draft-knodel-terminology-00.html)
