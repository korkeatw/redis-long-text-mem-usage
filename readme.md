Redis Set and Hset memory usage
====

> **Update** This experiment used Redis default config. It's not suite for this data, 
zip (compression) feature don't be used in this case. I changed values of `hash-max-ziplist-entries` 
and `hash-max-ziplist-value` then run script again. The `hset` command saves a lot of 
memory as we expect but use more CPU. This is trade-off what you should concern when 
use Redis in your stack.

This experiment is for testing memory usage when  store 1,000,000 long text
using `set` compare with `hset` command.

The text that use in experiment now showing below

```
Lorem Ipsum is simply dummy text of the printing and typesetting industry.
Lorem Ipsum has been the industry's standard dummy text ever since the 1500s,
when an unknown printer took a galley of type and scrambled it to make a type
specimen book. It has survived not only five centuries, but also the leap into
electronic typesetting, remaining essentially unchanged. It was popularised in
the 1960s with the release of Letraset sheets containing Lorem Ipsum passages,
and more recently with desktop publishing software like Aldus PageMaker
including versions of Lorem Ipsum
```

**Ref.** The first paragraph from [http://www.lipsum.com/](http://www.lipsum.com/)

## Set

```
# Memory
used_memory:704409696
used_memory_human:671.78M
used_memory_rss:730832896
used_memory_rss_human:696.98M
used_memory_peak:704409696
used_memory_peak_human:671.78M
total_system_memory:8248344576
total_system_memory_human:7.68G
used_memory_lua:37888
used_memory_lua_human:37.00K
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
mem_fragmentation_ratio:1.04
mem_allocator:jemalloc-4.0.3
```

## Hset

bucket_size = 100

```
# Memory
used_memory:732177624
used_memory_human:698.26M
used_memory_rss:760156160
used_memory_rss_human:724.94M
used_memory_peak:732177624
used_memory_peak_human:698.26M
total_system_memory:8248344576
total_system_memory_human:7.68G
used_memory_lua:37888
used_memory_lua_human:37.00K
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
mem_fragmentation_ratio:1.04
mem_allocator:jemalloc-4.0.3
```

bucket_size = 1000

```
# Memory
used_memory:720845280
used_memory_human:687.45M
used_memory_rss:747638784
used_memory_rss_human:713.00M
used_memory_peak:720845280
used_memory_peak_human:687.45M
total_system_memory:8248344576
total_system_memory_human:7.68G
used_memory_lua:37888
used_memory_lua_human:37.00K
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
mem_fragmentation_ratio:1.04
mem_allocator:jemalloc-4.0.3
```

bucket_size = 10000

```
# Memory
used_memory:729330480
used_memory_human:695.54M
used_memory_rss:755781632
used_memory_rss_human:720.77M
used_memory_peak:729330480
used_memory_peak_human:695.54M
total_system_memory:8248344576
total_system_memory_human:7.68G
used_memory_lua:37888
used_memory_lua_human:37.00K
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
mem_fragmentation_ratio:1.04
mem_allocator:jemalloc-4.0.3
```

## Conclusion

~~With long text, both command `set` and `hset` use memory equally and `bucket_size`
does not affect in this case.~~ `hset` use memory less than set but you have to set 
`hash-max-ziplist-entries` and `hash-max-ziplist-value` value fit with your data to get
benefir of compression feature.
