# CS1D Assignment 7: Balanced Trees

Notation: `N(left, right)` is a binary tree node; `-` is null. In the 2-4 trees below, each bracket is one node and children are listed from left to right.

## 1. AVL insertion: insert 64

The insertion path is `42 -> 50 -> 78 -> 62`. This makes node 50 right-left heavy.

1. Rotate right at 78.
2. Rotate left at 50.

```text
					42
				/    \
			32      64
		 /  \    /  \
	 17   34  50   78
						 / \    \
						49 62    88
```

## 2. AVL deletion: remove 32

Node 32 has one child, so replace it with 22. The root 48 then has a right-heavy balance of -2; rotate left at 48.

```text
					78
				/    \
			48      88
		 /  \      \
	 22   50      92
			 /  \
			49  64
```

## 3. AVL deletion: remove 22

Remove leaf 22. The root 48 is again right-heavy with balance -2; rotate left at 48.

```text
					78
				/    \
			48      88
		 /  \      \
	 32   50      92
			 /  \
			49  64
```

## 4. Splay 16

The access path is `8 -> 10 -> 11 -> 12 -> 15 -> 16`. Apply right-right zig-zig rotations from the bottom, followed by a final zig rotation.

```text
16
|
10
|\
8 12
|  |\
3  11 15
 \     /\
	4   13  -
	 \    \
		6    14
	 / \
	5   7
```

Equivalent notation:

```text
16(10(8(3(-,4(-,6(5,7))),-),12(11,15(13(-,14),-))),-)
```

## 5. Splay deletion: delete 7

Splay 7 to the root, remove it, then splay the maximum value of its left subtree (6) and attach the right subtree.

After splaying 7:

```text
7(3(-,6(4(-,5),-)),8(-,10(-,11(-,12(-,15(13(-,14),16))))))
```

Final tree:

```text
6
/ \
3   8
 \   \
	4   10
	 \    \
		5    11
					 \
						12
							\
							 15
							/  \
						 13  16
							 \
								14
```

## 6. Splay insertions

Each inserted item is splayed to the root. The tree after each insertion is shown in parenthesized notation.

```text
14: 14
44: 44(14,-)
15: 15(14,44)
55: 55(44(15(14,-),-),-)
45: 45(15(14,-),55(44,-))
85: 85(55(45(15(14,-),44),-),-)
75: 75(55(45(15(14,-),44),-),85)
65: 65(55(45(15(14,-),44),-),75(-,85))
99: 99(65(55(45(15(14,-),44),-),85(75,-)),-)
```

## 7. 2-4-tree insertion

Using top-down insertion, split a full node before descending into it.

```text
13: [13]
44: [13,44]
14: [13,14,44]

55:       [14]
				 /    \
			[13]  [44,55]

45:       [14]
				 /    \
			[13]  [44,45,55]

85:       [14,45]
				/      |      \
		 [13]    [44]   [55,85]

77:       [14,45]
				/      |        \
		 [13]    [44]   [55,77,85]

66:      [14,45,77]
			 /      |      |    \
		[13]    [44]  [55,66] [85]

98:          [45]
					 /      \
				[14]      [77]
			 /   \      /    \
		[13] [44] [55,66] [85,98]
```

## 8. 2-4-tree deletion: remove 50, then 45

Initial tree:

```text
					[30,40]
				/    |      \
		 [20] [33,35] [45,50]
```

After removing 50:

```text
					[30,40]
				/    |    \
		 [20] [33,35] [45]
```

Before removing 45, borrow through the parent from the middle sibling:

```text
					[30,35]
				/    |      \
		 [20]  [33]   [40,45]
```

Remove 45:

```text
					[30,35]
				/    |    \
		 [20]  [33]  [40]
```

## 9. 2-4-tree deletion: remove 35

The successor of 35 is 40. Replace 35 with 40, then delete 40 from the right child, which has spare keys.

```text
Initial:       [20,35]
						 /    |        \
					[10]  [30]   [40,55,60]

Final:         [20,40]
						 /    |       \
					[10]  [30]   [55,60]
```

## 10. Convert the red-black tree to a 2-4 tree

Combine each red node with its black parent:

- `5` and `14` combine with `10` to make `[5,10,14]`.
- `9` combines with `6` to make `[6,9]`.
- `16` and `30` combine with `18` to make `[16,18,30]`.

```text
							[5,10,14]
						 /    |    |       \
					[2]   [6,9] [12]  [16,18,30]
```

The deletion choices above use the standard predecessor/successor and sibling-borrow rules; an equally valid splay-delete implementation may join using the minimum of the right subtree instead of the maximum of the left subtree, producing a different but valid shape.