# Google AIP


<!--more-->

## **AIP-1** AIP 目的和指南 {#AIP-1}

{{< admonition info >}}
翻译时间：2023-04-05
{{< /admonition >}}

随着谷歌 API 语料库的发展，以及 API 治理团队的壮大，以满足支持它们的需求，越来越有必要为 API 生产者、审查者和其他相关方提供文档语料库以供参考。API 风格指南和介绍性的 One Platform 文档有意简洁而高级。AIP 集合提供了一种为 API 设计指南提供一致文档的方法。

## **AIP-131** 标准方法：获取（Standard methods: Get） {#AIP-131}

{{< admonition info >}}
翻译时间：2023-04-05
{{< /admonition >}}

在 REST API 中，为了检索资源，通常会向资源的 URI（例如 `/v1/publisher/{publisher}/books/{book}`）发出 `GET` 请求。

面向资源的设计（[AIP-121](#AIP-121)）通过 `Get` 方法来尊重这种设计模式。这些 RPC 接受表示该资源的 URI 并返回该资源。

### 指导

API <font color="#c5221f">必须</font>为资源提供一个 get 方法。get 方法的目的是从单个资源返回数据。

Get 方法是使用以下模式指定的：

```protobuf
rpc GetBook(GetBookRequest) returns (Book) {
  option (google.api.http) = {
    get: "/v1/{name=publishers/*/books/*}"
  };
  option (google.api.method_signature) = "name";
}
```

- RPC 的名称<font color="#c5221f">必须</font>以 `Get` 一词开头。RPC 名称的其余部分<font color="#f57c00">应该</font>是资源消息名称的单数形式。
- 请求消息<font color="#c5221f">必须</font>与 RPC 名称匹配，并带有 `Request` 后缀。
- 响应消息<font color="#c5221f">必须</font>是资源本身。（没有 `GetBookResponse`。）
  - 响应通常应包括完全填充的资源，除非有理由返回部分响应（见 [AIP-157](#AIP-157)）。
- HTTP 谓词<font color="#c5221f">必须</font>是 `GET`。
- URI <font color="#f57c00">应该</font>包含与资源名称相对应的单个变量字段。
  - 此字段应称为 `name`。
  - URI <font color="#f57c00">应该</font>有一个与此字段相对应的变量。
  - `name` 字段<font color="#f57c00">应该</font>是 URI 路径中唯一的变量。所有剩余的参数都<font color="#f57c00">应该</font>映射到 URI 查询参数。
- `google.api.http` 注释中<font color="#c5221f">不能</font>有 `body` 键。
- <font color="#f57c00">应该</font>只有一个值为 `"name"` 的 `google.api.method_signature` 注释。

### 请求消息

Get 方法实现了一个通用的请求消息模式：

```protobuf
message GetBookRequest {
  // The name of the book to retrieve.
  // Format: publishers/{publisher}/books/{book}
  string name = 1 [
    (google.api.field_behavior) = REQUIRED,
    (google.api.resource_reference) = {
      type: "library.googleapis.com/Book"
    }];
}
```

- <font color="#c5221f">必须</font>包含资源名称字段。它<font color="#f57c00">应该</font>被称为 name。
  - <font color="#f57c00">应该</font>根据需要对字段进行[注释](#AIP-203)。
  - 字段<font color="#c5221f">必须</font>标识其引用的[资源类型](#AIP-123)。
- `name` 字段的注释<font color="#f57c00">应该</font>记录资源模式。
- 请求消息<font color="#c5221f">不得</font>包含任何其他必需字段，也<font color="#f57c00">不应</font>包含除其他 AIP 中描述的字段之外的其他可选字段。

{{< admonition note >}}
请求对象中的 `name` 字段对应于 RPC 上 `google.api.http` 注释中的 `name` 变量。因为在使用 REST/JSON 接口时，根据 URL 中的值填充 request 中的 `name` 字段。
{{< /admonition >}}

### 错误

请参阅[errors](#AIP-193)，特别是何时使用 `PERMISSION_DENIED` 和 `NOT_FOUND` 错误。

### Changelog

- 2023-03-17: Align with AIP-122 and make Get a must.
- 2022-11-04: Aggregated error guidance to AIP-193.
- 2022-06-02: Changed suffix descriptions to eliminate superfluous "-".
- 2020-06-08: Added guidance on returning the full resource.
- 2019-10-18: Added guidance on annotations.
- 2019-08-12: Added guidance for error cases.
- 2019-08-01: Changed the examples from "shelves" to "publishers", to present a better example of resource ownership.
- 2019-05-29: Added an explicit prohibition on arbitrary fields in standard methods.

## **AIP-132** 标准方法：列表（Standard methods: List） {#AIP-132}

{{< admonition info >}}
翻译时间：2023-04-05
{{< /admonition >}}

在许多 API 中，通常会向集合的 URI（例如 `/v1/publisher/1/books`）发出 `GET` 请求，以便检索资源列表，每个资源都位于该集合中。

面向资源的设计（[AIP-121](#AIP-121)）通过 `List` 方法来尊重这种设计模式。这些 RPC 接受父集合（可能还有一些其他参数），并返回与该输入匹配的响应列表。

### 指导

API <font color="#c5221f">必须</font>为资源提供 `List` 方法，除非资源是[单例](#AIP-156)。`List` 方法的目的是从有限集合返回数据（通常是单个集合，除非操作支持[跨集合读取](#AIP-159)）。

List 方法是使用以下模式指定的：

```protobuf
rpc ListBooks(ListBooksRequest) returns (ListBooksResponse) {
  option (google.api.http) = {
    get: "/v1/{parent=publishers/*}/books"
  };
  option (google.api.method_signature) = "parent";
}
```

- RPC 的名称<font color="#c5221f">必须</font>以单词 `List` 开头。RPC 名称的其余部分<font color="#f57c00">应该</font>是所列资源的复数形式。
- 请求和响应消息<font color="#c5221f">必须</font>与 RPC 名称匹配，并带有 `Request` 和 `Response` 后缀。
- HTTP 谓词<font color="#c5221f">必须</font>是 `GET`。
- 列出其资源的集合<font color="#f57c00">应该</font>映射到 URI 路径。
  - 集合的父资源<font color="#f57c00">应</font>称为 `parent`，并且<font color="#f57c00">应</font>是 URI 路径中唯一的变量。所有剩余的参数都<font color="#f57c00">应该</font>映射到 URI 查询参数。
  - 集合标识符（上例中的 books）<font color="#c5221f">必须</font>是一个文本字符串。
- `google.api.http` 注释中的 `body` 键<font color="#c5221f">必须</font>省略。
- 如果列出的资源不是顶级资源，那么<font color="#f57c00">应该</font>只有一个值为 `"parent"` 的 `google.api.method_signature` 注释。如果列出的资源是顶级资源，则<font color="#f57c00">应该</font>没有 `google.api.method_signature` 注释，或者只有一个值为 `""` 的 `google.api.method_signature` 注释。

### 请求消息

List 方法实现了一种常见的请求消息模式：

```protobuf
message ListBooksRequest {
  // The parent, which owns this collection of books.
  // Format: publishers/{publisher}
  string parent = 1 [
    (google.api.field_behavior) = REQUIRED,
    (google.api.resource_reference) = {
      child_type: "library.googleapis.com/Book"
    }];

  // The maximum number of books to return. The service may return fewer than
  // this value.
  // If unspecified, at most 50 books will be returned.
  // The maximum value is 1000; values above 1000 will be coerced to 1000.
  int32 page_size = 2;

  // A page token, received from a previous `ListBooks` call.
  // Provide this to retrieve the subsequent page.
  //
  // When paginating, all other parameters provided to `ListBooks` must match
  // the call that provided the page token.
  string page_token = 3;
}
```

- 除非列出的资源是顶级资源，否则<font color="#c5221f">必须</font>包括 `parent` 字段。它应该被称为 `parent`。
  - <font color="#f57c00">应</font>根据需要对字段进行[注释](#AIP-203)。
  - 字段<font color="#c5221f">必须</font>标识所列资源的[资源类型](#AIP-123)。
- <font color="#c5221f">必须</font>在所有列表请求消息上指定支持分页的 `page_size` 和 `page_token` 字段。有关更多信息，请参阅 [AIP-158](#AIP-158)。
  - `page_size` 字段上方的注释<font color="#f57c00">应</font>记录允许的最大值，以及省略（或设置为 `0`）字段时的默认值。如果首选(_If preferred_)，API <font color="#188038">可以</font>声明服务器将使用合理的默认值。此默认值<font color="#188038">可</font>会随着时间的推移而更改。
  - 如果用户提供的值大于允许的最大值，则 API <font color="#f57c00">应</font>将该值强制为允许的最大。
  - 如果用户提供了负值或其他无效值，则 API <font color="#c5221f">必须</font>发送 `INVALID_ARGUMENT` 错误。
- `page_token` 字段<font color="#c5221f">必须</font>包含在所有列表请求消息中。
- 请求消息<font color="#188038">可以</font>包括与列表方法相关的常见设计模式的字段，例如 `string filter` 和 `string order_by`。
- 请求消息<font color="#c5221f">不得</font>包含任何其他必需字段，也<font color="#f57c00">不应</font>包含除本 AIP 或其他 AIP 中描述的字段之外的其他可选字段。

{{< admonition note >}}
对于任何有权对集合成功发出 List 请求的用户，List 方法都<font color="#f57c00">应该</font>返回相同的结果。Search 方法在这方面比较宽松。
{{< /admonition >}}

### 响应消息

List 方法实现了一个通用的响应消息模式：

```protobuf
message ListBooksResponse {
  // The books from the specified publisher.
  repeated Book books = 1;

  // A token, which can be sent as `page_token` to retrieve the next page.
  // If this field is omitted, there are no subsequent pages.
  string next_page_token = 2;
}
```

- 响应消息<font color="#c5221f">必须</font>包括一个与返回的资源相对应的重复字段，并且<font color="#f57c00">不应</font>包括任何其他重复字段，除非在另一个 AIP（例如，[AIP-217](#AIP-217)）中描述。
  - 响应通常<font color="#f57c00">应</font>包括完全填充的资源，除非有理由返回部分响应（见 [AIP-157](#AIP-157)）。
- 支持分页的 `next_page_token` 字段<font color="#c5221f">必须</font>包含在所有列表响应消息中。如果有后续页面，则<font color="#c5221f">必须</font>设置它，如果响应表示最终页面，则<font color="#c5221f">不得</font>设置它。有关更多信息，请参阅 [AIP-158](#AIP-158)。
- 该消息<font color="#188038">可以</font>包括具有集合中的项数的 `int32 total_size`（或 `int64 total_size`）字段。
  - 该值<font color="#188038">可以</font>是一个估计值（如果是的话，字段<font color="#f57c00">应该</font>清楚地记录在文档中）。
  - 如果使用筛选，则 `total_size` 字段<font color="#f57c00">应</font>反映应用筛选后集合的大小。

### 排序

`List` 方法<font color="#188038">可以</font>允许客户端指定排序顺序；如果他们这样做了，那么请求消息<font color="#f57c00">应该</font>包含一个 `string order_by` 字段。

- 值<font color="#f57c00">应该</font>是以逗号分隔的字段列表。例如：`"foo,bar"`。
- 默认的排序顺序是升序。为了指定字段的降序，用户会附加一个 `" desc"` 后缀；例如：`"foo desc, bar"`。
- 语法中多余的空格字符是无关紧要的。`"foo, bar desc"`、`" foo , bar desc "`和 `"foo,bar desc"` 都是等价的。
- 子字段是用指定的。字符，例如 `foo.bar` 或 `address.street`。

{{< admonition note >}}
只有在确定需要的情况下才提供排序。以后随时可以添加排序，但删除它是一个破坏性的改变。
{{< /admonition >}}

### 过滤

`List` 方法<font color="#188038">可以</font>允许客户端指定筛选器；如果他们这样做了，那么请求消息应该包含一个 `string filter` 字段。过滤在 [AIP-160](#AIP-160) 中有更详细的描述。

{{< admonition note >}}
只有在确定需要的情况下才提供筛选。以后随时是可以添加筛选，但删除它是一个突破性的改变。
{{< /admonition >}}

### 软删除的资源

Some APIs need to "[soft delete](#AIP-135)" resources, marking them as deleted or pending deletion (and optionally purging them later).

APIs that do this should not include deleted resources by default in list requests. APIs with soft deletion of a resource should include a bool show_deleted field in the list request that, if set, will cause soft-deleted resources to be included.

一些 API 需要“[软删除](#AIP-135)”资源，将其标记为已删除或待删除（并可选择稍后清除）。

默认情况下，执行此操作的 API <font color="#f57c00">不应</font>在列表请求中包括已删除的资源。具有软删除资源的 API <font color="#f57c00">应</font>在列表请求中包括一个 `bool show_deleted` 字段，如果设置该字段，将导致软删除的资源被包括在内。

### 错误

请参阅[errors](#AIP-193)，特别是何时使用 `PERMISSION_DENIED` 和 `NOT_FOUND` 错误。

### 延伸阅读

- 有关分页的详细信息，请参阅 [AIP-158](#AIP-158)。
- 有关跨多个父集合的列表，请参见 [AIP-159](#AIP-159)。

### Changelog

- 2023-03-22: Fix guidance wording to mention AIP-159.
- 2023-03-17: Align with AIP-122 and make Get a must.
- 2022-11-04: Aggregated error guidance to AIP-193.
- 2022-06-02: Changed suffix descriptions to eliminate superfluous "-".
- 2020-09-02: Add link to the filtering AIP.
- 2020-08-14: Added error guidance for permission denied cases.
- 2020-06-08: Added guidance on returning the full resource.
- 2020-05-19: Removed requirement to document ordering behavior.
- 2020-04-15: Added guidance on List permissions.
- 2019-10-18: Added guidance on annotations.
- 2019-08-01: Changed the examples from "shelves" to "publishers", to present a better example of resource ownership.
- 2019-07-30: Added guidance about documenting the ordering behavior.
- 2019-05-29: Added an explicit prohibition on arbitrary fields in standard methods.

