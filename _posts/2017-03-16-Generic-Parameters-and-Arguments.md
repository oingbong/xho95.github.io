---
layout: post
comments: true
title:  "Swift 3.1: 일반화된 매개 변수와 인자 (Generic Parameters and Arguments)"
date:   2017-03-16 00:00:00 +0900
categories: Swift Language Grammar Generic Parameters Arguments
---

> 이 글은 Swift 를 공부하기 위해 애플에서 공개한 [The Swift Programming Language (Swift 3.1)](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/) 책의 [Generic Parameters and Arguments](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/GenericParametersAndArguments.html#//apple_ref/doc/uid/TP40014097-CH37-ID406) 부분을 번역하고 주석을 달아서 정리한 글입니다. 현재는 Swift 3.1 버전에 대해서 정리되어 있습니다.

이번 장에서는 일반화된 (generic) 타입, 함수 및 초기자를 위한 매개 변수와 인자에 대해 설명합니다. 일반화된 타입, 함수 또는 초기자를 선언할 때는 그 일반화된 타입, 함수 또는 초기자가 사용할 수 있는 타입 매개 변수를 지정하게 됩니다. 이러한 타입 매개 변수는 자리 지킴이의 역할을 하는데 일반화된 타입의 인스턴스가 만들어지거나 일반화된 함수 또는 초기자가 호출될 때 실제 타입으로 대체되어 굳어집니다.

Swift 의 일반화 (generic) 에 대한 개요는 [Generics]() 에서 볼 수 있습니다.

<a name="generic-parameter-clause"></a>
### 일반화된 매개 변수 구절 (Generic Parameter Clause)

일반화된 매개 변수 구절은 일반화된 타입 또는 함수의 타입 매개 변수를 지정할 때 이 매개 변수와 관련된 제약 조건과 요구 사항을 함께 지정합니다. 일반화된 매개 변수 구절은 꺾쇠 괄호 (<>) 로 감싸며 다음과 같은 양식을 가집니다:

<`generic parameter list`>

일반화된 매개 변수 목록 (list) 은 쉼표 (comma) 로 구분된 일반화된 매개 변수들의 목록으로 각각의 일반화된 매개 변수의 양식은 다음과 같습니다:

`type parameter`: `constraint`

일반화된 매개 변수는 타입 매개 변수와 그에 뒤따르는 제약 조건으로 구성되는데 이 제약 조건은 선택 요소입니다. 타입 매개 변수는 그냥 단순히 자리 지킴이 역할을 하는 타입의 이름 (예를 들어, `T`, `U`, `V`, `Key`, `Value` 등) 입니다. 타입 매개 변수 (그리고 그와 연관된 타입) 에는 함수나 초기자의 서명을 포함하여 타입, 함수 및 초기자 선언부에서 접근할 수 있습니다. [^signature]

제약 조건은 타입 매개 변수가 특정한 클래스를 상속 받거나 프로토콜 또는 프로토콜 조합을 따르고 있음을 지정합니다. 예를 들어 아래의 일반화된 함수에서 일반화된 매개 변수 `T: Comparable` 은 타입 매개 변수 `T` 를 대체하는 어떠한 타입 인자라도 반드시 `Comparable` 프로토콜을 따라야 함을 지시하고 있습니다.

```
func simpleMax<T: Comparable>(_ x: T, _ y: T) -> T {
    if x < y {
        return y
    }
    return x
}
```

예를 들면 `Int` 와 `Double` 은 둘 다 `Comparable` 프로토콜을 따르고 있기 때문에 이 함수는 둘 중 어떤 타입이라도 인자로 받아들일 수 있습니다. 일반회된 타입에서와는 다르게 일반화된 함수나 초기자를 사용할 때는 일반화된 인자 구절을 따로 지정하지는 않습니다. 대신에 타입 인자는 함수나 초기자로 전달되는 인자의 타입으로부터 추론됩니다.

```
simpleMax(17, 42) // T is inferred to be Int
simpleMax(3.14159, 2.71828) // T is inferred to be Double
```

#### 일반화된 where 구절 (Generic Where Clauses)

타입 매개 변수와 그와 연관된 타입들에는 요구 사항을 추가로 지정할 수 있는데 이렇게 하려면 타입이나 함수 본체 중괄호의 바로 앞에 일반화된 `where` 구절을 넣으면 됩니다. 일반화된 `where` 구절은 `where` 키워드와 쉼표로 구분 된 하나 이상의 요구 사항 목록으로 구성됩니다.

where `requirements`

일반화된 `where` 구절에 있는 요구 사항은 타입 매개 변수가 클래스를 상속받거나 프로토콜 및 프로토콜 조합을 따르도록 지정합니다. 물론 일반화된 `where` 구절은 타입 매개 변수의 제약 조건 표현을 쉽게 해주는 꿀팁같은 것을 제공해 주지만 (예를 들어 `<T: Comparable>` 를 `<T> where T: Comparable` 로 동등하게 표현하는 것 등), 이를 잘 사용하면 타입 매개 변수와 그에 연관된 타입들에 더 복잡한 제약 조건도 부여할 수 있습니다. 가령 타입 매개 변수의 연관 타입이 프로토콜을 따르도록 제약할 수 있습니다. 예를 들어 `<S: Sequence> where S.Iterator.Element: Equatable` 은 이 `S` 가 `Sequence` 프로토콜을 따르면서 그 연관 타입인 `S.Iterator.Element` 가 `Equatable` 프로토콜을 따르도록 지정합니다. 이 제약 조건은 수열의 각 요소가 동등 비교를 수행할 수 있어야 함을 보장합니다.

두 타입이 동일해야 함을 요구 조건으로 지정할 수도 있는데 이 때는 `==` 연산자를 사용합니다. 예를 들면 `<S1: Sequence, S2: Sequence> where S1.Iterator.Element == S2.Iterator.Element` 는 `S1` 과 `S2` 가 `Sequence` 프로토콜을 따르도록 제약하면서 각 수열의 요소들이 반드시 같은 타입으로 되어 있어야 함을 표현합니다.

타입 매개 변수를 대체할 어떤 타입 인자라도 반드시 타입 매개 변수에 대한 모든 제약 조건과 요구 사항을 만족해야만 합니다.

일반화된 함수나 초기자는 타입 매개 변수에 다른 제약 조건, 요구 사항, 또는 둘 다를 제공해서 추가 정의 (overload) 할 수 있습니다. 일반화된 함수나 초기자의 추가 정의 버전을 호출하면 컴파일러는 이들 제약 조건을 사용하여 어떤 추가 정의 함수나 초기자를 실행해야할지를 결정하게 됩니다.

일반화된 `where` 구절에 대한 보다 많은 정보와 일반화된 함수 선언의 예를 보고 싶으면 [Generic Where Clauses]() 부분을 보면 됩니다.

> 일반화된 매개 변수 구절의 문법
> 
> generic-parameter-clause → **<­** generic-parameter-list ­**>**  
> generic-parameter-list → generic-parameter­ \| generic-parameter **,** ­generic-parameter-list­  
> generic-parameter → type-name­
> generic-parameter → type-name­ **:** ­type-identifier­  
> generic-parameter → type-name­ **:** ­protocol-composition-type
>
> generic-where-clause → **where** ­requirement-list­  
> requirement-list → requirement­  requirement­ **,** ­requirement-list  
> requirement → conformance-requirement­  same-type-requirement  
>
> conformance-requirement → type-identifier­ **:** ­type-identifier  
> conformance-requirement → type-identifier­ **:** ­protocol-composition-type  
> same-type-requirement → type-identifier­ **==­** type­

### 일반화된 인자 구절 (Generic Argument Clause)

일반화된 인자 구절은 일반화된 타입의 타입 인자를 지정합니다. 일반화된 인자 구절은 꺾쇠 괄호 (<>) 로 감싸며 다음과 같은 양식을 가집니다:

<`generic argument list`>

일반화된 인자 목록은 쉼표로 구분되는 타입 인자들의 목록입니다. 타입 인자는 실제로 굳혀지는 타입의 이름으로써 이것이 일반화된 타입의 일반화된 매개 변수 구절에 있는 관련된 타입 매개 변수를 대체하게 됩니다. 결과는 일반화된 타입의 특수한 버전입니다. [^specialized-version] 아래에 있는 예제는 Swift 표준 라이브러리의 일반화된 사전 (dictionary) 타입을 간략하게 나타낸 버전입니다.

```swift
struct Dictionary<Key: Hashable, Value>: Collection, ExpressibleByDictionaryLiteral {
    /* ... */
}
```

일반화된 `Dictionary` 타입의 특수한 버전인 `Dictionary<String, Int>` 은 일반화된 매개 변수 `Key: Hashable` 과 `Value` 를 굳혀진 타입 인자인 `String` 과 `Int` 로 대체합니다. 각 타입 인자들은 일반화된 매개 변수를 대체할 때 반드시 모든 제약 조건들을 만족해야하며 이 때 일반화된 `where` 구절에서 지정한 모든 추가 요구 사항들도 예외 없이 만족해야 합니다. 위의 예제에서 보면 `Key` 타입 매개 변수는 `Hashable` 프로토콜을 따르도록 제약하고 있으므로 `String` 은 반드시 `Hashable` 프로토콜을 따라야 합니다.

타입 매개 변수를 스스로가 일반화된 타입의 특수화 버전인 타입 인자로 대체할 수도 있습니다. (물론 이 타입 인자도 제약 조건과 요구 사항을 적절하게 만족하도록 주어져야 합니다.) 예를 들어 `Array<Element>` 의 타입 매개 변수인 `Element` 를 행렬의 특수화 버전인 `Array<Int>` 로 대체하여 그 요소들이 스스로 정수 배열인 배열을 만들 수 있습니다. [^specialized-form]

```swift
let arrayOfArrays: Array<Array<Int>> = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

일반화된 매개 변수 구절([Generic Parameter Clause](#generic-parameter-clause)) 에서 언급했듯이 일반화된 인자 구절을 사용하여 일반화도니 함수나 초기자의 타입 매개 변수를 지정하지 않도록 합니다.

> 일반화된 인자 구절의 문법
> 
> generic-argument-clause → **<­** generic-argument-list ­**>­**  
> generic-argument-list → generic-argument­ \| generic-argument **,** generic-argument-list­  
> generic-argument → type­

### 원문 자료

* [Generic Parameters and Arguments](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/GenericParametersAndArguments.html#//apple_ref/doc/uid/TP40014097-CH37-ID406) : [The Swift Programming Language (Swift 3.1)](https://developer.apple.com/library/prerelease/content/documentation/Swift/Conceptual/Swift_Programming_Language/) 자료입니다.

### 관련 자료

* [Swift 3.1: 스위프트 프로그래밍 언어 (Swift Programming Language)](http://xho95.github.io/swift/programming/language/grammar/2017/02/27/The-Swift-Programming-Language.html)

### 참고 자료

[^signature]: 여기서 함수나 초기자의 '서명' 이라는 것은 함수 본체를 제외한 함수의 이름, 매개 변수, 반환 타입 등을 포함한 모든 것입니다. 쉽게 생각해서 C 나 C++ 의 함수 선언 문장에 해당하는 것이 Swift 의 함수 서명이라고 볼 수 있습니다. '서명'이라는 용어를 사용하는 것은 이것이 각각의 함수를 구별하는 역할, 즉 마치 사람의 서명과도 같은 역할을 하기 때문인 것 같습니다.

[^specialized-version]: 이 부분은 C++ 언어의 메타 프로그래밍에서 나오는 일반화 (generalization) 및 특수화 (specialization) 의 개념과 유사한 것 같습니다. Swift 의 일반화 (generic; 제네릭) 이 C++ 의 템플릿 (template) 과 사실상 동등한 개념이므로 당연한 것이라고 할 수 있습니다.

[^specialized-form]: 이 부분은 아직 잘은 모르겠지만 C++ 의 template template parameter 와 유사한 개념인 것 같습니다.
