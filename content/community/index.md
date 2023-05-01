---
title: BGP Community
date: 2023-05-01T15:16+09:00
---

## Well-known BGP Communities

Well-known BGP communities are standardized values that control routing behaviors across BGP networks. They have universally accepted meanings and are recognized by all BGP routers, regardless of their network or AS. For example, NO_EXPORT_ANY (RFC 1997) prevents a route from being exported from the local AS, while BLACKHOLE_POP blackholes traffic for a specific route at a particular PoP.

| Community| Description | Action |
| -------- | ----------- | ------ |
| 65281 | NO_EXPORT_ANY | Route will not be exported from AS199683 |
| 65282 | NO_EXPORT_POP | Route will not be exported from this PoP |
| 65283 | No_EXPORT_REGION | Route will not be exported from this Region |
| 666 | BLACKHOLE_POP | Route will be blackhole at this PoP (RTD_BLACKHOLE) |

## Specific BGP Communities

We support specific BGP Control Communities.

### Actions

- ░ = 0   do not announce to target
- ░ = 1   prepend 1 to target
- ░ = 2   prepend 2 to target
- ░ = 4   prepend 4 to target
- ░ = 8   prepend 8 to target

### Action target selector

░ = Action
- (199683,1░00,0)            Do action to everyone
- (199683,1░01,asn)          Don't do action to this asn
- (199683,1░02,asn)          Do action to this asn
- (199683,1░10,0)            Do action to every region
- (199683,1░11,region_code)  Don't do action to this region
- (199683,1░12,region_code)  Do action to this region
- (199683,1019,0)            Disable (asn,1010,0), (asn,1011,local_region) as default value
- (199683,1░20,0)            Do action to every country
- (199683,1░21,country_code) Don't do action to this country
- (199683,1░22,country_code) Do action to this country
- (199683,1░30,1)            Do action to upstreams
- (199683,1░30,2)            Do action to ixp rs
- (199683,1░30,3)            Do action to peers
- (199683,1░30,4)            Do action to downstreams
- (199683,1░30,8)            Do action to route collectors

Examples:
```
prepend 11 to AS6939: 
    (199683,1102,6939): prepend 1 to AS6939
    (199683,1202,6939): prepend 2 to AS6939
    (199683,1802,6939): prepend 8 to AS6939
                   Total : 1+2+8 = 11

prepend 2 to everyone but 6939:
    (199683,1200,0):     prepend 2 to everyone
    (199683,1201,6939):  don't do this action(prepend 2) to AS6939

do not announce to anyone: 
    (199683,1000,0):     do not announce to everyone
```

### Informational Community

- (199683,10000,region_code)    Received from region
- (199683,10001,country_code)   Received from country

```
Region code:
  41: Europe
  42: North America-E
  43: North America-C
  44: North America-W
  45: Central America
  46: South America-E
  47: South America-W
  48: Africa-N (above Sahara)
  49: Africa-S (below Sahara)
  50: Asia-S (IN,PK,BD)
  51: Asia-SE (TH,SG,PH,ID,MY)
  52: Asia-E (JP,KR,TW,HK,CN)
  53: Pacific&Oceania (AU,NZ,FJ)
  54: Antarctica
  55: Asia-N (RU)
  56: Asia-W (IR,TR,UAE)
  57: Central Asia (AF,UZ,KZ)

 Country code:
  ISO-3166 numeric-3 country code
```