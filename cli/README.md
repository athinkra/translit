# Translit

```
usage: translit [-h] [-l] [-s SEARCH] [-i INPUT] [-t TRANSFORM] [-c CUSTOM] [-r RULES] [-rev]

Transliterate strings

Options:
  -h, --help                          Show this help message and exit
  -l, --list                          List all ICU supported transformations.
  -s SEARCH, --search SEARCH          Search supported transformations.
  -i INPUT, --input INPUT             String to be transformed.
  -t TRANSFORM, --transform TRANSFORM Transformation to be applied to input.
  -c CUSTOM, --custom CUSTOM          Custom transformation rules to be applied to input.
  -r RULES, --rules RULES             Read transformation rules from LDML file.
  -rev, --reverse                     List all supported transformations.
```

## Listing transforms available in ICU

You can list all available ICU transforms:

```bash
$ ./translit -l
```

### Targeted listing

It is possible to filter the list by matching a substring:

```bash
$ ./translit -s=<substring>
```

For example:

```bash
$ ./translit -s="alaloc"
Ethi-Latn/ALALOC, Ethiopic-Latin/ALALOC, Latin-Ethiopic/ALALOC, Latn-Ethi/ALALOC, Any-Ethiopic/ALALOC
```

## Transformations provided with ICU

Specifying an input string and an ICU transformation: `./translit -i=<input_string> -t=<transform>`

Examples:

```bash
$ ./translit -i="Ψάπφω" -t='Greek-Latin'
Psápphō
$ ./translit -i="የኢትዮጵያ ኦርቶዶክስ ተዋሕዶ ቤተ ክርስቲያን" -t="Ethiopic-Latin/ALALOC"
yaʼiteyop̣eyā oretodokes tawāḥedo béta keresetiyān
$ ./translit -i="የኢትዮጵያ ኦርቶዶክስ ተዋሕዶ ቤተ ክርስቲያን" -t="Ethiopic-Latin/Aethiopica"
yäʾitǝyop̣ǝya orǝtodokǝs täwaḥǝdo betä kǝrǝsǝtiyan
$ ./translit -i="የኢትዮጵያ ኦርቶዶክስ ተዋሕዶ ቤተ ክርስቲያን" -t="Ethiopic-Latin/Beta_Metsehaf"
yaʾitǝyoṗǝyā orǝtodokǝs tawāḥǝdo beta kǝrǝsǝtiyān
```

## Custom rules

```bash
$ ./translit -i="የኢትዮጵያ ኦርቶዶክስ ተዋሕዶ ቤተ ክርስቲያን" -c=":: any-hex;"
\u12E8\u12A2\u1275\u12EE\u1335\u12EB\u0020\u12A6\u122D\u1276\u12F6\u12AD\u1235\u0020\u1270\u12CB\u1215\u12F6\u0020\u1264\u1270\u0020\u12AD\u122D\u1235\u1272\u12EB\u1295
✗ ./translit -i="Thuɔŋjäŋ" -c=":: lower ; :: NFD ; :: any-hex;"
\u0074\u0068\u0075\u0254\u014B\u006A\u0061\u0308\u014B
$ ./translit -i="\u0074\u0068\u0075\u0254\u014B\u006A\u0061\u0308\u014B" -c=":: any-hex;" -rev
thuɔŋjäŋ
$ ./translit -i="Thuɔŋjäŋ" -c=":: Lower; :: NFD; :: [:Mn:] Remove;"
thuɔŋjaŋ
```

## Reading rules from LDML file
