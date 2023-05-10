# Basic Chinese support
[Localization/Chinese](https://wiki.archlinux.org/title/Localization/Chinese)  

[Localization/Simplified Chinese](https://wiki.archlinux.org/title/Localization/Simplified_Chinese?rdfrom=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%3Ftitle%3DLocalization%2FSimplified_Chinese_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29%26redirect%3Dno)


> To properly display Chinese, you must set the `locale` correctly and install the appropriate Chinese fonts.


## 1. locale settings
It is recommended to use UTF-8 locale. You need to modify /etc/locale.gen to set the locales that can be used in the system (erase the comment symbol "#" before the corresponding item):
```
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```