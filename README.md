[![Build Status](https://travis-ci.org/rrousselGit/provider.svg?branch=master)](https://travis-ci.org/rrousselGit/provider)
[![pub package](https://img.shields.io/pub/v/provider.svg)](https://pub.dartlang.org/packages/provider) [![codecov](https://codecov.io/gh/rrousselGit/provider/branch/master/graph/badge.svg)](https://codecov.io/gh/rrousselGit/provider)

A generic implementation of `InheritedWidget`. It allows to expose any kind of object, without having to manually write an `InheritedWidget` ourselves.

## Usage

To expose a value, simply wrap any given part of your widget tree into any of the available `Provider` as such:

```dart
Provider<int>(
  value: 42,
  child: // ...
)
```

Descendants of `Provider` and now obtain this value using the static `Provider.of<T>` method:

```dart
var value = Provider.of<int>(context);
```

You can also use `Consumer` widget to insert a descendant, useful when both creating a `Provider` and using it:

```dart
Provider<int>(
  value: 42,
  child: Consumer<int>(
    builder: (context, value) => Text(value.toString()),
  )
)
```

____



Note that you can freely use multiple providers with different type together:

```dart
Provider<int>(
  value: 42,
  child: Provider<String>(
    value: 'Hello World',
    child: // ...
  )
)
```

And obtain their value independently:

```dart
var value = Provider.of<int>(context);
var value2 = Provider.of<String>(context);
```


## Existing Providers:

### Provider

A simple provider which takes the exposed value directly:


```dart
Provider<int>(
  value: 42,
  child: // ...
)
```


### HookProvider

A provider which can use hooks from [flutter_hooks](https://github.com/rrousselGit/flutter_hooks)

This is especially useful to create complex providers, without having to make a `StatefulWidget`.

The following example uses BLoC pattern to create a BLoC, provide its value, and dispose it when the provider is removed from the tree.

```dart
HookProvider<MyBloc>(
  hook: () {
    final bloc = useMemoized(() => MyBloc());
    useEffect(() => bloc.dispose, [bloc]);
    return bloc;
  },
  child: // ...
)
```