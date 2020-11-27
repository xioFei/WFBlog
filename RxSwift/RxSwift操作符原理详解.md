### RxSwift操作符原理详解

最上层协议 `ObservableConvertibleType`

```swift
/// Type that can be converted to observable sequence (`Observable<E>`).
public protocol ObservableConvertibleType {
    associatedtype E
    func asObservable() -> Observable<E>
}
```

继承协议  `SharedSequenceConvertibleType`

```swift
///A type that can be converted to `SharedSequence`.
public protocol SharedSequenceConvertibleType : ObservableConvertibleType {
    associatedtype SharingStrategy: SharingStrategyProtocol
    /**
    Converts self to `SharedSequence`.
    */
    func asSharedSequence() -> SharedSequence<SharingStrategy, E>
}
```



操作符 `asObservable()` 创建一个`Observable` 然后订阅

```swift
extension ObservableType {
    /// Default implementation of converting `ObservableType` to `Observable`.
    public func asObservable() -> Observable<E> {
        // temporary workaround
        //return Observable.create(subscribe: self.subscribe)
        return Observable.create { o in
            return self.subscribe(o)
        }
    }
}

```

