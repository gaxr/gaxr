rouge 高亮样式生成shell代码

```sh
for style in base16 base16.dark base16.monokai base16.monokai.light base16.solarized base16.solarized.dark colorful github gruvbox gruvbox.light molokai monokai monokai.sublime thankful_eyes
do
    rougify style $style > $style.css
done
```

本目录下数字分别对应的css名称

```text
 1 ==> base16.css
 2 ==> base16.dark.css
 3 ==> base16.monokai.css
 4 ==> base16.monokai.light.css
 5 ==> base16.solarized.css
 6 ==> base16.solarized.dark.css
 7 ==> colorful.css
 8 ==> github.css
 9 ==> gruvbox.css
10 ==> gruvbox.light.css
11 ==> molokai.css
12 ==> monokai.css
13 ==> monokai.sublime.css
14 ==> thankful_eyes.css
```