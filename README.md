# Emoji synonyms and analyzer for Elasticsearch
> Add support for emoji in any Solr compatible search engine!

## What is this

This repository host synonym files in Solr format, based on [CLDR data set](http://cldr.unicode.org/).

Learn more about this in our [blog post](TODO).

Current version is based on 29β.

## Installation in Elasticsearch

### Elasticsearch mapping with emoji support

Download the emoji file you want and store it in `PATH_ES/config/analysis`.

Then create an analyzer (called `english_with_emoji` here):

```json
PUT /en-emoji
{
  "settings": {
    "analysis": {
      "filter": {
        "english_emoji": {
          "type": "synonym",
          "synonyms_path": "analysis/cldr-emoji-annotation-synonyms-en.txt" 
        }
      },
      "analyzer": {
        "english_with_emoji": {
          "tokenizer": "whitespace",
          "filter": [
            "lowercase",
            "english_emoji"
          ]
        }
      }
    }
  }
}
```

Trying it:

```json
GET /en-emoji/_analyze?analyzer=english_with_emoji
{
  "text": "I love 🍩!"
}

{
  "tokens": [
    {
      "token": "i",
      "start_offset": 0,
      "end_offset": 1,
      "type": "word",
      "position": 0
    },
    {
      "token": "love",
      "start_offset": 2,
      "end_offset": 6,
      "type": "word",
      "position": 1
    },
    {
      "token": "🍩!",
      "start_offset": 7,
      "end_offset": 10,
      "type": "word",
      "position": 2
    }
  ]
}
```

### Add Emoticon support

To support Emoticon, you can add this `char_filter`:

```json
PUT /en-emoji
{
  "settings": {
    "analysis": {
      "char_filter": {
        "emoticons_char_filter": {
          "type": "mapping",
          "mappings": [">:(=>😠",">:[=>😠",">:-(=>😠",">:-[=>😠",">=(=>😠",">=[=>😠",">=-(=>😠",">=-[=>😠",":\")=>😊",":\"]=>😊",":\"D=>😊",":-\")=>😊",":-\"]=>😊",":-\"D=>😊","=\")=>😊","=\"]=>😊","=\"D=>😊","=-\")=>😊","=-\"]=>😊","=-\"D=>😊","<\\\\3=>💔","<\/3=>💔",":\/=>😕",":\\\\=>😕",":-\/=>😕",":-\\\\=>😕","=\/=>😕","=\\\\=>😕","=-\/=>😕","=-\\\\=>😕",":,(=>😢",":,[=>😢",":,|=>😢",":,-(=>😢",":,-[=>😢",":,-|=>😢",":'(=>😢",":'[=>😢",":'|=>😢",":'-(=>😢",":'-[=>😢",":'-|=>😢","=,(=>😢","=,[=>😢","=,|=>😢","=,-(=>😢","=,-[=>😢","=,-|=>😢","='(=>😢","='[=>😢","='|=>😢","='-(=>😢","='-[=>😢","='-|=>😢",":(=>😦",":[=>😦",":-(=>😦",":-[=>😦","=(=>😦","=[=>😦","=-(=>😦","=-[=>😦","<3=>❤️","]:(=>👿","]:[=>👿","]:-(=>👿","]:-[=>👿","]=(=>👿","]=[=>👿","]=-(=>👿","]=-[=>👿","o:)=>😇","o:]=>😇","o:D=>😇","o:-)=>😇","o:-]=>😇","o:-D=>😇","o=)=>😇","o=]=>😇","o=D=>😇","o=-)=>😇","o=-]=>😇","o=-D=>😇","O:)=>😇","O:]=>😇","O:D=>😇","O:-)=>😇","O:-]=>😇","O:-D=>😇","O=)=>😇","O=]=>😇","O=D=>😇","O=-)=>😇","O=-]=>😇","O=-D=>😇","0:)=>😇","0:]=>😇","0:D=>😇","0:-)=>😇","0:-]=>😇","0:-D=>😇","0=)=>😇","0=]=>😇","0=D=>😇","0=-)=>😇","0=-]=>😇","0=-D=>😇",":,)=>😂",":,]=>😂",":,D=>😂",":,-)=>😂",":,-]=>😂",":,-D=>😂",":')=>😂",":']=>😂",":'D=>😂",":'-)=>😂",":'-]=>😂",":'-D=>😂","=,)=>😂","=,]=>😂","=,D=>😂","=,-)=>😂","=,-]=>😂","=,-D=>😂","=')=>😂","=']=>😂","='D=>😂","='-)=>😂","='-]=>😂","='-D=>😂",":*=>😗",":-*=>😗","=*=>😗","=-*=>😗","x)=>😆","x]=>😆","xD=>😆","x-)=>😆","x-]=>😆","x-D=>😆","X)=>😆","X]=>😆","X-)=>😆","X-]=>😆","X-D=>😆",":3=>👨",":-3=>👨","=3=>👨","=-3=>👨",";3=>👨",";-3=>👨","x3=>👨","x-3=>👨","X3=>👨","X-3=>👨",":|=>😐",":-|=>😐","=|=>😐","=-|=>😐",":-=>😶",":o=>😮",":O=>😮",":0=>😮",":-o=>😮",":-O=>😮",":-0=>😮","=o=>😮","=O=>😮","=0=>😮","=-o=>😮","=-O=>😮","=-0=>😮",":@=>😡",":-@=>😡","=@=>😡","=-@=>😡",":D=>😄",":-D=>😄","=D=>😄","=-D=>😄",":)=>😃",":]=>😃",":-)=>😃",":-]=>😃","=)=>😃","=]=>😃","=-)=>😃","=-]=>😃","]:)=>😈","]:]=>😈","]:D=>😈","]:-)=>😈","]:-]=>😈","]:-D=>😈","]=)=>😈","]=]=>😈","]=D=>😈","]=-)=>😈","]=-]=>😈","]=-D=>😈",":,'(=>😭",":,'[=>😭",":,'-(=>😭",":,'-[=>😭",":',(=>😭",":',[=>😭",":',-(=>😭",":',-[=>😭","=,'(=>😭","=,'[=>😭","=,'-(=>😭","=,'-[=>😭","=',(=>😭","=',[=>😭","=',-(=>😭","=',-[=>😭",":p=>😛",":P=>😛",":d=>😛",":-p=>😛",":-P=>😛",":-d=>😛","=p=>😛","=P=>😛","=d=>😛","=-p=>😛","=-P=>😛","=-d=>😛","xP=>😝","x-p=>😝","x-P=>😝","x-d=>😝","Xp=>😝","Xd=>😝","X-p=>😝","X-P=>😝","X-d=>😝",";p=>😜",";P=>😜",";d=>😜",";-p=>😜",";-P=>😜",";-d=>😜","8)=>😎","8]=>😎","8D=>😎","8-)=>😎","8-]=>😎","8-D=>😎","B)=>😎","B]=>😎","B-)=>😎","B-]=>😎","B-D=>😎",",:(=>😓",",:[=>😓",",:-(=>😓",",:-[=>😓",",=(=>😓",",=[=>😓",",=-(=>😓",",=-[=>😓","':(=>😓","':[=>😓","':-(=>😓","':-[=>😓","'=(=>😓","'=[=>😓","'=-(=>😓","'=-[=>😓",",:)=>😅",",:]=>😅",",:D=>😅",",:-)=>😅",",:-]=>😅",",:-D=>😅",",=)=>😅",",=]=>😅",",=D=>😅",",=-)=>😅",",=-]=>😅",",=-D=>😅","':)=>😅","':]=>😅","':D=>😅","':-)=>😅","':-]=>😅","':-D=>😅","'=)=>😅","'=]=>😅","'=D=>😅","'=-)=>😅","'=-]=>😅","'=-D=>😅",":$=>😒",":s=>😒",":z=>😒",":S=>😒",":Z=>😒",":-$=>😒",":-s=>😒",":-z=>😒",":-S=>😒",":-Z=>😒","=$=>😒","=s=>😒","=z=>😒","=S=>😒","=Z=>😒","=-$=>😒","=-s=>😒","=-z=>😒","=-S=>😒","=-Z=>😒",";)=>😉",";]=>😉",";D=>😉",";-)=>😉",";-]=>😉",";-D=>😉"]
        }
      },
      "filter": {
        "english_emoji": {
          "type": "synonym",
          "synonyms_path": "analysis/cldr-emoji-annotation-synonyms-en.txt" 
        }
      },
      "analyzer": {
        "english_with_emoji": {
          "char_filter": "emoticons_char_filter",
          "tokenizer": "whitespace",
          "filter": [
            "lowercase",
            "english_emoji"
          ]
        }
      }
    }
  }
}
```

```
GET /en-emoji/_analyze?analyzer=english_with_emoji
{
  "text": "I <\\3  🍩"
}

{
  "tokens": [
    {
      "token": "i",
      "start_offset": 0,
      "end_offset": 1,
      "type": "word",
      "position": 0
    },
    {
      "token": "💔",
      "start_offset": 2,
      "end_offset": 5,
      "type": "SYNONYM",
      "position": 1
    },
    {
      "token": "break",
      "start_offset": 2,
      "end_offset": 5,
      "type": "SYNONYM",
      "position": 1
    },
    {
      "token": "broken",
      "start_offset": 2,
      "end_offset": 5,
      "type": "SYNONYM",
      "position": 1
    },
    {
      "token": "heart",
      "start_offset": 2,
      "end_offset": 5,
      "type": "SYNONYM",
      "position": 1
    },
    {
      "token": "🍩",
      "start_offset": 7,
      "end_offset": 9,
      "type": "SYNONYM",
      "position": 2
    },
    {
      "token": "dessert",
      "start_offset": 7,
      "end_offset": 9,
      "type": "SYNONYM",
      "position": 2
    },
    {
      "token": "donut",
      "start_offset": 7,
      "end_offset": 9,
      "type": "SYNONYM",
      "position": 2
    },
    {
      "token": "sweet",
      "start_offset": 7,
      "end_offset": 9,
      "type": "SYNONYM",
      "position": 2
    }
  ]
}
```

## How to contribute

## Credits

## License