Homework #2
===========

[Classroom](https://classroom.github.com/a/pLYtjgIp)

Task 1
------

1. Create a module named `HW2.T1`.

2. Using the `Tree` data type defined in this module,

   ```
   data Tree a = Leaf | Branch Int (Tree a) a (Tree a)
     deriving (Show)
   ```

   where `Int` stands for the tree size, define the following function:

   ```
   tfoldr :: (a -> b -> b) -> b -> Tree a -> b
   ```

   It must collect the elements in order:

   ```
   treeToList :: Tree a -> [a]    -- output list is sorted
   treeToList = tfoldr (:) []
   ```

   This follows from the **Sorted** invariant.

   You are encouraged to define `tfoldr` in an efficient manner, doing only a
   single pass over the tree and without constructing intermediate lists.

Task 2
------

1. Create a module named `HW2.T2`.

2. Implement the following function:

   ```
   splitOn :: Eq a => a -> [a] -> NonEmpty [a]
   ```

   Conceptually, it splits a list into sublists by a separator:

   ```
   ghci> splitOn '/' "path/to/file"
   ["path", "to", "file"]

   ghci> splitOn '/' "path/with/trailing/slash/"
   ["path", "with", "trailing", "slash", ""]
   ```

   Due to the use of `NonEmpty` to enforce that there is at least one sublist in the output,
   the actual GHCi result will look slightly differently:

   ```
   ghci> splitOn '/' "path/to/file"
   "path" :| ["to","file"]
   ```

   Do not let that confuse you. The first element is not in any way special.

3. Implement the following function:

   ```
   joinWith :: a -> NonEmpty [a] -> [a]
   ```

   It must be the inverse of `splitOn`, so that:

   ```
   (joinWith sep . splitOn sep)  ≡  id
   ```

   Example usage:

   ```
   ghci> "import " ++ joinWith '.' ("Data" :| "List" : "NonEmpty" : [])
   "import Data.List.NonEmpty"
   ```

Task 3
------

1. Create a module named `HW2.T3`.

2. Using `Foldable` methods *only*\*, implement the following function:

   ```
   mcat :: Monoid a => [Maybe a] -> a
   ```

   Example usage:

   ```
   ghci> mcat [Just "mo", Nothing, Nothing, Just "no", Just "id"]
   "monoid"

   ghci> Data.Monoid.getSum $ mcat [Nothing, Just 2, Nothing, Just 40]
   42
   ```
   \* Means that the only allowed import in `HW2.T3` is `Data.Foldable`

3. Using `foldMap` to consume the list, implement the following function:

   ```
   epart :: (Monoid a, Monoid b) => [Either a b] -> (a, b)
   ```

   Example usage:

   ```
   ghci> epart [Left (Sum 3), Right [1,2,3], Left (Sum 5), Right [4,5]]
   (Sum {getSum = 8},[1,2,3,4,5])
   ```

Task 4
------

1. Create a module named `HW2.T4`.

2. Define the following data type and a lawful `Semigroup` instance for it:

   ```
   data ListPlus a = a :+ ListPlus a | Last a
   infixr 5 :+
   ```

3. Define the following data type and a lawful `Semigroup` instance for it:

   ```
   data Inclusive a b = This a | That b | Both a b
   ```

   The instance must not discard any values:

   ```
   This i  <>  This j  =  This (i <> j)   -- OK
   This i  <>  This _  =  This i          -- This is not the Semigroup you're looking for.
   ```

4. Define the following data type:

   ```
   newtype DotString = DS String
   ```

   Implement a `Semigroup` instance for it, such that the strings are
   concatenated with a dot:

   ```
   ghci> DS "person" <> DS "address" <> DS "city"
   DS "person.address.city"
   ```

   Implement a `Monoid` instance for it using `DS ""` as the identity element.
   Make sure that the laws hold:

   ```
   mempty <> a  ≡  a
   a <> mempty  ≡  a
   ```

5. Define the following data type:

   ```
   newtype Fun a = F (a -> a)
   ```

   Implement lawful `Semigroup` and `Monoid` instances for it.