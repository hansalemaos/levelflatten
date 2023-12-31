# performs customizable level-wise flattening of nested iterables.

## pip install levelflatten 

#### Tested against Windows 10 / Python 3.10 / Anaconda 


## The level_flatten function provides the following advantages:

### Customizable flattening: 

It allows you to control the flattening behavior for different data types. 
You can specify how dictionaries should be treated (by items, keys, or values) 
and which non-iterable data types should be considered for individual element flattening.

### Level-wise flattening: 

It performs flattening in a level-wise manner, allowing you to control the depth of 
flattening. You can specify the maximum number of levels to flatten, which provides 
flexibility in handling complex nested structures.

### Support for various data types: 

It handles various data types such as lists, tuples, sets, dictionaries, strings, bytes, 
frozensets, bytearrays, NumPy arrays, and more. 
This makes it suitable for working with diverse data structures.

### Maintains order: 

The function preserves the order of elements while flattening, 
ensuring that the relative order of elements is maintained in the flattened list.

```python
Args:
 iterable (iterable): The input iterable to be flattened.
 n (int, optional): The maximum number of levels to flatten. Defaults to sys.maxsize.
 dict_treatment (str, optional): Treatment for dictionaries. Can be one of "items",
	 "keys", or "values". Defaults to "items".
 consider_non_iter (tuple or list, optional): Tuple or list of data types to be
	 considered non-iterable and flattened as individual elements. Defaults to
	 (str, bytes).

Returns:
 list: A flattened list containing the elements from the input iterable.
 
 
 from levelflatten import level_flatten
 import numpy as np
 from collections import defaultdict

 d = defaultdict(list)
 d[1].append((1, 2, 3, [44, 5, 5]))
 iti = [
	 {(1, 2), (3, 4)},
	 ["a", 111, 3],
	 (1, 2),
	 d,
	 "stristristri",
	 range(100, 110),
	 455j,
	 b"xxxxaaa",
	 frozenset({1, 2, 3, 4}),
	 [
		 11,
		 [
			 222,
			 33,
			 4,
			 (333, 4, {1, 2, 3, 4}, {3333: 333}, [33312, 33323], bytearray(b"xxxxx")),
		 ],
	 ],
	 [
		 {2: 3},
		 (
			 2,
			 3,
			 4,
		 ),
		 ["babab", "dd", {10: 12}, (("bbb", 12), [333, 4, {4: 32}])],
	 ],
	 (1.2, 11, 1232),
	 bytearray(b"xxxxx"),
	 np.array([1, 2, 3, 4, 5]),
	 [None, np.nan],
	 [[True, False, True]],
 ]

 t1 = level_flatten(
	 iti,
	 n=1,
	 dict_treatment="items",
	 consider_non_iter=(str, bytes, frozenset, bytearray),
 )
 print(t1)
 # [(1, 2), (3, 4), 'a', 111, 3, 1, 2, (1, [(1, 2, 3, [44, 5, 5])]), 'stristristri', 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 455j, b'xxxxaaa', frozenset({1, 2, 3, 4}), 11, [222, 33, 4, (333, 4, {1, 2, 3, 4}, {3333: 333}, [33312, 33323], bytearray(b'xxxxx'))], {2: 3}, (2, 3, 4), ['babab', 'dd', {10: 12}, (('bbb', 12), [333, 4, {4: 32}])], 1.2, 11, 1232, bytearray(b'xxxxx'), 1, 2, 3, 4, 5, None, nan, [True, False, True]]

 t1 = level_flatten(
	 iti, n=2, dict_treatment="values", consider_non_iter=(str, bytes, dict)
 )
 print(t1)
 # [1, 2, 3, 4, 'a', 111, 3, 1, 2, (1, 2, 3, [44, 5, 5]), 'stristristri', 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 455j, b'xxxxaaa', 1, 2, 3, 4, 11, 222, 33, 4, (333, 4, {1, 2, 3, 4}, {3333: 333}, [33312, 33323], bytearray(b'xxxxx')), {2: 3}, 2, 3, 4, 'babab', 'dd', {10: 12}, (('bbb', 12), [333, 4, {4: 32}]), 1.2, 11, 1232, 120, 120, 120, 120, 120, 1, 2, 3, 4, 5, None, nan, True, False, True]


 t1 = level_flatten(
	 iti, n=3, dict_treatment="keys", consider_non_iter=(str, bytes, set, tuple)
 )
 print(t1)
 # [{(1, 2), (3, 4)}, 'a', 111, 3, (1, 2), 1, 'stristristri', 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 455j, b'xxxxaaa', 1, 2, 3, 4, 11, 222, 33, 4, (333, 4, {1, 2, 3, 4}, {3333: 333}, [33312, 33323], bytearray(b'xxxxx')), 2, (2, 3, 4), 'babab', 'dd', 10, (('bbb', 12), [333, 4, {4: 32}]), (1.2, 11, 1232), 120, 120, 120, 120, 120, 1, 2, 3, 4, 5, None, nan, True, False, True]


 t1 = level_flatten(
	 iti,
	 n=4,
	 dict_treatment="items",
	 consider_non_iter=(str, bytes, set, tuple, dict, bytearray),
 )
 print(t1)
 # [{(1, 2), (3, 4)}, 'a', 111, 3, (1, 2), (1, [(1, 2, 3, [44, 5, 5])]), 'stristristri', 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 455j, b'xxxxaaa', 1, 2, 3, 4, 11, 222, 33, 4, (333, 4, {1, 2, 3, 4}, {3333: 333}, [33312, 33323], bytearray(b'xxxxx')), {2: 3}, (2, 3, 4), 'babab', 'dd', {10: 12}, (('bbb', 12), [333, 4, {4: 32}]), (1.2, 11, 1232), bytearray(b'xxxxx'), 1, 2, 3, 4, 5, None, nan, True, False, True]


 t1 = level_flatten(
	 iti, n=sys.maxsize, dict_treatment="items", consider_non_iter=(np.array, np.ndarray)
 )
 print(t1)
 # [1, 2, 3, 4, 'a', 111, 3, 1, 2, 1, 1, 2, 3, 44, 5, 5, 's', 't', 'r', 'i', 's', 't', 'r', 'i', 's', 't', 'r', 'i', 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 455j, 120, 120, 120, 120, 97, 97, 97, 1, 2, 3, 4, 11, 222, 33, 4, 333, 4, 1, 2, 3, 4, 3333, 333, 33312, 33323, 120, 120, 120, 120, 120, 2, 3, 2, 3, 4, 'b', 'a', 'b', 'a', 'b', 'd', 'd', 10, 12, 'b', 'b', 'b', 12, 333, 4, 4, 32, 1.2, 11, 1232, 120, 120, 120, 120, 120, array([1, 2, 3, 4, 5]), None, nan, True, False, True]


```