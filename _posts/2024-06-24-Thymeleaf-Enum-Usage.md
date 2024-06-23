---
layout: post
title: Thymeeleaf에서 Enum 사용
subtitle: Thymeleaf
tags: [HTML, Thymeleaf]
comments: true
---

### Overview
Thymeleaf에서 TAG SELECT 내 데이터를 사용할때 하드코딩으로 하는 방법도 있지만, 아래 두가지 방법으로 Java Enum Type을 사용할 수 있다.
1. th:each를 사용
```html
<select class="form-select" id="dtl-poiItemCd" name="poiItemCd" th:field="${detail.ProductCd}">
    <option th:each="enumValue: ${T(com.example.common.ProductCd).values()}"
            th:value="${enumValue}" 
            th:text="${enumValue.label()}"
    />
</select>
```
> ${T(com.example.common.ProductCd.values())} 를 th:each로 지정 후 value, text를 위 형태로 사용한다.

<br>

2. Controller @ModelAttribute사용.
* JAVA
```java
@ModelAttribute("ProductCd")
public ProductCd[] productCds() {
    return ProductCd.values();
}
```
<br>
* HTML
```html
<select class="form-select" id="dtl-poiItemCd" name="poiItemCd" th:field="${detail.토}">
	<option th:each="productCd : ${ProductCd}" 
				  th:value="${productCd}" 
				  th:text="${productCd.label()}"/>
</select>
```

>> th:field 를 사용하여 해당 값을 세팅해준다.