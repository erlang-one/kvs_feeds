# KVS Feeds
**Generic KVS feed-server**

## Overview

* In development

## Dependency

* [kvs](https://github.com/synrc/kvs) – Database middleware.
* [n2o](https://github.com/synrc/n2o) – n2o_async needed.

## Configuration

### sys.config

* `log :: {Mod,Fun}` – Custom logging `function/3`, default: `{wf,info}`
* `feed_timeout :: non_neg_integer()` – Pool worker timeout, in ms, default: `600000` (10 min)
* `feed_timeout_action :: hibernate | stop` – Action for inactive pool workers, default: hibernate

## Usage

```erlang
R=#post{},
{ok,R2}=kvs_feeds:append(R),
Link = 1000,
FunUpdate=fun(#post{links=Links}=P) -> {ok,P#post{links=lists:usort([Link | Links])}} end,
{ok,R3}=kvs_feeds:update(R2,FunUpdate),
     ok=kvs_feeds:delete(R3),
        kvs_feeds:purge(R3,fun() -> [{feed,post,fun kvs_feeds:delete/1}] end),
```

## Credits

* [@m-2k](https://github.com/m-2k)
