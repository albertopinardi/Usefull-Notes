PUT albums
{
  "settings": {
    "number_of_shards": "8",
    "number_of_replicas": "1",
    "analysis": {
      "analyzer": {
          "folding": {
              "filter": [
              "lowercase",
              "asciifolding",
              "apostrophe"
              ],
      "tokenizer": "standard"
            }
          }
        }
      }
}

PUT albums/albums/spotifydb21751152
{

    "id": 21751152,
    "isrc_list": "itg020700001 itg029100002 itg028400005 itg029100005 itg028800007 itg028400007 itg029700005 itb007300945 itg028800004 itg028800001 itg020300005 it7009101110 itg028200003 itb007600688 itg020700008 it7009101080 itg028400001 itg020700004 it7009101040 itg028800006 it7009101050 itg029500001 itg028800002 itg029900008 itg029900004 itg020700003 it7009812940 itb007271193 itg028200005 itb007500797 itg020700002 itg029700008",
    "artists_list": "antonello venditti",
    "title": "le donne",
    "track_titles": "dalla pelle al cuore alta marea (don't dream it's over) ci vorrebbe un amico amici mai 21 modi per dirti ti amo piero e cinzia settembre le tue mani su di me mitico amore ricordati di me con che cuore giulia dimmelo tu cos'è una stupida e lurida storia d'amore regali di natale buona domenica notte prima degli esami indimenticabile sotto il segno dei pesci il compleanno di cristina sara ogni volta miraggi lula che tesoro che sei scatole vuote donna in bottiglia sora rosa le ragazze di monaco lilly piove su roma le cose della vita",
    "source": "spotifydb",
    "album_id": "7orqkogl5wwxba0uoo39ny"

}

GET albums/_mapping

PUT albums/_mapping/albums
{
  "albums":{
    "properties":{
      "title": {
        "type": "text",
        "fields": {
          "folded": {
            "type": "text",
            "analyzer": "folding"
            }
          }
      },
    "artist_list": {
      "type": "text",
      "fields": {
        "folded": {
          "type": "text",
          "analyzer": "folding"
        }
      }
    },
    "track_titles": {
      "type": "text",
      "fields": {
        "folded": {
          "type": "text",
          "analyzer": "folding"
        }
      }
    }
    }}
}

PUT albums/albums/spotifydb21751159
{

    "id": 21751159,
    "isrc_list": "itg029900001 itg029900002 itg029900003 itg029900004 itg029900005 itg029900006 itg029900007 itg029900008 itg029900009",
    "artists_list": "antonello venditti",
    "title": "goodbye novecento",
    "track_titles": "goodbye novecento shake in questo mondo che non puoi capire che tesoro che sei fianco a fianco su questa nave chiamata musica la coscienza di zeman lula v.a.s.t.",
    "source": "spotifydb",
    "album_id": "3tdg734kzcltvtkirfbyql"

}

GET albums/_search
{
  "query": {
    "multi_match": {
      "query": "venditti donne",
      "fields": ["track_titles","track_titles.folded","artist_list","artist_list.folded","title","title.folded"]
    }
      
    }
  }

GET albums/_analyze
{
  "field": "track_titles.folded",
  "text": ["lta marea (don't dream it's over) v.a.s.t."]
}