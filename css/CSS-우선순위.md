# CSS 우선순위

```
!important > id [ 100 ] > class [ 10 ] > tag [ 1 ] > * [ 0 ]
```

우선순위에 따라 어떤 css가 표시되어질지 결정된다.

 `!important` 를 붙이게 되면 무조건 적으로 우선시 되고 나머지 경우는 계산해서 선택하게 된다.

```html
<div class='my-div'>
  <p>
    안녕하세요.
  </p>
</div>
```

html

```css
.my-div p {
  color: blue;
}

p{
  color: red;
}
```

css

일 때,  .my-dev (10점) +  p tag (1점) > p tag(10점) 이므로 `안녕하세요.` 는 파란색이 보여지게 된다.

