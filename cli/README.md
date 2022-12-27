# Translit: a flexible command line text transformation tool

```bash
usage: translit [-h] [-l] [-s SEARCH] [-i INPUT] [-t TRANSFORM] [-c CUSTOM] [-r RULES] [-rev]

Transliterate strings

Options:
  -h, --help                          Show this help message and exit
  -l, --list                          List all ICU supported transformations.
  -s SEARCH, --search SEARCH          Search supported transformations.
  -i INPUT, --input INPUT             String to be transformed.
  -t TRANSFORM, --transform TRANSFORM Transformation to be applied to input.
  -r RULES, --rules RULES             Read transformation rules from LDML file.
  -c CUSTOM, --custom CUSTOM          Custom transformation rules to be applied to input.
  -rev, --reverse                     List all supported transformations.
```

## Listing transforms available in ICU

You can list all available ICU transforms:

```bash
$ ./translit -l
```

### Targeted listing

It is possible to filter the list by matching a substring: `./translit -s=<substring>`

For example:

```bash
$ ./translit -s="alaloc"
Ethi-Latn/ALALOC, Ethiopic-Latin/ALALOC, Latin-Ethiopic/ALALOC, Latn-Ethi/ALALOC, Any-Ethiopic/ALALOC
```

## Transformations

### Using transformations provided with ICU

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

### Using custom rules

It is possible to go beyond the inbuilt transliteration rules and build a transformation that chains transliterations together or adds additional transformations.

Specify an input string and transformation rules: `./translit -i=<input_string> -c=<transform_rules>`


```bash
$ ./translit -i="የኢትዮጵያ ኦርቶዶክስ ተዋሕዶ ቤተ ክርስቲያን" -c=":: any-hex;"
\u12E8\u12A2\u1275\u12EE\u1335\u12EB\u0020\u12A6\u122D\u1276\u12F6\u12AD\u1235\u0020\u1270\u12CB\u1215\u12F6\u0020\u1264\u1270\u0020\u12AD\u122D\u1235\u1272\u12EB\u1295
$ ./translit -i="Thuɔŋjäŋ" -c=":: lower ; :: NFD ; :: any-hex;"
\u0074\u0068\u0075\u0254\u014B\u006A\u0061\u0308\u014B
$ ./translit -i="\u0074\u0068\u0075\u0254\u014B\u006A\u0061\u0308\u014B" -c=":: any-hex;" -rev
thuɔŋjäŋ
$ ./translit -i="Thuɔŋjäŋ" -c=":: Lower; :: NFD; :: [:Mn:] Remove;"
thuɔŋjaŋ
./translit -i="Ψάπφω" -c=':: Greek-Latin; :: Lower;  ::Latin-Ascii; '
psappho
$ ./translit -i='नागार्जुन' -c=':: Deva-Latin; :: Title;'
Nāgārjuna
$ ./translit -i='नागार्जुन' -c=':: Deva-Latin; :: Latin-ASCII; :: Title;'
Nagarjuna
```

### Reading rules from LDML files

It is possible to read in transformation rules from a LDML file. Specify an input string and transformation rules: `./translit -i=<input_string> -r=<ldml_file_path>`.

For example:

```bash
$ ./translit -i="የኢትዮጵያ ኦርቶዶክስ ተዋሕዶ ቤተ ክርስቲያን" -r="ldml/und-Ethi-t-und-latn-m0-alaloc.xml"
yaʼiteyop̣eyā oretodokes tawāḥedo béta keresetiyān
$ ./translit -i="Yeŋu luêêl yïn ye an ee nyankui?" -r='ldml/din-Latn-unified-t-din-Latin-standard.xml'
Yeŋu luëël yïn ye an ee nyankui?
```

#### Combining LDML files with custom rules

It is also possible to read in a set of rules from an LDML file and use that transformation in a cchain if custom rules. Specify an input string, LDML file path and transformation rules: `./translit -i=<input_string> -r=<ldml_file_path> -c=<transform_rules>`

```bash
$ ./translit -i="Yeŋu luêêl yïn ye an ee nyankui?" -r='ldml/din-Latn-unified-t-din-Latin-standard.xml' -c=":: dinkaUnified-dinkaStandard ; :: Title; :: NFD; "
Yeŋu Luëël Yïn Ye An Ee Nyankui?
```
