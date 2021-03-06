---
layout: post
title: Swift API Design Guidelines 톺아보기 - 1
tags:
  - Swift
---
어떤 프로그래밍 언어를 사용하더라도 해당 언어답게 잘 사용하기 위한 Style Guide가 존재합니다. 
Python의 경우 PEP 8에서 Python 코드의 Style Guide를 제시하고 있고, 구글에서는 Java Style Guide를 제시하고 있습니다. 
Swift도 Apple에서 제안하는 API Design Guidelines가 있습니다. 
이번 포스트를 통해서 Swift를 Swift답게 잘 사용하기 위하여 Swift API Design Guidelines 톺아보고자 합니다. 

Swift API Design Guidelines는 swift.org에 잘 설명되어 있고, 이 내용을 한국어로 번역한 블로그 포스트도 매우 많습니다. 
그래서 번역된 내용 위에 제 나름대로의 결론을 덧붙이는 방식으로 작성하려 합니다. 
단순히 가이드라인의 내용만 보고싶으시다면 [레퍼런스](./#레퍼런스)에 있는 원문과 민소네님의 번역본을 보시는 것을 추천드립니다.

---

# API Design Guidelines
가이드라인을 확인하기 전에 먼저 API라는 용어를 이해할 필요가 있습니다. 
`API(Application Programming Interface)는 응용 프로그램에서 사용할 수 있도록 OS나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스`를 뜻합니다. 

API와 비슷한 개념으로 **UI(User Interface)**가 있는데, UI는 대표적으로 소프트웨어와 소프트웨어를 사용하는 유저 사이를 연결해주는 방법을 제공합니다.
유저가 상품의 상세 화면을 보기 위해서 상품을 터치해야 한다거나, 아이폰에서 이전 화면을 보기 위해 화면 왼쪽 끝에서 오른쪽으로 스와이핑을 해야하는 등의 매우 다양한 UI는
유저가 소프트웨어를 제어할 수 있게 해줍니다. 

API도 마찬가지입니다. 카카오의 지도 API를 사용해서 우리는 우리의 프로그램에서 카카오의 지도 프로그램의 특정 기능을 실행할 수 있습니다.
네이버 API로 손쉽게 한글을 영어로 번역할 수도 있죠. 이렇게 무언가 제어할 수 있도록 다리를 놓아주는 역할을 하는 것이 바로 Interface이고 API는 Application Programming을 위한 Interface가 되는 것입니다.
`Swift API Design Guidelines 이라는 것도 결국엔 우리가 Swift로 작성할 기능을 누군가가 쉽고 효율적으로 사용할 수 있도록 Apple에서 정한 컨벤션입니다.`

그럼 이제 가이드라인을 살펴보도록 하겠습니다.

---

## Table of Contents
- [기본 개념(Fundamentals)](./#fundamentals)
- [이름 지정(Naming)](../swift-api-design-guideline2/#naming)
    - [명확한 사용 활성화하기(Promote Clear Usage)](../swift-api-design-guideline2/#promote-clear-usage)
    - [유창하게 사용할 수 있도록 노력하기(Strive for Fluent Usage)](../swift-api-design-guideline2/#strive-for-fluent-usage)
    - [전문용어를 잘 사용하기(Use Terminology Well)](../swift-api-design-guideline2/#use-terminology-well)
- [규칙(Conventions)](../swift-api-design-guideline3/#conventions)
    - [일반적인 규칙(General Conventions)](../swift-api-design-guideline3/#general-conventions)
    - [매개 변수(Parameters)](../swift-api-design-guideline3/#parameters)
    - [인자 레이블(Argument Labels)](../swift-api-design-guideline3/#argument-labels)
- [특별 지침(Special Instructions)](../swift-api-design-guideline3/#special-instructions)


> 📕 하나의 포스트로 모든 내용을 다루기엔 내용이 많아서 총 3개의 포스트로 나누어 작성하였습니다.
> Table of Contents의 기본 개념(Fundamentals)을 제외한 각 링크는 다른 포스트에 연결되어 있습니다.

---

<h2 id="fundamentals">기본 개념(Fundamentals)</h2>

- **가장 중요한 목표는 사용 시점에서의 명료성입니다.** API는 한번 선언되지만 반복적으로 사용되는 메소드, 속성과 같은 개체(Entity)들을 이해하기 쉽고 간결하게 만드는 데에 중점을 두어야 합니다. API의 설계에 대한 평가는 선언부를 읽는 것만으로는 충분하지 않습니다. 사용 사례에서 문맥상 명확하게 이해가 되는지를 기준으로 평가해야 합니다.

- **명료성은 간결성보다 중요합니다.** Swift 코드를 간결하게 작성할 수는 있지만, 문자들 몇 개만 사용해서 가능한 가장 적은 양의 코드를 작성하는 것이 목표가 아닙니다. Swift 코드의 간결함은 강력한 type 시스템과 boilerplate 코드를 줄여주는 기능들이 제공하는 부수적인 효과입니다.

- 모든 선언문에 문서화용 주석(documentation comments)을 작성하세요. API 문서를 작성하면서 얻은 통찰력은 API 디자인에 큰 영향을 미칠 수 있으니 절대 미루지 마세요.

- `간단한 용어로 API 기능을 설명할 수 없다면, 당신의 API 설계는 잘못되었을 가능성이 높습니다.`

<br>

> 🧑🏻‍💻 API는 결국 간단한 용어로 설명이 가능하며, 읽기 쉽고 그 역할이 명확해서 사용하기 편해야 합니다.
> 또한, 주석을 항상 작성해서 사용자가 이해하기 쉽게 만들어주어야 합니다.
> 그러지 못해온 제 자신을 반성합니다.

<br>

### 문서화용 주석(documentation comments) 자세히
- Swift에서 지원하는 [Markdown 언어](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/)를 사용하기.

- 선언된 개체(entity)를 설명하는 요약으로 시작하세요. API는 선언과 요약을 통해 완전히 이해되는 경우가 많습니다.
```swift
/// 같은 요소를 포함하는 `self`의 "view"를 역순으로 반환.
func reversed() -> ReverseCollection
```

  - **요약에 초점을 맞추세요.** 요약은 매우 중요한 부분입니다. 많은 우수한 코드 주석은 훌륭한 요약문을 가지고 있습니다.

  - 가능하면 한 개의 절을 사용하고, 마침표로 끝내세요. 완전한 문장을 사용하지 마세요.

  - 함수 또는 메소드가 어떤 일을 하는지, 어떤 것을 반환하는지 설명하고, null 효과와 Void 반환은 설명을 생략하세요.
```swift
/// `self` 시작부분에 `newHead`를 삽입.
mutating func prepend(newHead: Int)
/// `self`의 요소를 동반하는 `head` 가 포함된 `List` 를 반환.
func prepending(head: Element) -> List
/// 비어있지 않다면 `self`의 첫 번째 요소를 제거 및 반환하고; 비어있다면 `nil`을 반환.
mutating func popFirst() -> Element?
```

  - subscript가 어떤 것에 접근하는지 설명합니다.
```swift
/// `index` 번째 요소에 접근.
subscript(index: Int) -> Element { get set }
```

  - 이니셜라이저가 무엇을 생성하는지 설명합니다.
```swift
/// `x`를 `n`번 반복하는 인스턴스를 생성.
init(count n: Int, repeatedElement x: Element)
```

  - 그 외의 경우, 선언된 개체가 무엇인지 설명합니다.
```swift
/// 어떤 위치에서든 똑같이 효율적으로 삽입/제거할 수 있는 컬렉션.
struct List {
  /// `self`의 첫 번째 요소, 또는 self가 비어있다면 `nil`
  var first: Element?
  ...
```

- 경우에 따라서는 굳이 1개의 절이 아니라 여러 절과 글 머리 기호를 사용할 수 있습니다. 공백 줄로 절을 나누고 완전한 문장을 사용합니다.
```swift
/// 표준 출력에 `items` 각 요소의 텍스트 표현을 작성합니다. ← 요약
///                                             ← 빈 줄
/// 각 요소인 `x`의 텍스트 표현은 `String(x)` 표현식으로   ← 추가 설명
/// 제공됩니다.
///
/// - Parameter seperator: 요소 사이에 출력되는 텍스트     ⎫
/// - Parameter terminator: 끝부분에 출력되는 텍스트      ⎭ 매개변수 부분
///
/// - Note: 끝부분에 줄 바꿈을 출력하지 않으려면          ⎫
///   `terminator: ""`를 전달하세요.                ⎟
///                                              ⎬ 기타 참고사항
/// - SeeAlso: `CustomDebugStringConvertible`,   ⎟
///   `CustomStringConvertible`, `debugPrint`.   ⎭
public func print(items: Any..., separator: String = " ", terminator: String = "\n")
```
  - 요약문 외에 추가 정보를 제공할 때에는 많은 사람들이 이해할 수 있게 [symbol 문서 마크업 요소들](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1)을 사용하세요.
  - [symbol 커맨드 구문](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html#//apple_ref/doc/uid/TP40016497-CH51-SW1)을 익히고 활용하세요. Xcode와 같이 인기 있는 개발 도구는 다음 키워드로 시작하는 글 머리 기호(예: - Note)를 특별하게 취급합니다.
  - [특별하게 취급되는 키워드 리스트(Attention~Warning)](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/Attention.html)

<br>

> 🧑🏻‍💻 선언된 모든 entity에 대해서 간단한 용어로 작성된 주석이 반드시 있어야 한다는 점이 중요해보입니다.
> 저는 당연하게 생각한 entity도 누군가에겐 설명이 필요할 수 있겠다는 생각이 들었습니다.   
> Xcode에서 지원되는 마크다운 언어에 대한 문서를 확인하고 code snippet으로 추가해 사용한다면 매우 유용할 것 같습니다.
> 마크다운 언어를 잘 사용한다면, 제가 만든 API나 엔티티도 다른 사람에게는 iOS 프레임워크 라이브러리처럼 기깔나게 보여질 수 있겠다는 생각이 드네요!

---






<br>

내용이 길어질 것 같아서 3개의 파트로 나눠서 작성하도록 하겠습니다.   
다음 포스트는 이름 지정(Naming)입니다 😎

<br>

## 레퍼런스

- [[원문] Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
- [[번역] Swift API Design Guidelines](https://minsone.github.io/swift-internals/api-design-guidelines/)