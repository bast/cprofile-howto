

# How to cProfile

How to cProfile Python scripts written by others (where you don't know which
file to edit or where to even start looking).


## Problem

Assume we want to profile a pip-installed Python application which provides a script with an `entry_point`.
This is our example script with example arguments:
```shell
$ myscript --somearg --anotheroption X
```

After browsing the
[cProfile](https://docs.python.org/3/library/profile.html#module-cProfile)
documentation it was not clear to me where to insert the `cProfile.run()` for
the pip installed script.

After some trial and error, the following worked:


## How to profile the script

```shell
$ python -m cProfile -o restats $(which myscript) --somearg --anotheroption X
```

## Producing statistics from the restats file

Run this script to extract statistics:
```python
import pstats

p = pstats.Stats('restats')
p.sort_stats('time').print_stats(10)
```

Example output:
```
Tue Jan 21 21:08:28 2020    restats

         8168533 function calls (7730858 primitive calls) in 13.080 seconds

   Ordered by: internal time
   List reduced from 5735 to 10 due to restriction <10>

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
      174    2.932    0.017    2.932    0.017 {method 'executemany' of 'sqlite3.Cursor' objects}
    81975    0.743    0.000    1.389    0.000 {method 'randint' of 'numpy.random.mtrand.RandomState' objects}
395367/19848    0.582    0.000    1.498    0.000 /home/user/.local/share/virtualenvs/somecode-0C9px_8o/lib/python3.8/copy.py:128(deepcopy)
    35788    0.318    0.000    0.439    0.000 /home/user/.local/share/virtualenvs/somecode-0C9px_8o/lib/python3.8/site-packages/scipy/special/_basic.py:1938(comb)
    16562    0.222    0.000    0.227    0.000 /home/user/somecode/somecode/cluster_data.py:139(_add_excitor)
   146757    0.216    0.000    0.216    0.000 {built-in method numpy.array}
    81984    0.210    0.000    0.646    0.000 /home/user/.local/share/virtualenvs/somecode-0C9px_8o/lib/python3.8/site-packages/numpy/core/_dtype.py:333(_name_get)
     9925    0.208    0.000    2.795    0.000 /home/user/somecode/somecode/cc_rep_w_sampling.py:85(_get_cluster_in_combo)
    49161    0.176    0.000    0.321    0.000 /home/user/somecode/somecode/hvertices.py:332(_fully_specified)
    69860    0.166    0.000    0.166    0.000 {built-in method numpy.arange}
```
