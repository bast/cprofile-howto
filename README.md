

# cprofile-howto

How to cProfile Python scripts written by others.

```python
import pstats

p = pstats.Stats('restats')
p.sort_stats('time').print_stats(10)
```
