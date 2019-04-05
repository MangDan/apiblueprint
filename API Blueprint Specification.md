# API Blueprint
본 문서는 API Blueprint 공식 깃헙 문서를 번역한 문서입니다.  
  
원문 깃헙 문서 :  
https://github.com/MangDan/apiblueprint/blob/master/API%20Blueprint%20Specification.md  
APU Blueprint 공식 홈페이지의 Specification 페이지 :  
https://apiblueprint.org/documentation/specification.html

---

Author: z@apiary.io
Version: 1A9

---

# API Blueprint
#### Format 1A revision 9

## [I. API Blueprint Language](#def-api-blueprint-language)
+ [Introduction](#def-introduction)
+ [API Blueprint](#def-api-blueprint)
+ [API Blueprint document](#def-api-blueprint-document)
+ [Blueprint section](#def-blueprint-section)
    + [Section types](#def-section-types)
    + [Section structure](#def-section-structure)
    + [Keywords](#def-keywords)
    + [Identifier](#def-identifier)
    + [Description](#def-description)
    + [Nested sections](#def-nested-sections)

## [II. Sections Reference](#def-sections-reference)

### Abstract
+ [Named section](#def-named-section)
+ [Asset section](#def-asset-section)
+ [Payload section](#def-payload-section)

### Section Basics
+ [Metadata section](#def-metadata-section)
+ [API name & overview section](#def-api-name-section)
+ [Resource group section](#def-resourcegroup-section)
+ [Resource section](#def-resource-section)
+ [Resource model section](#def-model-section)
+ [Schema section](#def-schema-section)
+ [Action section](#def-action-section)
+ [Request section](#def-request-section)
+ [Response section](#def-response-section)
+ [URI parameters section](#def-uriparameters-section)
+ [Attributes section](#def-attributes-section)
+ [Headers section](#def-headers-section)
+ [Body section](#def-body-section)

### Going Further
+ [Data Structures section](#def-data-structures)
+ [Relation section](#def-relation-section)


## [III. Appendix](#def-appendix)
+ [URI Templates](#def-uri-templates)

---

<br>

<a name="def-api-blueprint-language"></a>
# I. API Blueprint 언어

<a name="def-introduction"></a>
## 소개
이 문서는 API Blueprint 형식의 전체 규격이다.  
API Blueprint에 대한 간단하게 요약된 내용은 [API Blueprint Tutorial](Tutorial.md) 혹은 [examples][] 중 일부를 확인하기 바란다.

<a name="def-api-blueprint"></a>
## API Blueprint
API Blueprint는 API 문서 지향 웹 API 설명 언어다. (역자: 웹 API(REST)를 설계하기 위한 문서 작성 언어라고 이해하자) API Blueprint는 기본적으로 웹 API를 기술하기 위해 사용되는 Markdown 구문 위에 놓인 의미적 가정의 집합이다. (역자: 마크다운 구문을 활용해서 API 정의하는 스펙정도로 이해하면 된다.)

API Blueprint는 일반 [Markdown syntax][] 외에 [GitHub Fluested Markdown syntax][]을 준수한다.

<a name="def-api-blueprint-document"></a>
## API Blueprint 문서
API Blueprint 문서( Blueprint)는 웹 API의 전체 또는 일부를 기술하기 위한 일반 텍스트 마크다운 문서다. 그 문서는 논리적인 **섹션***으로 구성되어 있다. 각 섹션은 문서에서 고유한 의미, 내용 및 위치를 가진다.

일반 섹션 정의 및 구조는 [Blueprint section](#def-blueprint-section) 장 뒷부분에서 상세히 다룬다.

모든 Blueprint 섹션은 선택 사항이다. 단, 섹션은 API Blueprint **문서 구조(document structure)**를 준수해야 한다.

### Blueprint 문서 구조 (document structure)

+ [`0-1` **Metadata** section](#def-metadata-section)
+ [`0-1` **API Name & overview** section](#def-api-name-section)
+ [`0+` **Resource** sections](#def-resource-section)
    + [`0-1` **URI Parameters** section](#def-uriparameters-section)
    + [`0-1` **Attributes** section](#def-attributes-section)
    + [`0-1` **Model** section](#def-model-section)
        + [`0-1` **Headers** section](#def-headers-section)
        + [`0-1` **Attributes** section](#def-attributes-section)
        + [`0-1` **Body** section](#def-body-section)
        + [`0-1` **Schema** section](#def-schema-section)
    + [`1+` **Action** sections](#def-action-section)
        + [`0-1` **Relation** section](#def-relation-section)
        + [`0-1` **URI Parameters** section](#def-uriparameters-section)
        + [`0-1` **Attributes** section](#def-attributes-section)
        + [`0+` **Request** sections](#def-request-section)
            + [`0-1` **Headers** section](#def-headers-section)
            + [`0-1` **Attributes** section](#def-attributes-section)
            + [`0-1` **Body** section](#def-body-section)
            + [`0-1` **Schema** section](#def-schema-section)
        + [`1+` **Response** sections](#def-response-section)
            + [`0-1` **Headers** section](#def-headers-section)
            + [`0-1` **Attributes** section](#def-attributes-section)
            + [`0-1` **Body** section](#def-body-section)
            + [`0-1` **Schema** section](#def-schema-section)
+ [`0+` **Resource Group** sections](#def-resourcegroup-section)
    + [`0+` **Resource** sections](#def-resource-section) (see above)
+ [`0+` **Data Structures** section](#def-data-structures)

> **NOTE:** 섹션 이름 앞의 숫자는 해당 섹션에 대한 허용되는 개수를 나타낸다. (역자: 각 레벨에서 > 허용하는 셋션 수, 예를 들면 Resource 섹션 하위 URI Parameters 섹션은 1개까지만 포함될 수 있다.)

> **NOTE:** 특정 섹션 유형에 대한 설명은 [Sections Reference](#defections-reference)를 참조.


<a name="def-blueprint-section"></a>
## Blueprint 섹션(section)
_섹션_은 API Blueprint의 논리적인 단위다. 예: API 개요, 리소스 그룹 또는 리소스 정의.

일반적으로 섹션은 마크다운 요소에서 키워드를 사용하여 정의된다. 키워드는 섹션의 유형에 따라 마크다운 헤더 요소 또는 목록 항목 요소로 작성된다.

섹션 정의는 또한 식별자(identifier) 및 추가 수식어(modifier)와 같은 추가 변수 구성요소를 포함할 수 있다.

> **NOTE**: 키워드 대신 문서에서 위치에 따라 인식되는 두 개의 특별한 섹션이 있다: [Metadata section](#def-metadata-section)과 [API Name & Overview section](#def-api-name-section)이다. 정의에 대한 자세한 내용은 해당 섹션 항목을 참조.


#### 예: 헤더로 정의된 섹션

    # <keyword>

     ...

    # <keyword>

     ...



> **NOTE:** 이 규격은 "atx" 스타일 헤더(`#`s 사용)를 사용하지만, 아래와 같이 "Setext" 헤더 구문(====)> 도 서로 바꾸어 사용할 수 있다:
>
>     <keyword>
>     =========
>
>     ...
>
>     <keyword>
>     =========
>
>     ...

#### Example: 목록으로 정의된 섹션

    + <keyword>

     ...

    + <keyword>

     ...

> **NOTE:**  이 규격은 플러스(`+`)를 목록 마커로 사용하지만, 별표(`*`), 플러스(`+`) 및 하이픈(`-`)과 같은 마크다운 [list syntax][]을 사용할 수도 있다:
>
>     * <keyword>
>
>     ...
>
>     - <keyword>
>
>     ...

<a name="def-section-types"></a>
### 섹션 유형들 (Section types)
API Blueprint 섹션에는 여러가지 유형이 있다. [Section Reference](#def-sections-reference)에서 섹션 유형의 전체 목록을 확인할 수 있다.

**Blueprint 섹션 챕터에서는 일반적인 섹션 구문에 대해서 얘기한다.**
**특정 섹션 유형은 이 일반 섹션 구문의 일부 부분에서 확인할 수 있다.**
구문에 대한 자세한 내용은 항상 각 섹션 참조 (Section Reference)를 참고.

<a name="def-section-structure"></a>
### 섹션 구조 (Section structure)
**키워드**로 정의된 API Blueprint 섹션의 일반적인 구조는 **식별자(identifier) (name)**, 섹션 **설명(description)** 및 **내포된 섹션(nested
sections)** 또는 특별하게 **정의된 콘텐츠(specific content)**를 포함한다.

#### 예: 헤더로 정의된 섹션 구조

    # <keyword> <identifier>

    <description>

    <specific content>

    <nested sections>

#### Example: 목록으로 정의된 섹션 구조

    + <keyword> <identifier>

        <description>

        <specific content>

        <nested sections>

<a name="def-keywords"></a>
### 키워드 (Keywords)
섹션 정의에는 다음과 같은 예약된 키워드가 사용된다.

#### 헤더 키워드 (Header keywords)
- `Group`
- `Data Structures`
- [HTTP methods][httpmethods] (e.g. `GET, POST, PUT, DELETE`...)
- [URI templates][uritemplate] (e.g. `/resource/{id}`)
- Combinations of an HTTP method and URI Template (e.g. `GET /resource/{id}`)

#### 목록 키워드 (List keywords)
- `Request`
- `Response`
- `Body`
- `Schema`
- `Model`
- `Header` & `Headers`
- `Parameter` & `Parameters`
- `Values`
- `Attribute` & `Attributes`
- `Relation`

> **NOTE: 다른 Markdown 헤더 또는 목록에서 이러한 키워드를 사용하지 마십시오. (역자: 사용하지 못한다는 의미라기 보다는 API Blueprint 고유 키워드이므로, 다른 마크다운에서는 인식하지 못할 뿐)

> **NOTE:** WHTTP 메서드 키워드(GET, POST, PUT, DELETE)를 제외하고 섹션 키워드는 대소문자를 구분하지 않는다.

<a name="def-identifier"></a>
### 식별자 (Identifier)
섹션 정의에서는 섹션의 식별자(Identifier)를 포함할 수 있거나 포함해야 한다. 식별자는 `[`, `]`,
`(`, `)` 및 newline 문자를 제외한 모든 문자의 조합이다.

식별자는 키워드를 포함하지 않아야 한다.

#### 예제

```
Adam's Message 42
```

```
my-awesome-message_2
```


<a name="def-description"></a>
### 설명 (Description)
섹션 설명(Description)이란 섹션 정의를 따르는 임의의 마크다운 형식의 내용을 말한다. (역자: 섹션을 정의할 때 섹션에 대한 설명을 붙이고 싶을 때 사용하는 부분이라고 이해하면 된다.)

[reserved keywords](#def-keywords)와 충돌하지 않는 한 섹션 설명(Description)에 Markdown 헤더 또는 목록 항목을 사용하는 것이 가능하다.

> **NOTE:** 헤더 레벨은 실제 섹션에서 중첩된 상태로 유지하는 것을 좋은 사례로 간주한다. (먼 말이냐 ㅡㅡ)

<a name="def-nested-sections"></a>
### 중첩, 내포된 섹션 (Nested sections)
한 섹션은 다른 내포된 섹션을 포함할 수 있다.

내포된 섹션 유형에 따라 섹션을 내포하려면 헤더 수준 (동일한 레벨)이나 목록 항목 들여쓰기로 증가(포함)시키기만 하면 된다. 동일한 레벨에서 섹션 시작과 다음 섹션의 시작 사이에 있는 모든 것은 섹션의 일부로 간주된다.

중첩될 수 있는 섹션과 장소는 관련 섹션의 기재에 설명된 바와 같이, 경우에 따라 섹션에 따라 달라진다.

#### Example: Nested header-defined section

    # <section definition>

     ...

    ## <nested section definition>

     ...

#### Example: Nested list-defined section

    + <section definition>

         ...

        + <nested section definition>

         ...

> **NOTE:** 꼭은 아니지만 내포된 섹션 마크다운 헤더의 레벨을 높이는 것은 좋은 습관이다.

> **NOTE:** 마크다운 리스트 섹션은 항상 앞의 마크다운 헤더 섹션에 따라 중첩되는 것으로 간주된다.

---

<a name="def-sections-reference"></a>
# II. 섹션 참조 (Sections Reference)
> **NOTE:** "Abstract"으로 표시된 섹션은 다른 섹션의 베이스 역할을 하므로 직접 사용할 수 없다.

# Abstract

<a name="def-named-section"></a>
## Named section
- **Abstract**
- **Parent sections:** vary, see descendants
- **Nested sections:** vary, see descendants
- **Markdown entity:** header, list
- **Inherits from**: none

#### 정의
선택적 섹션 이름에 의한 [keyword](#def-keywords)에 의해 정의된다 - 마크다운 헤더 또는 목록 엔티티에서의 [identifier](#def-identifier).

```
# <keyword> <identifier>
```

```
+ <keyword> <identifier>
```

#### 설명
명명된 섹션은 대부분의 API Blueprint 섹션의 기본 섹션이다. [general section](#def-section-structure)에 부합하며 섹션 이름(식별자), 설명 및 내포된 섹션 또는 특정 형식의 내용으로 구성된다(하위 설명 참조).

#### 예제

    # <keyword> Section Name
    This the `Section Name` description.

    - one
    - **two**
    - three

    <nested sections> |  <formatted content>

---

<a name="def-asset-section"></a>
## Asset section
- **Abstract**
- **Parent sections:** vary, see descendants
- **Nested sections:** none
- **Markdown entity:** list
- **Inherits from**: none

#### 정의
마크다운 목록 엔터티내에서 [keyword](#def-keywords)로 정의된다.

    + <keyword>

#### 설명
Asset 섹션은 API Blueprint의 원자형 데이터에 대한 기본 섹션이다. 이 섹션의 내용은 [pre-formatted code block](http://daringfireball.net/projects/markdown/syntax#precode)이 될 것으로 보인다.

#### 예제

    + <keyword>

            {
                "message": "Hello"
            }

#### 예제: Fenced code blocks

    + <keyword>

        ```
        {
            "message": "Hello"
        }
        ```

---

<a name="def-payload-section"></a>
## 페이로드 섹션 (Payload section)
- **Abstract**
- **Parent sections:** vary, see descendants
- **Nested sections:** [`0-1` Headers section](#def-headers-section), [`0-1` Attributes section](#def-attributes-section), [`0-1` Body section](#def-body-section), [`0-1` Schema section](#def-schema-section)
- **Markdown entity:** list
- **Inherits from**: [Named section](#def-named-section)

#### 정의
Markdown 목록 엔티티에서 [keyword](#def-keywords)로 정의됨. 키워드 뒤에 식별자가 붙을 수 있다. 정의는 브레이스()로 둘러싸인 페이로드의 미디어 유형이 포함될 수 있다.

    + <keyword> <identifier> (<media type>)

> **NOTE:** 특정 섹션 유형 정의는 하위 항목을 참조.

#### 설명
페이로드 섹션은 HTTP 요청 또는 응답 메시지의 페이로드로 전송되는 정보를 나타낸다. 페이로드(Payload)는 HTTP 헤더 형태의 메타 정보와 HTTP 본문 형태의 임의의 콘텐츠로 선택적으로 구성된다.

또한 API Blueprint 컨텍스트에서 페이로드에는 설명, 메시지 본문 속성 설명 및 메시지 본문 검증 스키마가 포함된다.

페이로드에는 미디어 유형이 연관되어 있을 수 있다. 페이로드의 미디어 유형은 HTTP Content-Type 헤더 형식으로써 수신되거나 전송된 메타데이터를 나타낸다. 지정된 경우 페이로드는 내포된 차체 [Body section](#def-body-section)을 포함한다.

이 섹션에는 다음의 내포된 섹션 하나 이상이 포함되어야 한다:
- [`0-1` Headers section](#def-headers-section)
- [`0-1` Attributes section](#def-attributes-section)
- [`0-1` Body section](#def-body-section)
- [`0-1` Schema section](#def-schema-section)

페이로드 섹션의 내용에 내포된 세션이 없다면, [Body section](#def-body-section)으로 간주된다.

#### Relation of Body, Schema and Attributes sections
Each of body, schema and attributes sections describe a message payload's body.
These descriptions **should** be consistent, not violating each other. When
multiple body descriptions are provided they **should** be prioritized as
follows:

1. For resolving message-body schema
    1. Schema section
    2. Attributes section
    3. Body section

2. For resolving message-body example
    1. Body section
    2. Attributes section
    3. Schema section

#### Referencing
Instead of providing a payload section content, a
[model payload section](#def-model-section) can be referenced using the
Markdown implicit [reference syntax][]:

    [<identifier>][]

#### Example

    + <keyword> Payload Name (application/json)

        This the `Payload Name` description.

        + Headers

         ...

        + Body

         ...

        + Schema

        ...

#### Example: Referencing model payload

    + <keyword> Payload Name

        [Resource model identifier][]

---


# Section Basics


<a name="def-metadata-section"></a>
## Metadata section
- **Parent sections:** none
- **Nested sections:** none
- **Markdown entity:** special
- **Inherits from**: none

#### Definition
Key-value pairs. Each key is separated from its value by a colon (`:`). One
pair per line. Starts at the beginning of the document and ends with the first
Markdown element that is not recognized as a key-value pair.

#### Description
Metadata keys and their values are tool-specific. Refer to relevant tool
documentation for the list of supported keys.

#### Example

    FORMAT: 1A
    HOST: http://blog.acme.com

---

<a name="def-api-name-section"></a>
## API name & overview section
- **Parent sections:** none
- **Nested sections:** none
- **Markdown entity:** special, header
- **Inherits from**: [Named section](#def-named-section)

#### Definition
Defined by the **first** Markdown header in the blueprint document, unless it
represents another section definition.

#### Description
Name and description of the API

#### Example

    # Basic ACME Blog API
    Welcome to the **ACME Blog** API. This API provides access to the **ACME
    Blog** service.

---

<a name="def-resourcegroup-section"></a>
## Resource group section
- **Parent sections:** none
- **Nested sections:** [`0+` Resource section](#def-resource-section)
- **Markdown entity:** header
- **Inherits from**: [Named section](#def-named-section)

#### Definition
Defined by the `Group` keyword followed by group [name (identifier)](#def-identifier):

    # Group <identifier>

#### Description
This section represents a group of resources (Resource Sections). **May**
include one or more nested [Resource Sections](#def-resource-section).

#### Example

```apib
# Group Blog Posts

## Resource 1 [/resource1]

 ...

# Group Authors
Resources in this groups are related to **ACME Blog** authors.

## Resource 2 [/resource2]

 ...
```

---

<a name="def-resource-section"></a>
## Resource section
- **Parent sections:** none, [Resource group section](#def-resourcegroup-section)
- **Nested sections:** [`0-1` Parameters section](#def-uriparameters-section), [`0-1` Attributes section](#def-attributes-section), [`0-1` Model section](#def-model-section), [`1+` Action section](#def-action-section)
- **Markdown entity:** header
- **Inherits from**: [Named section](#def-named-section)

#### Definition
Defined by an [URI template][uritemplate]:

    # <URI template>

**-- or --**

Defined by a resource [name (identifier)](#def-identifier) followed by an
[URI template][uritemplate] enclosed in square brackets `[]`.

    # <identifier> [<URI template>]

**-- or --**

Defined by an [HTTP request method][httpmethods] followed by [URI template][uritemplate]:

    # <HTTP request method> <URI template>

**-- or --**

Defined by a resource [name (identifier)](#def-identifier) followed by an
[HTTP request method][httpmethods] and an [URI template][uritemplate] enclosed
in square brackets `[]`:

    # <identifier> [<HTTP request method> <URI template>]

> **NOTE:** In the latter two cases the rest of this section represents the
> [Action section](#def-action-section) including its description and nested
> sections and **follows the rules of the Action section instead**.

#### Description
An API [resource](http://www.w3.org/TR/di-gloss/#def-resource) as specified by
its *URI* or a set of resources (a resource template) matching its *URI
template*.

This section **should** include at least one nested
[Action section](#def-action-section) and **may** include following nested
sections:

- [`0-1` URI parameters section](#def-uriparameters-section)

    URI parameters defined in the scope of a Resource section apply to
    _any and all_ nested Action sections except when an [URI template][uritemplate] has
    been defined for the Action.

- [`0-1` Attributes section][]

    Attributes defined in the scope of a Resource section represent Resource
    attributes. If the resource is defined with a name these attributes **may**
    be referenced in [Attributes sections][].

- [`0-1` Model section](#def-model-section)

- Additional [Action sections](#def-action-section)

> **NOTE:** A blueprint document may contain multiple sections for the same
> resource (or resource set), as long as their HTTP methods differ. However it
> is considered good practice to group multiple HTTP methods under one resource
> (resource set).

#### Example

```apib
# Blog Posts [/posts/{id}]
Resource representing **ACME Blog** posts.
```

```apib
# /posts/{id}
```

```apib
# GET /posts/{id}
```

---

<a name="def-model-section"></a>
## Resource model section
- **Parent sections:** [Resource section](#def-resource-section)
- **Nested sections:** [Refer to payload section](#def-payload-section)
- **Markdown entity:** list
- **Inherits from**: [Payload section](#def-payload-section)

#### Definition
Defined by the `Model` keyword followed by an optional media type:

    + Model (<media type>)

#### Description
A [resource manifestation](http://www.w3.org/TR/di-gloss/#def-resource-manifestation) - one
exemplary representation of the resource in the form of a
[payload](#def-payload-section).

#### Referencing
The payload defined in this section **may** be referenced in any response or
request section in the document using parent section's identifier. You can
refer to this payload in any of the following [Request](#def-request-section)
or [Response](#def-response-section) payload sections using the Markdown
implicit [reference syntax][].

#### Example

```apib
# My Resource [/resource]

+ Model (text/plain)

        Hello World

## Retrieve My Resource [GET]

+ Response 200

    [My Resource][]
```

---

<a name="def-schema-section"></a>
## Schema section
- **Parent sections:** [Payload section](#def-payload-section)
- **Nested sections:** none
- **Markdown entity:** list
- **Inherits from**: [Asset section](#def-asset-section)

#### Definition
Defined by the `Schema` keyword in Markdown list entity.

    + Schema

#### Description
Specifies a validation schema for the HTTP message-body of parent payload section.

#### Example

Following example uses [Body section](#def-body-section) to provide an example of an `application/json` payload, and [Schema section](#def-schema-section) to provide a [JSON Schema](http://json-schema.org/) describing all possible valid shapes of the payload.

```apib
## Retrieve a Message [GET]

+ Response 200 (application/json)
    + Body

            {"message": "Hello world!"}

    + Schema

            {
                "$schema": "http://json-schema.org/draft-04/schema#",
                "type": "object",
                "properties": {
                    "message": {
                        "type": "string"
                    }
                }
            }
```

---

<a name="def-action-section"></a>
## Action section
- **Parent sections:** [Resource section](#def-resource-section)
- **Nested sections:**
    [`0-1` Relation section](#def-relation-section),
    [`0-1` URI parameters section](#def-uriparameters-section),
    [`0-1` Attributes section](#def-attributes-section),
    [`0+` Request section](#def-request-section),
    [`1+` Response section](#def-response-section)
- **Markdown entity:** header
- **Inherits from**: [Named section](#def-named-section)

#### Definition
Defined by an [HTTP request method][httpmethods]:

    ## <HTTP request method>

**-- or --**

Defined by an action [name (identifier)](#def-identifier) followed by an
[HTTP request method][httpmethods] enclosed in square brackets `[]`.

    ## <identifier> [<HTTP request method>]

**-- or --**

Defined by an action [name (identifier)](#def-identifier) followed by an
[HTTP request method][httpmethods] and
[URI template][uritemplate] enclosed in square brackets `[]`.

    ## <identifier> [<HTTP request method> <URI template>]

#### Description
Definition of at least one complete HTTP transaction as performed with the
parent resource section. An action section **may** consist of multiple HTTP
transaction examples for the given HTTP request method.

This section **may** include one nested
[URI parameters section](#def-uriparameters-section) describing any URI
parameters _specific_ to the action – URI parameters discussed in the scope of
an Action section apply to the respective Action section ONLY.

This section **may** include one nested [Attributes section][] defining the
input (request) attributes of the section. If present, these attributes
**should** be inherited in every Action's [Request section][] unless specified
otherwise.

Action section **should** include at least one nested
[Response section](#def-response-section) and **may** include additional nested
[Request](#def-request-section) and [Response](#def-response-section) sections.

Nested Request and Response sections **may** be ordered into groups where each
group represents one transaction example. The first transaction example group
starts with the first nested Request or Response section. Subsequent groups
start with the first nested Request section following a Response section.

Multiple Request and Response nested sections within one transaction example
**should** have different identifiers.

#### Example

```apib
# Blog Posts [/posts{?limit}]
 ...

## Retrieve Blog Posts [GET]
Retrieves the list of **ACME Blog** posts.

+ Parameters
    + limit (optional, number) ... Maximum number of posts to retrieve

+ Response 200

        ...

## Create a Post [POST]

+ Attributes

        ...

+ Request

        ...

+ Response 201

        ...

## Delete a Post [DELETE /posts/{id}]

+ Parameters
    + id (string) ... Id of the post

+ Response 204
```

#### Example Multiple Transaction Examples

```apib
# Resource [/resource]
## Create Resource [POST]

+ request A

        ...

+ response 200

        ...

+ request B

        ...

+ response 200

        ...

+ response 500

        ...

+ request C

        ...

+ request D

        ...

+ response 200

        ...
```

> **NOTE:** The "Multiple Transaction Examples" example demonstrates three
> transaction examples for one given action:
>
> 1. 1st example: request `A`, response `200`
> 2. 2nd example: request `B`, responses `200` and `500`
> 3. 3rd example: requests `C` and `D`, response `200`

---

<a name="def-request-section"></a>
## Request section
- **Parent sections:** [Action section](#def-action-section)
- **Nested sections:** [Refer to payload section](#def-payload-section)
- **Markdown entity:** list
- **Inherits from**: [Payload section](#def-payload-section)

#### Definition
Defined by the `Request` keyword followed by an optional [identifier](#def-identifier):

    + Request <identifier> (<Media Type>)

#### Description
One HTTP request-message example – payload.

#### Example

```apib
+ Request Create Blog Post (application/json)

        { "message" : "Hello World." }
```

---

<a name="def-response-section"></a>
## Response section
- **Parent sections:** [Action section](#def-action-section)
- **Nested sections:** [Refer to payload section](#def-payload-section)
- **Markdown entity:** list
- **Inherits from**: [Payload section](#def-payload-section)

#### Definition
Defined by the `Response` keyword. The response section definition **should**
include an [HTTP status code][] as its identifier.

    + Response <HTTP status code> (<Media Type>)

#### Description
One HTTP response-message example – payload.

#### Example

```apib
+ Response 201 (application/json)

            { "message" : "created" }
```

---

<a name="def-uriparameters-section"></a>
## URI parameters section
- **Parent Sections:** [Resource section](#def-resource-section) | [Action section](#def-action-section)
- **Nested Sections:** none
- **Markdown entity:** list
- **Inherits from**: none, special

#### Definition
Defined by the `Parameters` keyword written in a Markdown list item:

    + Parameters

#### Description
Discussion of URI parameters _in the scope of the parent section_.

This section **must** be composed of nested list items only. This section
**must not** contain any other elements. Each list item describes a single URI
parameter. The nested list items subsections inherit from the
[Named section](#def-named-section) and are subject to additional formatting as
follows:

    + <parameter name>: `<example value>` (<type> | enum[<type>], required | optional) - <description>

        <additional description>

        + Default: `<default value>`

        + Members
            + `<enumeration value 1>`
            + `<enumeration value 2>`
            ...
            + `<enumeration value N>`

Where:

+ `<parameter name>` is the parameter name as written in
  [Resource Section](#def-resource-section)'s URI (e.g. "id").
+ `<description>` is any **optional** Markdown-formatted description of the
  parameter.
+ `<additional description>` is any additional **optional** Markdown-formatted
  [description](#def-description) of the parameter.
+ `<default value>` is an **optional** default value of the parameter – a value
  that is used when no value is explicitly set (optional parameters only).
+ `<example value>` is an **optional** example value of the parameter (e.g. `1234`).
+ `<type>` is the **optional** parameter type as expected by the API (e.g.
  "number", "string", "boolean"). "string" is the **default**.
+ `Members` is the **optional** enumeration of possible values.
  `<type>` should be surrounded by `enum[]` if this is present.
  For example, if enumeration values are present for a parameter whose type is
  `number`, then `enum[number]` should be used instead of `number` to.
+ `<enumeration value n>` represents an element of enumeration type.
+ `required` is the **optional** specifier of a required parameter
  (this is the **default**)
+ `optional` is the **optional** specifier of an optional parameter.

> **NOTE:** This section **should only** contain parameters that are specified
> in the parent's resource URI template, and does not have to list every URI
> parameter.

#### Example

```apib
# GET /posts/{id}
```

```apib
+ Parameters
    + id - Id of a post.
```

```apib
+ Parameters
    + id (number) - Id of a post.
```

```apib
+ Parameters
    + id: `1001` (number, required) - Id of a post.
```

```apib
+ Parameters
    + id: `1001` (number, optional) - Id of a post.
        + Default: `20`
```

```apib
+ Parameters
    + id (enum[string])

        Id of a Post

        + Members
            + `A`
            + `B`
            + `C`
```
---

<a name="def-attributes-section"></a>
## Attributes Section
- **Parent sections:** [Resource section](#def-resource-section) | [Action section](#def-action-section) | [Payload section](#def-payload-section)
- **Nested sections:** See **[Markdown Syntax for Object Notation][MSON]**
- **Markdown entity:** list
- **Inherits from**: none

#### Definition
Defined by the `Attributes` keyword followed by an optional
[MSON Type Definition][] enclosed in parentheses.

    + Attributes <Type Definition>

`<Type Definition>` is the type definition of the data structure being
described. If the `<Type Definition>` is not specified, an `object` base type
is assumed. See [MSON Type Definition][] for details.

##### Example

```apib
+ Attributes (object)
```

#### Description
This section describes a data structure using the
**[Markdown Syntax for Object Notation][MSON] (MSON)**.
Based on the parent section, the data structure being described is one of the
following:

1. Resource data structure attributes ([Resource section](#def-resource-section))
2. Action request attributes ([Action section](#def-action-section))
3. Payload message-body attributes ([Payload section](#def-payload-section))

Data structures defined in this section **may** refer to any arbitrary data
structures defined in the [Data Structures section](#def-data-structures) as
well as to any data structures defined by a named resource attributes
description (see below).

#### Resource Attributes description
Description of the resource data structure.

If defined in a named [Resource section](#def-resource-section), this data
structure **may** be referenced by other data structures using the resource
name.

##### Example

```apib
# Blog Post [/posts/{id}]
Resource representing **ACME Blog** posts.

+ Attributes
    + id (number)
    + message (string) - The blog post article
    + author: john@appleseed.com (string) - Author of the blog post
```

> **NOTE:** This data structure can be later referred as:
>
>     + Attributes (Blog Post)
>

#### Action Attributes description
Description of the default request message-body data structure.

If defined, all the [Request sections](#def-request-section) of the respective
[Action section](#def-action-section) inherits these attributes unless
specified otherwise.

##### Example

```apib
## Create a Post [POST]

+ Attributes
    + message (string) - The blog post article
    + author: john@appleseed.com (string) - Author of the blog post

+ Request (application/json)

+ Request (application/yaml)

+ Response 201
```

#### Payload Attributes description
Description of payload (request, response, model) message-body attributes.

Not every attribute has to be described. However, when an attribute is
described, it **should** appear in the respective
[Body section](#def-body-section) example, if a Body section is provided.

If defined, the [Body section](#def-body-section) **may** be omitted and the
example representation **should** be generated from the attributes description.

The description of message-body attributes **may** be used to describe
message-body validation if no [Schema section](#def-schema-section) is
provided. When a Schema section is provided, the attributes description
**should** conform to the schema.

##### Example

```apib
## Retrieve a Post [GET]

+ Response 200 (application/json)

    + Attributes (object)
        + message (string) - Message to the world

    + Body

            { "message" : "Hello World." }
```

---

<a name="def-headers-section"></a>
## Headers section
- **Parent sections:** [Payload section](#def-payload-section)
- **Nested sections:** none
- **Markdown entity:** list
- **Inherits from**: none

#### Definition
Defined by the `Headers` keyword in Markdown list entity.

    + Headers

#### Description
Specifies the HTTP message-headers of the parent payload section. The content
this section is expected to be a [pre-formatted code block](http://daringfireball.net/projects/markdown/syntax#precode)
with the following syntax:

    <HTTP header name>: <value>

One HTTP header per line.

#### Example

```apib
+ Headers

        Accept-Charset: utf-8
        Connection: keep-alive
        Content-Type: multipart/form-data, boundary=AaB03x
```

---

<a name="def-body-section"></a>
## Body section
- **Parent sections:** [Payload section](#def-payload-section)
- **Nested sections:** none
- **Markdown entity:** list
- **Inherits from**: [Asset section](#def-asset-section)

#### Definition
Defined by the `Body` keyword in Markdown list entity.

    + Body

#### Description
Specifies the HTTP message-body of a payload section.

#### Example

```apib
+ Body

        {
            "message": "Hello"
        }
```

---


<a name="def-data-structures"></a>
## Data Structures section
- **Parent sections:** none
- **Nested sections:** _MSON Named Type definition_ (see below)
- **Markdown entity:** header
- **Inherits from**: none

#### Definition
Defined by the `Data Structures` keyword.

    # Data Structures

#### Description
This section holds arbitrary data structures definitions defined in the form of
[MSON Named Types][].

Data structures defined in this section **may** be used in any [Attributes section][].
Similarly, any data structures defined in a [Attributes section][] of a named
[Resource Section][] **may** be used in a data structure definition.

Refer to the [MSON][] specification for full details on how to define an MSON Named type.

#### Example

```apib
# Data Structures

## Message (object)

+ text (string) - text of the message
+ author (Author) - author of the message

## Author (object)

+ name: John
+ email: john@appleseed.com
```

#### Example reusing Data Structure in Resource

```apib
# User [/user]

+ Attributes (Author)

# Data Structures

## Author (object)

+ name: John
+ email: john@appleseed.com
```

#### Example reusing Resource-defined Data Structure

```apib
# User [/user]

+ Attributes
    + name: John
    + email: john@appleseed.com

# Data Structures

## Author (User)
```

---

<a name="def-relation-section"></a>
## Relation section
- **Parent sections:** [Action section](#def-action-section)
- **Nested Sections:** none
- **Markdown entity:** list
- **Inherits from**: none

#### Definition
Defined by the `Relation` keyword written in a Markdown list item followed by a
colon (`:`) and a link relation identifier.

    + Relation: <link relation identifier>

#### Description
This section specifies a [link relation type](https://tools.ietf.org/html/rfc5988#section-4)
for the given action as specified by [RFC 5988](https://tools.ietf.org/html/rfc5988).

> **NOTE:** The link relation identifiers should be unique per resource in the blueprint document.

#### Example

```apib
# Task [/tasks/{id}]

+ Parameters
    + id

## Retrieve Task [GET]

+ Relation: task
+ Response 200

        { ... }

## Delete Task [DELETE]

+ Relation: delete
+ Response 204
```

---


<br>

<a name="def-appendix"></a>
# III. Appendix

<a name="def-uri-templates"></a>
## URI Templates

The API Blueprint uses a subset of [RFC6570][rfc6570] to define a resource URI Template.

### URI Path Segment

At its simplest form – without any variables – a path segment of an
URI Template is identical to an URI path segment:

```
/path/to/resources/42
```

### URI Template Variable

Variable names are case-sensitive. The variable name may consists of following
characters **only**:

- ASCII alpha numeric characters (`a-z`, `A-Z`)
- Decimal digits (`0-9`)
- `_`
- [Percent-encoded][pct-encoded] characters
- `.`

Multiple variables are separated by the comma **without** any leading or
trailing spaces. A variable(s) **must** be enclosed in braces – `{}`
**without** any additional leading or trailing whitespace.

#### Operators

The first variable in the braces **might** be preceded by an operator.
API Blueprint currently supports the following operators:

- `#` – _fragment identifier_ operator
- `+` – _reserved value_ operator
- `?` – _form-style query_ operator
- `&` – _form-style query continuation_ operator

#### Examples

```
{var}
{var1,var2,var3}
{#var}
{+var}
{?var}
{?var1,var2}
{?%24var}
{&var}
```

> **NOTE:** The [explode variable modifier][uri-explode] is also supported.
> Refer to RFC6570 for its description.

#### Variable Reserved Values

Following characters are **reserved** in variable _values_:

`:` / `/` / `?` / `#` / `[` / `]` / `@` / `!` / `$` / `&` / `'` / `(` / `)` / `*` / `+` / `,` / `;` / `=`

### Path Segment Variable

Simple path segment component variable is defined without any operator:

```
/path/to/resources/{var}
```

With `var := 42` the expansion is `/path/to/resources/42`.

> **NOTE:** RFC6570 – Level 1

### Fragment Identifier Variable

URI Template variables for fragment identifiers are defined using the
crosshatch (`#`) operator:

```
/path/to/resources/42{#var}
```

With `var := my_id` the expansion is `/path/to/resources/42#my_id`.

> **NOTE:** RFC6570 – Level 2

### Variable with Reserved Characters Values

To define URI Template variables with reserved URI characters,
use the plus (`+`) operator:

```
/path/{+var}/42
```

With `var := to/resources` the expansion is `/path/to/resources/42`.

> **NOTE:** RFC6570 – Level 2

### Form-style Query Variable

To define variables for a form-style query use the question mark (`?`) operator

```
/path/to/resources/{varone}{?vartwo}
```

With `varone := 42` and `vartwo = hello` the expansion is `/path/to/resources/42?vartwo=hello`.

To continue a form-style query use the ampersand (`&`) operator:

```
/path/to/resources/{varone}?path=test{&vartwo,varthree}
```

With `varone := 42`, `vartwo = hello`, `varthree = 1024` the expansion is `/path/to/resources/42?path=test&vartwo=hello&varthree=1024`.

> **NOTE:** RFC6570 – Part of Level 3

---

[apiblueprint.org]: http://apiblueprint.org
[markdown syntax]: http://daringfireball.net/projects/markdown
[reference syntax]: http://daringfireball.net/projects/markdown/syntax#link
[gitHub flavored markdown syntax]: https://help.github.com/articles/github-flavored-markdown
[httpmethods]: https://github.com/for-GET/know-your-http-well/blob/master/methods.md#know-your-http-methods-well
[uritemplate]: #def-uri-templates
[rfc6570]: http://tools.ietf.org/html/rfc6570
[HTTP status code]: https://github.com/for-GET/know-your-http-well/blob/master/status-codes.md
[header syntax]: https://daringfireball.net/projects/markdown/syntax#header
[list syntax]: https://daringfireball.net/projects/markdown/syntax#list
[pct-encoded]: http://en.wikipedia.org/wiki/Percent-encoding
[uri-explode]: http://tools.ietf.org/html/rfc6570#section-2.4.2
[examples]: https://github.com/apiaryio/api-blueprint/tree/master/examples

[MSON]: https://github.com/apiaryio/mson
[MSON Named Types]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#22-named-types
[MSON Type Definition]: https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md#35-type-definition

[`0-1` Attributes section]: #def-attributes-section
[Attributes section]: #def-attributes-section
[Attributes sections]: #def-attributes-section

[Resource Section]: #def-resource-section

[Request section]: #def-request-section
