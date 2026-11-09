---
id: boolean.md
title: "Filtering Explained"
summary: "Milvus provides powerful filtering capabilities that enable precise querying of your data. Filter expressions allow you to target specific scalar fields and refine search results with different conditions. This guide explains how to use filter expressions in Milvus, with examples focused on query operations. You can also apply these filters in search and delete requests."
---

# Filtering Explained

{{var.product_name}} provides powerful filtering capabilities that enable precise querying of your data. Filter expressions allow you to select entities based on field values and refine search results with different conditions. This guide explains how to use filter expressions in {{var.product_name}}, with examples focused on query operations. You can also apply these filters in search and delete requests.

## Basic operators

{{var.product_name}} supports several basic operators for filtering data:

- **Comparison Operators**: `==`, `!=`, `>`, `<`, `>=`, and `<=` allow filtering based on numeric or text fields.
- **Range and pattern filters**: `IN`, `LIKE`, `=~`, and `!~` match values, wildcard patterns, or regex patterns. For details about string patterns, refer to [Pattern Matching](https://milvus.io/docs/pattern-matching.md).
- **Arithmetic Operators**: `+`, `-`, `*`, `/`, `%`, and `**` are used for calculations involving numeric fields.
- **Bitwise Operators**: In {{var.product_name}} 3.0.0 and later, `&`, `|`, and `^` filter integer fields that encode multiple flags, such as permissions or status bits. For details, refer to [Basic Operators](https://milvus.io/docs/basic-operators.md#Bitwise-operators).
- **Logical Operators**: `AND`, `OR`, and `NOT` combine multiple conditions into complex expressions.
- **IS NULL and IS NOT NULL Operators**: Use `IS NULL` to select entities whose field value is NULL, or `IS NOT NULL` to select entities whose field value is non-NULL. These operators support scalar fields. Starting in {{var.product_name}} 3.0.3, they also support ordinary vector fields. For supported types, syntax, and examples, refer to [IS NULL and IS NOT NULL operators](basic-filtering-operators.md#is-null-and-is-not-null-operators).

### Example: Filtering by Color

To find entities with primary colors (red, green, or blue) in a scalar field `color`, use the following filter expression:

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter='color in ["red", "green", "blue"]'
```

```java
String filter = "color in [\"red\", \"green\", \"blue\"]";
```

```go
filter := "color in [\"red\", \"green\", \"blue\"]"
```

```rust
let filter = "color in [\"red\", \"green\", \"blue\"]";
```

```c++
std::string filter = "color in [\"red\", \"green\", \"blue\"]";
```

```javascript
const filter = 'color in ["red", "green", "blue"]';
```

```csharp
string filter = "color in [\"red\", \"green\", \"blue\"]";
```

```bash
filter='color in ["red", "green", "blue"]'
```

### Example: Filtering by Permission Bits

To find entities whose integer `permissions` field has the `SHARE` bit set, use the bitwise AND operator (`&`):

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter='(permissions & 4) == 4'
```

```java
String filter = "(permissions & 4) == 4";
```

```go
filter := "(permissions & 4) == 4"
```

```rust
let filter = "(permissions & 4) == 4";
```

```c++
std::string filter = "(permissions & 4) == 4";
```

```javascript
const filter = '(permissions & 4) == 4';
```

```csharp
string filter = "(permissions & 4) == 4";
```

```bash
filter='(permissions & 4) == 4'
```

### Example: Filtering by Regex Pattern

To find entities whose `message` field contains an error code such as `E1001`, use the regex match operator `=~`:

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter='message =~ "E[0-9]{4}"'
```

```java
String filter = "message =~ \"E[0-9]{4}\"";
```

```go
filter := "message =~ \"E[0-9]{4}\""
```

```rust
let filter = "message =~ \"E[0-9]{4}\"";
```

```c++
std::string filter = "message =~ \"E[0-9]{4}\"";
```

```javascript
const filter = 'message =~ "E[0-9]{4}"';
```

```csharp
string filter = "message =~ \"E[0-9]{4}\"";
```

```bash
filter='message =~ "E[0-9]{4}"'
```

Regex filters use substring matching. To require the entire field value to match the pattern, add `^` and `$` anchors. For details, refer to [Pattern Matching](https://milvus.io/docs/pattern-matching.md).

### Example: Filtering JSON Fields

{{var.product_name}} allows referencing keys in JSON fields. For instance, if you have a JSON field `product` with keys `price` and `model`, and want to find products with a specific model and price lower than 1,850, use this filter expression:

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter='product["model"] == "JSN-087" AND product["price"] < 1850'
```

```java
String filter = "product[\"model\"] == \"JSN-087\" AND product[\"price\"] < 1850";
```

```go
filter := "product[\"model\"] == \"JSN-087\" AND product[\"price\"] < 1850"
```

```rust
let filter = "product[\"model\"] == \"JSN-087\" AND product[\"price\"] < 1850";
```

```c++
std::string filter = "product[\"model\"] == \"JSN-087\" AND product[\"price\"] < 1850";
```

```javascript
const filter = 'product["model"] == "JSN-087" AND product["price"] < 1850';
```

```csharp
string filter = "product[\"model\"] == \"JSN-087\" AND product[\"price\"] < 1850";
```

```bash
filter='product["model"] == "JSN-087" AND product["price"] < 1850'
```

### Example: Filtering Array Fields

If you have an array field `history_temperatures` containing the records of average temperatures reported by observatories since the year 2000, and want to find observatories where the temperature in 2009 (the 10th recorded ) exceeds 23°C, use this expression:

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter='history_temperatures[10] > 23'
```

```java
String filter = "history_temperatures[10] > 23";
```

```go
filter := "history_temperatures[10] > 23"
```

```rust
let filter = "history_temperatures[10] > 23";
```

```c++
std::string filter = "history_temperatures[10] > 23";
```

```javascript
const filter = 'history_temperatures[10] > 23';
```

```csharp
string filter = "history_temperatures[10] > 23";
```

```bash
filter='history_temperatures[10] > 23'
```

For more information on these basic operators, refer to [Basic Operators](https://milvus.io/docs/basic-operators.md).

## Filter expression templates

When filtering using CJK characters, processing can be more complex due to their larger character sets and encoding differences. This can result in slower performance, especially with the `IN` operator.

{{var.product_name}} introduces filter expression templating to optimize performance when working with CJK characters. By separating dynamic values from the filter expression, the query engine handles parameter insertion more efficiently.

To find individuals over the age of `25` living in either `"北京"` (Beijing) or `"上海"` (Shanghai), use the following template expression:

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter = "age > 25 AND city IN ['北京', '上海']"
```

```java
String filter = "age > 25 AND city IN ['北京', '上海']";
```

```go
filter := "age > 25 AND city IN ['北京', '上海']"
```

```rust
let filter = "age > 25 AND city IN ['北京', '上海']";
```

```c++
std::string filter = "age > 25 AND city IN ['北京', '上海']";
```

```javascript
const filter = "age > 25 AND city IN ['北京', '上海']";
```

```csharp
string filter = "age > 25 AND city IN ['北京', '上海']";
```

```bash
filter="age > 25 AND city IN ['北京', '上海']"
```

To improve performance, use this variation with parameters:

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter = "age > {age} AND city in {city}"
filter_params = {"age": 25, "city": ["北京", "上海"]}
```

```java
import java.util.*;

String filter = "age > {age} AND city in {city}";
Map<String, Object> filterTemplateValues = new HashMap<>();
filterTemplateValues.put("age", 25);
filterTemplateValues.put("city", Arrays.asList("北京", "上海"));
```

```go
filter := "age > {age} AND city in {city}"
filterTemplates := map[string]any{
    "age":  25,
    "city": []string{"北京", "上海"},
}
```

```rust
use milvus::v2::prelude::*;
use std::collections::HashMap;

let filter = "age > {age} AND city in {city}";
let filter_templates: HashMap<String, FilterTemplateValue> = HashMap::from([
    ("age".into(), FilterTemplateValue::Int64(25)),
    ("city".into(), FilterTemplateValue::StringArray(vec!["北京".to_string(), "上海".to_string()])),
]);
```

```c++
std::string filter = "age > {age} AND city in {city}";
std::unordered_map<std::string, nlohmann::json> filterTemplates = {
    {"age", 25},
    {"city", nlohmann::json::array({"北京", "上海"})}
};
```

```javascript
const filter = "age > {age} AND city in {city}";
const exprValues = { age: 25, city: ["北京", "上海"] };
```

```csharp
using System.Collections.Generic;

string filter = "age > {age} AND city in {city}";
var filterTemplateValues = new Dictionary<string, object>
{
    ["age"] = 25,
    ["city"] = new[] { "北京", "上海" }
};
```

```bash
filter="age > {age} AND city in {city}"
filter_params='{"age": 25, "city": ["北京", "上海"]}'
```

This approach reduces parsing overhead and improves query speed. For more information, see <cite doc-id="TumJwDYrhiDYcUkKsUIcuSnbnCf" file-type="wiki" title="Filter Templating" type="doc"></cite>.

## Data type-specific operators

{{var.product_name}} provides advanced filtering operators for specific data types, such as JSON, ARRAY, and VARCHAR fields.

### JSON field-specific operators

{{var.product_name}} offers advanced operators for querying JSON fields, enabling precise filtering within complex JSON structures:

**`JSON_CONTAINS(identifier, jsonExpr)`**: Checks if a JSON expression exists in the field.

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
# JSON data: {"tags": ["electronics", "sale", "new"]}
filter='json_contains(tags, "sale")'
```

```java
// JSON data: {"tags": ["electronics", "sale", "new"]}
String filter = "json_contains(tags, \"sale\")";
```

```go
// JSON data: {"tags": ["electronics", "sale", "new"]}
filter := "json_contains(tags, \"sale\")"
```

```rust
// JSON data: {"tags": ["electronics", "sale", "new"]}
let filter = "json_contains(tags, \"sale\")";
```

```c++
// JSON data: {"tags": ["electronics", "sale", "new"]}
std::string filter = "json_contains(tags, \"sale\")";
```

```javascript
// JSON data: {"tags": ["electronics", "sale", "new"]}
const filter = 'json_contains(tags, "sale")';
```

```csharp
// JSON data: {"tags": ["electronics", "sale", "new"]}
string filter = "json_contains(tags, \"sale\")";
```

```bash
# JSON data: {"tags": ["electronics", "sale", "new"]}
filter='json_contains(tags, "sale")'
```

**`JSON_CONTAINS_ALL(identifier, jsonExpr)`**: Ensures all elements of the JSON expression are present.

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
# JSON data: {"tags": ["electronics", "sale", "new", "discount"]}
filter='json_contains_all(tags, ["electronics", "sale", "new"])'
```

```java
// JSON data: {"tags": ["electronics", "sale", "new", "discount"]}
String filter = "json_contains_all(tags, [\"electronics\", \"sale\", \"new\"])";
```

```go
// JSON data: {"tags": ["electronics", "sale", "new", "discount"]}
filter := "json_contains_all(tags, [\"electronics\", \"sale\", \"new\"])"
```

```rust
// JSON data: {"tags": ["electronics", "sale", "new", "discount"]}
let filter = "json_contains_all(tags, [\"electronics\", \"sale\", \"new\"])";
```

```c++
// JSON data: {"tags": ["electronics", "sale", "new", "discount"]}
std::string filter = "json_contains_all(tags, [\"electronics\", \"sale\", \"new\"])";
```

```javascript
// JSON data: {"tags": ["electronics", "sale", "new", "discount"]}
const filter = 'json_contains_all(tags, ["electronics", "sale", "new"])';
```

```csharp
// JSON data: {"tags": ["electronics", "sale", "new", "discount"]}
string filter = "json_contains_all(tags, [\"electronics\", \"sale\", \"new\"])";
```

```bash
# JSON data: {"tags": ["electronics", "sale", "new", "discount"]}
filter='json_contains_all(tags, ["electronics", "sale", "new"])'
```

**`JSON_CONTAINS_ANY(identifier, jsonExpr)`**: Filters for entities where at least one element exists in the JSON expression.

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
# JSON data: {"tags": ["electronics", "sale", "new"]}
filter='json_contains_any(tags, ["electronics", "new", "clearance"])'
```

```java
// JSON data: {"tags": ["electronics", "sale", "new"]}
String filter = "json_contains_any(tags, [\"electronics\", \"new\", \"clearance\"])";
```

```go
// JSON data: {"tags": ["electronics", "sale", "new"]}
filter := "json_contains_any(tags, [\"electronics\", \"new\", \"clearance\"])"
```

```rust
// JSON data: {"tags": ["electronics", "sale", "new"]}
let filter = "json_contains_any(tags, [\"electronics\", \"new\", \"clearance\"])";
```

```c++
// JSON data: {"tags": ["electronics", "sale", "new"]}
std::string filter = "json_contains_any(tags, [\"electronics\", \"new\", \"clearance\"])";
```

```javascript
// JSON data: {"tags": ["electronics", "sale", "new"]}
const filter = 'json_contains_any(tags, ["electronics", "new", "clearance"])';
```

```csharp
// JSON data: {"tags": ["electronics", "sale", "new"]}
string filter = "json_contains_any(tags, [\"electronics\", \"new\", \"clearance\"])";
```

```bash
# JSON data: {"tags": ["electronics", "sale", "new"]}
filter='json_contains_any(tags, ["electronics", "new", "clearance"])'
```

For more details on JSON operators, refer to <cite doc-id="Py6zwu6r4iPMqVkKAYXcUYLEnXg" file-type="wiki" title="JSON Operators" type="doc"></cite>.

### ARRAY field-specific operators

{{var.product_name}} provides advanced filtering operators for array fields, such as `ARRAY_CONTAINS`, `ARRAY_CONTAINS_ALL`, `ARRAY_CONTAINS_ANY`, and `ARRAY_LENGTH`, which allow fine-grained control over array data:

**`ARRAY_CONTAINS`**: Filters entities containing a specific element.

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter="ARRAY_CONTAINS(history_temperatures, 23)"
```

```java
String filter = "ARRAY_CONTAINS(history_temperatures, 23)";
```

```go
filter := "ARRAY_CONTAINS(history_temperatures, 23)"
```

```rust
let filter = "ARRAY_CONTAINS(history_temperatures, 23)";
```

```c++
std::string filter = "ARRAY_CONTAINS(history_temperatures, 23)";
```

```javascript
const filter = 'ARRAY_CONTAINS(history_temperatures, 23)';
```

```csharp
string filter = "ARRAY_CONTAINS(history_temperatures, 23)";
```

```bash
filter='ARRAY_CONTAINS(history_temperatures, 23)'
```

**`ARRAY_CONTAINS_ALL`**: Filters entities where all elements in a list are present.

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter="ARRAY_CONTAINS_ALL(history_temperatures, [23, 24])"
```

```java
String filter = "ARRAY_CONTAINS_ALL(history_temperatures, [23, 24])";
```

```go
filter := "ARRAY_CONTAINS_ALL(history_temperatures, [23, 24])"
```

```rust
let filter = "ARRAY_CONTAINS_ALL(history_temperatures, [23, 24])";
```

```c++
std::string filter = "ARRAY_CONTAINS_ALL(history_temperatures, [23, 24])";
```

```javascript
const filter = 'ARRAY_CONTAINS_ALL(history_temperatures, [23, 24])';
```

```csharp
string filter = "ARRAY_CONTAINS_ALL(history_temperatures, [23, 24])";
```

```bash
filter='ARRAY_CONTAINS_ALL(history_temperatures, [23, 24])'
```

**`ARRAY_CONTAINS_ANY`**: Filters entities containing any element from the list.

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter="ARRAY_CONTAINS_ANY(history_temperatures, [23, 24])"
```

```java
String filter = "ARRAY_CONTAINS_ANY(history_temperatures, [23, 24])";
```

```go
filter := "ARRAY_CONTAINS_ANY(history_temperatures, [23, 24])"
```

```rust
let filter = "ARRAY_CONTAINS_ANY(history_temperatures, [23, 24])";
```

```c++
std::string filter = "ARRAY_CONTAINS_ANY(history_temperatures, [23, 24])";
```

```javascript
const filter = 'ARRAY_CONTAINS_ANY(history_temperatures, [23, 24])';
```

```csharp
string filter = "ARRAY_CONTAINS_ANY(history_temperatures, [23, 24])";
```

```bash
filter='ARRAY_CONTAINS_ANY(history_temperatures, [23, 24])'
```

**`ARRAY_LENGTH`**: Filters based on the length of the array.

<div class="multipleCode">
    <a href="#python">Python</a>
    <a href="#java">Java</a>
    <a href="#go">Go</a>
    <a href="#rust">Rust</a>
    <a href="#cpp">C++</a>
    <a href="#javascript">NodeJS</a>
    <a href="#csharp">CSharp</a>
    <a href="#bash">cURL</a>
</div>
```python
filter="ARRAY_LENGTH(history_temperatures) < 10"
```

```java
String filter = "ARRAY_LENGTH(history_temperatures) < 10";
```

```go
filter := "ARRAY_LENGTH(history_temperatures) < 10"
```

```rust
let filter = "ARRAY_LENGTH(history_temperatures) < 10";
```

```c++
std::string filter = "ARRAY_LENGTH(history_temperatures) < 10";
```

```javascript
const filter = 'ARRAY_LENGTH(history_temperatures) < 10';
```

```csharp
string filter = "ARRAY_LENGTH(history_temperatures) < 10";
```

```bash
filter='ARRAY_LENGTH(history_temperatures) < 10'
```

For more details on array operators, see <cite doc-id="MaWywRYCniq6vwkJsT7c2wAyn0f" file-type="wiki" title="ARRAY Operators" type="doc"></cite>.

### VARCHAR field-specific operators

{{var.product_name}} provides specialized operators for precise text-based searches on VARCHAR fields:

#### Pattern matching operators

The `LIKE`, `=~`, and `!~` operators match string patterns on `VARCHAR` fields, JSON string paths, and specific `ARRAY<VARCHAR>` elements. Use `LIKE` for simple wildcard patterns. Use `=~` and `!~` for RE2 regular expressions.

For details, refer to [Pattern Matching](https://milvus.io/docs/pattern-matching.md).

#### `TEXT_MATCH` operator

The `TEXT_MATCH` operator allows precise document retrieval based on specific query terms. It is particularly useful for filtered searches that combine scalar filters with vector similarity searches. Unlike semantic searches, Text Match focuses on exact term occurrences.

{{var.product_name}} uses Tantivy to support inverted indexing and term-based text search. The process involves:

1. **Analyzer**: Tokenizes and processes input text.
2. **Indexing**: Creates an inverted index mapping unique tokens to documents.

For more details, refer to Text Match.

#### `PHRASE_MATCH` operator

The **PHRASE_MATCH** operator enables precise retrieval of documents based on exact phrase matches, considering both the order and adjacency of query terms.

For more details, refer to <cite doc-id="O2YiwLai5iSjT1k1WEsc06E8nEe" file-type="wiki" title="Phrase Match" type="doc"></cite>.