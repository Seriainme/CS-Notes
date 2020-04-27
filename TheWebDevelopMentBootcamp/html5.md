# HTML

## html:5 basic

```html
<!DOCTYPE html>
<html>
<head>
    <!-- Our metedata goes here -->
    <title></title>
</head>
<body>
    <!-- Our content goes here -->
</body>
</html>
```

## basic tag
paragraph
```html
<h1></h1>
<h2></h2>
<p></p>
```

## list

```html
<!-- order list -->
<ol>
    <li>A</li>
    <li>B</li>
    <li>C</li>
</ol>

<!--un order list -->
<ul>
    <li>A</li>
    <li>B</li>
    <li>C</li>
</ul>
```

## div and span

```html
<!-- for style -->
<!-- div is just a container for grouping things together -->
<div></div>
<!-- span is for text in paragraph -->
<span></span>
```

## link

```html
<a href="http://"></a>
```

## table
```html
<table>
    <thead>
        <tr> <!-- table row-->
            <th>table head A</th>
            <th>table head B</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>A</td> <!-- table data -->
            <td>B</td>
        </tr>
    </tbody>
</table>
```

## forms 表单

```html
<form action="/my-form-submitting-page" method="get">

<!-- All our inputs go in here -->
<input type="text">
<input type="date">
<input type="color">
<input type="file">
<input type="checkbox">

</form>
```

## input

```html
<input type="radio" name = "gender" value = "male"> <!--单选-->
<input type="radio" name = "gender" value = "female"> <!--单选-->
<input type="radio" name = "gender" value = "other"> <!--单选-->
```

## label

```html
<label for="username">Username:</label>
<input type="text" id="username">
```
