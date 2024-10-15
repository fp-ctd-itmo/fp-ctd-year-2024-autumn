# Monad practice

Во всех заданиях подразумевается использование монад.

## Задание 1

Рассмотрим тип, который представляет собой синтаксическое дерево арифметического выражения:
```haskell
data Expr a
  = Const a
  | MinusInfinity
  | PlusInfinity
  | Add (Expr a) (Expr a)
  | Sub (Expr a) (Expr a)
  | Mul (Expr a) (Expr a)
  | Div (Expr a) (Expr a)
  | Pow (Expr a) (Expr a)

data ErrMsg
  = DivisionByZero
  | ...
```

Реализуйте функцию
```haskell
evalExpr :: (???) => Expr a -> Either ErrMsg a
```

## Задание 2

Еще один тип для представления синтаксического дерева:
```haskell
data Expr a
  = Const a
  | Var String
  | Add (Expr a) (Expr a)
  | Sub (Expr a) (Expr a)
  | Mul (Expr a) (Expr a)

newtype VarMap a = VarMap (Map String a)
```

Реализуйте функцию:
```haskell
evalExpr :: (???) => Expr a -> VarMap a -> a
```

В случае, если переменная неопределена, можно кидать ошибку с помощью `error`

## Задание 3

Больше типов синтаксических деревьев богу синтаксических деревьев:
```haskell
data Expr a
  = Const a
  | Add (Expr a) (Expr a)
  | Sub (Expr a) (Expr a)
  | Mul (Expr a) (Expr a)

data Statement a
  = Seq (Statement a) (Statement a)
  | EvalAndPrint (Expr a)
  | Log String
```

Реализуйте функцию:
```haskell
evalStatement :: (???) => Statement a -> String
```

## Задание 4

Реализуйте алгоритм Евклида с логированием
(для каждого шага алгоритма необходимо сообщить, чему были равны числа,
для которых считалось GCD).

```haskell
gcdWithLog :: (???) => a -> a -> ([(a, a)], a)
```

## Задание 5

Рассмотрим следующие типы:
```haskell
data Post = Post { pTitle :: String, pBody :: String }

data Blog = Blog
  { bPosts   :: [Post]
  , bCounter :: Int
  }
```

Реализуйте инстанс `Monad` для:
```haskell
newtype BlogM a = BlogM (Blog -> (a, Blog))
```

А также функции:
```haskell
readPost :: Int -> BlogM Post

newPost :: Post -> BlogM ()
```

## Задание 6

Задание про использование IORef и IOArray (также можно использовать [vector](https://hackage.haskell.org/package/vector) если есть желание).
Пишем на хаскеле в императивном стиле.

Реализуйте утилиту `sort` которая в качестве аргументов командной строки будет
принимать массив, который необходимо отсортировать и печатать отсортированный массив,
например:
```sh
stack exec -- sort 2 4 1 6 5 7
1 2 4 5 6 7
```

Это задание может быть рассказано более чем одним человеком при условии, что разными людьми
будут реализованы разные сортировки. Сортировки должны быть in-place и не медленее,
чем O(nlogn). Помимо этого можно реализовывать "не совсем алгоритмы сортировки",
например radix sort.

## Задание 7

Реализуйте алгоритм [Simple Moving Average](https://en.wikipedia.org/wiki/Moving_average), используя State монаду.

```sh
ghci> moving 4 [1, 5, 3, 8, 7, 9, 6]
[1.0, 3.0, 3.0, 4.25, 5.75, 6.75, 7.5]

ghci> moving 2 [1, 5, 3, 8, 7, 9, 6]
[1.0, 3.0, 4.0, 5.5, 7.5, 8.0, 7.5]
```

## Задание 8

Задание про работу с файлами и аргументами командной строки.
Рекомендуется использовать [optparse-applicative](https://hackage.haskell.org/package/optparse-applicative)
для парсинга аргументов командной строки.

Реализовать простую утилиту для поддержания TODO-листа, она должна уметь:
* Добавить таску
* Изменить статус таски (open, in progress, done)
* Удалить таску
* Посмотреть на все таски с каким-либо статусом.

Например:
```sh
stack exec -- todo-list --add-task 'Придумать задания для практики'
```

## Задание 9

Задание про использование [`System.Process`](https://hackage.haskell.org/package/process-1.6.7.0/docs/System-Process.html)
и [`System.IO`](http://hackage.haskell.org/package/base-4.12.0.0/docs/System-IO.html).

Реализовать утилиту, которая принимает в качестве аргумента путь до директории,
рекурсивно перебирает все файлы в директории и производит сравнение файлов на похожесть
попарно друг с другом. В качестве результата утилиты выводит наиболее похожие
файлы. Для сравнения похожести файлов можете использовать `diff` или другие аналоги.

Пример использования:
```sh
$ ls hw1_solutions
hw1_zyoba.hs hw1_zyaba.hs hw1_biba.hs hw1_boba.hs
$ stack exec -- diff-measurer hw1_solutions
Two most similar files: hw1_zyoba.hs hw1_boba.hs
```

## Задание 10 *

Реализуйте агоритм сортировки слиянием используя CPS (continuation passing style),
а именно: функцию `mergeSortCPS` и вспомогательные функции `splitCPS` и `mergeCPS`
со следующими сигнатурами:

```haskell
mergeSortCPS :: (Ord a) => [a] -> ([a] -> r) -> r

splitCPS     :: (Ord a) => [a] -> (([a], [a]) -> r) -> r

mergeCPS     :: (Ord a) => [a] -> [a] -> ([a] -> r) -> r
```

**Бонусный вариант** используйте тип данных `Cont`, представленный на лекции, и
его инстанс монады для реализации сортировки в CPS стиле. Сигнатуры функций должны
быть изменены соответствующим образом.
